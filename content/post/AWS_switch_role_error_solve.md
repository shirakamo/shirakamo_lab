---
title: "【エラー解決】AWSでスイッチロールできない問題を解決"
date: 2024-05-03T20:21:51+09:00
draft: false
description: "AWSで新たに作成したロールにスイッチできない問題を解決するまでのメモ"
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1714742249/shirakamo_lab_tech_blog/AWS_switch_role_erorr_solve/blog_thumbnails_becdnx.webp"
categories:
- "エンジニアリング"
tags:
- "AWS"
---

AWSで新たにロールを作成した際、管理者アカウントからスイッチロール出来ないという問題が発生しました。  
本記事ではこの問題を解決するまでに調べたこと、やったことをまとめます。  

## 発生した問題

AWSに管理者（Admin）アカウントでログインし、IAMで新たにデータ分析目的のロールを作成しました。  
ここでは新たに作成したロールをデータ分析ロールと呼ぶことにします。  
その際データ分析ロールに設定したポリシーは以下です。  

- `AmazonSagemakerFullAccess`
- データ分析用に用意したS3バケットのRead/Write権限諸々

ロールを作成した後にAWSコンソールからデータ分析ロールにスイッチしようとしたところ出来ない、という問題に直面しました。  
AWSで作成したロールにスイッチする際は以下の画面に必要事項を入力します。  
（前とUIが変わった気がする...？）  
<br>

![AWS_switch_role](https://res.cloudinary.com/dda9f1d6p/image/upload/v1714742248/shirakamo_lab_tech_blog/AWS_switch_role_erorr_solve/AWS_switch_to_role_kxyqq6.webp "AWS_switch_role")  

<br>

上記にスイッチ元となるAdminのアカウントIDと、スイッチ先となるデータ分析ロールのロール名を入力したのですが、  
正しい値を入力しているにも関わらず  

```
1つ以上のフィールドに無効な情報があります。情報を確認するか、管理者に連絡してください。
```

と表示されてしまいました。  

## 原因調査

Gemini様にお聞きするなどして調べてみたところ、どうもスイッチロールのために追加でポリシーを設定する必要がありそうでした。  
<br>

[AWS マネジメントコンソールを使用して IAM ロールを切り替える際の問題を解決するにはどうすればよいですか?](https://repost.aws/ja/knowledge-center/iam-switch-role "iam_switch_role_repost")

<br>
上記で示されている対処法のうち、上の2つ

1. AssumeRoleアクションの許可が正しくない
2. IAM信頼ポリシーが正しくない

に記載されている対処をしたところ解決しました。  

### AssumeRoleアクションの許可

上記のURLに記載されている通りに、データ分析ロールの許可ポリシーに以下のポリシーをインラインポリシーとして手入力で追加しました。  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::<アカウントID No.>:role/<スイッチ先のRole名>"
        }
    ]
}
```

### IAM信頼ポリシー

データ分析ロールのIAM画面において、 `信頼関係` というタブで信頼ポリシーに以下を追加しました。  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            <元から設定していた信頼ポリシー>
        },
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<アカウントID No.>:root"
            },
            "Action": "sts:AssumeRole",
            "Condition": {}
        }
    ]
}
```

`Statement` の2つ目の要素が今回新たに追加した要素です。

上記2点の修正によってデータ分析ロールへのスイッチは出来るようになりました。  

## 新たな問題

データ分析ロールへのスイッチはできるようになったのですが、  
スイッチした先のマネジメントコンソールで以下のエラーが表示されてしまいました。  

```
許可が必要です
User: arn:aws:sts::<アカウントID No.>:assumed-role/<スイッチ先ロール名>/<スイッチ元ロール名> is not authorized to perform: access-analyzer:ListPolicyGenerations on resource: arn:aws:access-analyzer:us-east-1:<アカウントID No.>:*
```

どうもIAM Access Analyzerのポリシーを付与する必要がありそうです。  
ピンポイントで権限を付与する方法がまだ分からず...  
仕方ないのでIAM Access AnalyzerのFull権限を設定することにしました。  

以下のポリシーを追加で設定しました。  

- `IAMAccessAnalyzerFullAccess`

この対処をしたところエラー表示はなくなりました。  

## 今後

これにてスイッチロールに関する問題は解決したようです。  
ただ実際に分析を進めようとすると諸々他にも問題が起きそうな気が...  
さらに問題が起きて解決出来たら本記事に追加するか別の記事を書いていこうと思います。  

また、今回IAM Access AnalyzerについてFullAccess権限を与えて対処しました。  
おそらくこれは不必要な権限も与えてしまっているのではないかと思います。  
そもそも最初に与えたSageMakerのFullAccess権限はかなり広い範囲の権限になっていると思うので、そちらも今後不必要な権限を見定めて削除していきたいと思います。
