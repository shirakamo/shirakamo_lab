---
title: "【Googleデータポータル入門第1回】Googleデータポータルのセットアップ&データの取り込み"
date: 2023-02-10T18:49:24+09:00
draft: false
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1676024035/shirakamo_lab_tech_blog/google_dataportal_setup/google_dataportal_setup_thumbnails_bnnovg.webp"
categories:
- "データ分析"
tags:
- "Googleデータポータル"
---

[Googleデータポータル](https://marketingplatform.google.com/intl/ja/about/data-studio/ "google_dataportal")を使ってみたところ簡単に使えてとても便利だったため、使い方を何回かに分けて説明します。  
この記事では実際に分析を行う準備として、Googleデータポータルのセットアップとサンプルデータの取り込みまでを説明します。

## Googleデータポータルとは

Googleデータポータルとは、様々なデータをブラウザ上で分析、可視化し、レポートとしてまとめることができるBIツールです。  
Googleが提供するサービスなので、GmailやGoogleドライブと同様、ブラウザだけで作業を行うことができます。  

また、特筆すべきはなんと言っても __無料であるということ。__ 素晴らしいですね。  

類似のツールとしては有名なものでは Tableau、Qlik sense などがあります。  
これらは基本有料であるため、なかなか個人では手を出せないかもしれません（無料で使える部分もあります）。  

他には Microsoft の Power BI / Power BI Desktop があります。  
Power BI Desktop であれば個人でインストールして使用する分には無料なので、Googleデータポータルに近いと言えるかもしれません。  
ただし、Power BI Desktopでレポートの共有機能を使うは有料版が必要です。  

Power BI Desktop は自分のコンピュータ上で動作し、Googleデータポータルはウェブ上で動作するという違いがあります。  
どちらも素晴らしいツールですが、ウェブ上で動作するGoogleデータポータルはアカウントさえ持っていれば別のコンピュータからも同じ作業ができる、という利点があります。  
自分がどのように使いたいかでツールを選択すると良いでしょう。  

この連載ではサンプルデータを用いながらGoogleデータポータルの使い方を簡単に説明します。この記事を読んで皆様も身の回りのデータを分析してみてくださいね。  

## サンプルデータの準備

今回は簡単にGoogleスプレッドシートとしてデータを用意し、Googleデータポータルで分析、可視化してみます。  
そのためにまずはデータをGoogleドライブに用意します。  

サンプルデータをGoogleドライブに格納し、Googleスプレッドシートとして開けるようにしておきます。

1. Googleドライブ上に分析対象のファイル (csv, tsv, エクセルファイル) をアップロードします。
    - 今回はKaggle上のデータセット [Movies on Netflix, Prime Video, Hulu and Disney+](https://www.kaggle.com/ruchi798/movies-on-netflix-prime-video-hulu-and-disney "movie") を題材として分析してみます。  
    このデータを使う場合はリンク先の "Download" をクリックするとzipがダウンロードされるので、適当なディレクトリで展開します。  
    その後、展開したcsvファイルをGoogleドライブ上の任意のフォルダにアップします。

1. Googleドライブ上で分析対象のファイルを右クリック → "アプリで開く" → "Googleスプレッドシート" を選択して、スプレッドシート上でファイルを開きます。  
自動でスプレッドシートが保存されます。

## Googleデータポータルにログイン

Googleデータポータルにログインする方法について説明します。  
ここで説明する内容はログインする最初の一回のみ必要な手順です。

1. Googleアカウントを持っていない場合は作成しておきます。

1. [Googleデータポータル](https://marketingplatform.google.com/intl/ja/about/data-studio/ "google_dataportal") ページを開き、右上の "データポータルにログイン" ボタンをクリックします。

1. "Google データポータルへようこそ" という画面が出てくるので、 "使ってみる" をクリックします。

1. 国名と使用許諾へのチェックを入れる画面が開く。国を選択し、使用許諾へチェックを入れます。
    - "日本"がすごい下の方にあります。

1. おすすめ情報などのメールを受け取るか選択する画面になります。  
欲しい情報がある場合はお好みの情報の "はい" にチェックを入れて下さい。

1. これでGoogleデータポータルの画面が開きます。  

テンプレートが用意されていますね。自分でレポートを作成する際はこのテンプレートを用いたほうが良さそうです。  
![dataportal home](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676024035/shirakamo_lab_tech_blog/google_dataportal_setup/dataportal_home_arheh8.webp "dataportal_home")

## Googleデータポータルへのデータの取り込み

Googleデータポータルにデータを取り込みます。  
今回は先程作成したGoogleスプレッドシートからデータを取り込みます。  
他にも様々な方法でデータを取り込むことが可能なので、使いたい方法をググってみて下さい。  

個人的には Google Analyticsや Big Query が気になりました。  
巨大なファイルはBig Queryに取り込んでおけばGoogleデータポータルで好きに分析できるようになりそうですね。  
他にはこのブログのアクセスが伸びてきたら Google Analytics のデータを取り込んでサイト分析してみても面白そうです。

1. 先程のログイン画面左上の "作成" ボタン → "データソース" をクリックします。

1. たくさんのデータ取り込みのための接続先が表示されます。  
ここから必要な接続先を選びデータをデータポータルに取り込みます。  
今回はこの中から "Googleスプレッドシート" を選びます。  
実際は画像より下にたくさんの3rd partyの接続先が表示されています。  
![all data sources](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676024035/shirakamo_lab_tech_blog/google_dataportal_setup/data_sources_all_tzkjhi.webp "all_data_sources")  

1. Googleドライブへのアクセスの許可を求められたら許可します。  
すると、画面上で先ほど作ったスプレッドシートが選択できるようになっているはずです。  
![dataportal spreadsheet](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676024035/shirakamo_lab_tech_blog/google_dataportal_setup/dataportal_source_spreadsheet_l9xsyp.webp "dataportal_spreadsheet")  

1. スプレッドシートを選択すると、ワークシート選択ができるようになります。  
ワークシートを選択すると右上の接続ボタンがクリックできるようになるので、クリックします。  
![spreadsheet connect](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676024035/shirakamo_lab_tech_blog/google_dataportal_setup/dataportal_source_spreadsheet_connect_txg9sc.webp "connect")  
    - 今回のスプレッドシートはcsvから作成したので、ワークシートは1つしかありません。  
    複数ワークシートがある場合は必要なものを選択して下さい。

1. データに接続すると、諸々の設定を行う画面に移ります。  
![settings](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676024035/shirakamo_lab_tech_blog/google_dataportal_setup/dataportal_settings_ze5imt.webp "settings")  
今回はオープンデータでただのスプレッドシートなので何も設定しなくても良いです。  
他のデータソーズの場合は気にする必要がありそうなので、以下に設定ポイントを挙げます。  
ちなみにここで設定しなくともあとで同じ画面を開いて設定することができます。
    - __データの更新間隔__ : データの取得先からどれくらいの時間間隔でデータを取得するかを設定します。  
    取得先が有料サービスの場合（BIG Queryなど）はデータを取得する度に料金がかかる可能性があるので、注意したほうが良さそうです。
    - __コミュニティにおける視覚化へのアクセス__ : [コミュニティビジュアライゼーション](https://support.google.com/datastudio/answer/9206527?hl=ja "community_visualization") 機能を使用するか否かを設定します。  
    この機能は今はβ版です。第三者が作成した可視化用の設定やアプリを使えるようにするかどうかのようです。  
    自分のデータがサードパーティ側にも表示される？ようになるっぽいので、使用する場合は認定されたディベロッパーのビジュアライゼーションのみを使うのが無難そうです。
    - __レポートのフィールド編集__ : 元になっているデータの定義（整数か、文字列かなど）をグラフごとなどで変更できるようにするかどうかを設定します。  
    特に問題がない場合は柔軟な集計や可視化ができるのでオンにしておいた方が良さそうです。
    - 真ん中の画面でデータの定義やデフォルトの集計方法を変更できます。  
    IDなど、整数だけど文字列として扱いたいなどの場合があれば変更します。

1. これでレポートを作成する準備が整いました。  
右上の "レポートを作成" をクリックすれば様々な可視化を駆使したレポートを作成できます。

今回の記事はここまでです。次回から実際にデータ分析を行ってみます。
