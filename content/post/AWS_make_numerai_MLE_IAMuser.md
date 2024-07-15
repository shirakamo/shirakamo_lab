---
title: "AWSでNumeraiのデータ分析用IAMユーザを用意してみる"
date: 2024-05-06T22:54:46+09:00
draft: false
description: "AWSでNumeraiの分析を行うIAMユーザを作成するまでの作業ログ"
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1715004998/shirakamo_lab_tech_blog/AWS_make_numerai_MLE_IAMuser/AWS_make_numerai_MLE_IAMuser_dtyxq3.webp"
categories:
- "エンジニアリング"
tags:
- "AWS"
- "Numerai"
---

Numeraiというデータ分析コンペに参加するため、AWSで分析を行うためのユーザ設定手順メモです。  
個人での利用を想定しています。  

最終的には定期的に予測を提出するためのシステムまで構築したいところですが、  
今回はデータ分析、機械学習モデル構築のためのユーザ設定（IAMユーザ作成）のみについて記載します。  

実は、NumeraiからAWSで予測提出自動化システムの構築方法は提供されています。  
しかし、ここでは学習目的で自力で予測提出自動化システムを組むことにします。

正直このような作業に慣れていないので...  
色々間違いだったり無駄だったりあると思いますが、温かい目で見守ってください。  
今後新たな設定を加えたり既存の設定を修正した時は本記事も更新していきます。

## Numerai用に必要なサービス

まずはNumeraiの分析と予測用のシステム構築を進めるためのに必要なサービスを洗い出します。  
今回作成するIAMユーザは1つですが、ここでは後の分かりやすさのために役割ごとに分割して洗い出します。  

| 役割 | サービス | 用途 |
| --- | --- | --- |
| データ分析 | S3 | データ格納 |
| | Athena | 探索的データ分析 |
| 機械学習モデル開発 | SageMaker | 機械学習モデルの構築など |
| | CodeCommit | モデル開発コード管理 |
| | ECR | 予測パイプラインコンテナ格納 |
| システム開発 | Lambda | 予測パイプライン呼び出し |
| | ECS | 予測パイプライン実行 |
| | CloudWatch | ログ管理 |

一旦はこんなところでしょうか...  
今後やっていく中で変わっていくと思うので今後もアップデートしていきます。  
（バッチ予測のベストプラクティスが分からない...）

本記事では機械学習モデル開発回りの権限設定を環境設定までを行います。

## AWSアカウント作成

まずはAWSアカウントを作成します。  
この辺りはAWSの説明がリッチなのでそちらをご参照ください。  

[AWS アカウント作成の流れ](https://aws.amazon.com/jp/register-flow/ "aws_create_account")  

ちなみにアカウント作成の最後に有償のサポートプランに入るかどうか聞かれるます。  
おそらく本記事を見ていらっしゃる方は個人での利用を考えていらっしゃるかと思うので、  
無料のベーシックサポートで良いと思います。  

（この円安でドル建ての支払いがツライ...）  

## ルートユーザのセキュリティ対策

先の手順でアカウントを開設すると __"ルートユーザ"__ と呼ばれる最初のユーザが作成されます。  
ルートユーザはデータ分析、アプリケーションの作成、さらにはアカウントの削除など何でもできるユーザです。  
こんなユーザが不正利用されては大変なので必要なセキュリティ対策を講じておきましょう。  

基本的には以下のドキュメントで説明されている事項を実施します。  

[AWS アカウントのルートユーザーのベストプラクティス](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/root-user-best-practices.html#ru-bp-mfa "root_security")  

個人での利用だと基本的に AWS Organizations は利用しないと思うので、以下を実施することになるかと思います。  

- 多要素認証（MFA）の設定
- アカウント回復メカニズムの設定
- 日常的なタスクをこなすための管理者ユーザを作成し、管理者ユーザで作業する

最後の管理者ユーザについては次で説明します。

## Adminユーザ作成

個人利用の場合、管理者ユーザ（Adminユーザ）は [IAM](https://aws.amazon.com/jp/iam/ "iam") という機能して __"IAMユーザ"__ を作成するので十分だと思います。  
企業など大規模に利用する場合は [AWS Organizations](https://aws.amazon.com/jp/organizations/ "aws_organizations")  を利用することも検討しても良いかもです。  
ただ、私がOrganizationsは使ったことがないので、ここでは解説しません（というかできません...）。  

Adminユーザの作成方法は以下で解説されています。  

[ステップ 1: AWS アカウントを設定して管理者ユーザーを作成する](https://docs.aws.amazon.com/ja_jp/streams/latest/dev/setting-up.html "set_up_admin_user")  

上記の作業手順にもありますが、IAMのベストプラクティスの一つとして権限を付与したグループを作成してユーザをそのグループに追加するようにする、というものがあります。  
こちらの方が個々のユーザにいちいち権限を振るよりは楽かつミスも少ない方法ですね。  

また、AdminユーザについてもMFAを設定した方がセキュリティ面でより安全です。  

[仮想 Multi-Factor Authentication (MFA) デバイスの有効化 (コンソール)](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html "AWS_MFA")  

Adminユーザの作成＆設定が終わったらrootユーザからログアウトしてAdminユーザでログインし直します。  

## Numerai用のリソース作成

以下のリソースをAdminユーザで作成します。

- Numerai Tournament用のS3バケット
- SageMaker
- Numerai Tournament用のCodeCommitリポジトリ

### S3バケット

マネジメントコンソールからS3コンソールを開き、適当な名前でバケットを作成します。  
S3のバケット名はリージョンで一意である必要があります。  
そのため、S3バケット名にアカウント名を入れることで重複を防ぐなどの対策が必要です。  
ただし、公開するS3バケット名にアカウント名が入っているのはセキュリティ的に不安があるので、  
その際は何かしら別の命名規則を用いたほうが良さそうです。  

[バケットの作成](https://docs.aws.amazon.com/ja_jp/AmazonS3/latest/userguide/create-bucket-overview.html "create_bucket")  

後でNumerai用のIAMユーザを作成し、そのIAMユーザから作成したS3バケットにアクセスする必要があります。  
そのため、作成したS3バケットへのアクセスを許可するIAMポリシー を作成します。  

[Amazon S3のアクセスに必要な最低限のIAMポリシーの設定](https://www.jpcyber.com/support/minimal-iam-policy-to-access-amazon-s3 "s3_policy")

ここでは作成したポリシー名を `numerai-s3-bucket-access` としました。

### SageMakerセットアップ

AdminユーザでマネジメントコンソールからSageMakerコンソールに移動し、 `ドメイン > ドメインの作成` からドメインを作成します。  
ドメインの作成時は `シングルユーザ向けの設定（クイックセットアップ）` と `組織向けの設定` の2つから選択できます。  
ここでは簡易にセットアップするためにシングルユーザ向けの設定でセットアップします。  
基本画面の指示に従う形で設定できるはずです。  

ドメインを作成出来たら、 `ドメイン > ユーザープロファイル > ユーザを追加` からユーザープロファイルを追加します。  
こちらも基本画面の指示に従うだけです。  

### CodeCommitリポジトリ

こちらもマネジメントコンソールからCodeCommitコンソールを開き、適当な名前でリポジトリを作成します。  

[AWS CodeCommit リポジトリを作成する](https://docs.aws.amazon.com/ja_jp/codecommit/latest/userguide/how-to-create-repository.html "create_repository")

CodeCommitリポジトリについても、後で作成するNumerai用のIAMユーザからアクセスできるよう認証情報を設定する必要があります。  
こちらについてはIAMユーザに関する設定で行うので、次節で設定します。  

## Numerai用のIAMユーザ作成～ポリシー設定まで

まずはNumerai用のIAMグループを作成し、そこに必要な権限を設定します。  
その後、IAMユーザを作成し、Numerai用のIAMグループに所属させるという形でIAMユーザに権限を与えます。  

### IAMグループ作成、権限付与

マネジメントコンソールからIAMコンソールを開き、 `ユーザーグループ > グループの作成` を選択し、適当なグループ名を付けてグループを作成します。  
さらにポリシーのリストから付与する権限のポリシーを選択してグループを作成します。  

[IAMユーザーグループの作成](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_groups_create.html "create_iam_group")  

付与した権限は以下です。  

- `AmazonSageMakerFullAccess`
- `AWSCodeCommitPowerUser`
- `numerai-s3-bucket-access`
  - S3バケットを作成した際に作成したポリシー

今後Lambdaなどのポリシーも追加していく必要がありますが、  
今の段階ではSageMakerで分析、CodeCommitでコード管理、S3でデータなどを蓄積という分析のワークアラウンドが実行できることを目指しているのでこの設定としました。  

### IAMユーザ作成

こちらもIAMコンソールに行き、 `ユーザー > ユーザーの作成`  からIAMユーザを作成します。  
その後、ユーザーグループから先ほど作成したユーザグループの設定を開き、今作成したIAMユーザをIAMグループに追加します。  

[AWSアカウントでのIAMユーザーの作成](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_users_create.html "create_iam_user")  
[IAMグループへのユーザーの追加と削除](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_groups_manage_add-remove-users.html "add_iam_user_to_group")

最後にIAMユーザに先ほど作成したCodeCommitリポジトリの認証情報を設定してHTTPS経由でリポジトリへ接続できるようにします。  
IAMコンソールから、今作成したIAMユーザを選択し、 `セキュリティ認証情報 > AWS CodeCommit の HTTPS Git 認証情報` にて認証情報を生成します。  
ここで生成されるパスワードは一度しか表示されないため、手元に控えておくか、 Download credentials からCSV形式で認証情報をダウンロードして安全な所に格納しておきます。  

<br>

以上の作業が終了したらAdminユーザからログアウトしておきます。  
この後の作業は今作成したNumerai用IAMユーザで行います。

ただし、権限設定回りで何か誤りがあった場合や、権限の追加が必要な場合は再度Adminユーザでログインして作業する必要があります。  

## Numerai用IAMユーザでSageMakerログイン～リポジトリのクローンまで

ここからは先に作成したNumerai用のIAMユーザでログインして作業します。  

Numerai用のIAMユーザのマネジメントコンソールからSageMakerコンソールに移動し、Adminユーザの時に立ち上げたSageMakerドメインの詳細を開きます。  
（Numerai用IAMユーザに適切にSageMakerの権限が付与できていなければこの段階で失敗します。）  

`ドメイン > 作成したドメイン > ユーザープロファイル > 作成したユーザー > 起動 > Studio` からSageMaker Studioを開きます。  

SageMaker Studioでは作業環境としてJupyterLabとCode Editor（実質VSCodeです）が選べます。  
今回は使い慣れているVSCodeで作業します。  

画面からCode Editorを選択し、`+Create Code Editor space` からCode Editorを立ち上げます。  

立ち上げたSpaceのRunを選択するとVSCodeの画面が出てくるので、そこで作業していきます。  
まずはリポジトリを作業ディレクトリにクローンします。  

先に作成したCodeCommitリポジトリを開き、クローン用のURL（HTTPS）をコピーします。  

VSCodeのターミナルを開き、 `git clone <HTTPS URL>` でリポジトリをクローンします。  
このとき、IAMユーザを作成する際に取得したCodeCommitの認証情報を使い、ユーザ名とパスワードを入力します。  

権限設定やCodeCommit認証情報が適切に設定されていれば作業ディレクトリにリポジトリがクローンされたはずです。  

## 今後の作業

- SageMakerでNumeraiのデータをダウンロード->S3バケットに保存
- Numeraiのサンプルコードを利用してベースラインモデルを作成
- ベースラインモデルを元に予測値自動提出システム構築
