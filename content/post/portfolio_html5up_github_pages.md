---
title: "HTML5 UPとGithub Pagesを利用したポートフォリオサイト作成"
date: 2023-02-10T23:40:33+09:00
description: "HTML5 UPを利用してポートフォリオサイトを作成し、Github Pagesで公開します"
draft: false
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1676041565/shirakamo_lab_tech_blog/portfolio_html5up_github_pages/blog_thumbnails_k7h5v1.webp"
categories:
- "エンジニアリング"
tags:
- "サイト制作"
---

HTML5 UPのテンプレートとGithub Pagesを利用して自分のポートフォリオサイトを作成、公開した作業のまとめです。  
N番煎じも良いところでしょうが...個人的な作業ログとして残しておきます。  

以下のサイトを参考にさせていただきました。というか、ほぼこちらのサイトに従って作業させてもらいました。  

- ポートフォリオサイト作成全般について  

[GitHub Pagesで自作portfolioを公開して、Google Analyticsでアクセス情報などを収集するようにした話](https://qiita.com/shota-imazeki/items/0bfcf21951d160518f15 "portfolio_example")

- Google Analyticsの設定について  

[Googleアナリティクスの導入・設定方法と活用のポイント【初心者向け】](https://wacul-ai.com/blog/access-analysis/google-analytics-setting/ga-beginner/ "ga_example")  

サイト制作は初めてだったのですが、Github Pagesの設定で少し詰まったくらいで大体スムーズに公開できました。  
作成したサイトはこちらです。

- [Shirakamo's Portfolio](https://shirakamo.github.io/portfolio_github_io/ "portfolio site")

HTML5 UP + Github Pagesという組み合わせは __「簡単で良いからウェブサイトを作りたい」__ という方にぴったりだと思います。  
以下に作業した順番で内容を書いていきます。  

## 1.HTML5 UPからテンプレートをダウンロードする

[HTML5 UP](https://html5up.net/ "html5up")とはウェブサイトのテンプレートを集めたサイトです。  
以下のような特徴があります。

- ダウンロード後即公開可能なHTML, CSS, Javascriptセットのテンプレート集
- PC、タブレット、スマホに対応したレスポンシブデザイン
- 商用利用可能なCreative Commons3.0ライセンス
  - フッターの `HTML5UP` のクレジットを残しておけば編集可能

ほぼ自由に編集可能、全部入り、センスの良いテンプレートということでサイト制作の経験が無い身としては神かと思えますね。  
レスポンシブ対応もありがたいです。ちなみに各テンプレートの `Live Demo` ボタンから各デバイスでどう見えるかチェックできます。  

今回は初めてなのでシンプルなものにしたく、 [Miniport](https://html5up.net/miniport "miniport") というテンプレートを選びました。  
今後より手の込んだものにしたくなったら別テンプレートも試してみようと思います。  

各テンプレートの `Download` ボタンを押すとテンプレートを構成するコード一式（HTML, CSS, Javascript, 画像その他）がzipでダウンロードされます。  
展開してどこか作業用のディレクトリに格納しておきます。このディレクトリは後でgitで管理します。  

## 2.テンプレートを修正してサイト作成

基本的にはダウンロードしたファイルに含まれる `index.html` をいじっていけば良いです。  
複数ページがあるテンプレートの場合はそれに対応する `xxxx.html` ファイルをいじるのではないかと思います。  

文章を修正する程度ならば、公開済みのテンプレートとHTMLファイルを見比べて修正箇所を見つけるという方法でも行けるような気がします。  
ただ、HTML、CSSの知識があったほうが良いのは間違いないので、その部分は書籍やオンラインコースなどで学ぶと良いかもしれません。  
ここではオンラインコースとして [Progate](https://prog-8.com/ "progate") を紹介しておきます。  

とはいえテンプレートが良くできているので、あまり深入りして学ばなくとも初心者向けの情報を把握し、必要に応じて検索して学べるようになれば十分だと思います。  

自分は文章の修正に加え、アイコンやボタンの色も変えたかったのでほんの一部CSSにも手を加えました。

### サイト作成の開発環境

テンプレートの修正をする際は修正した結果を常に確認できると効率よく進められます。  
そこで自分は開発環境として[Visual Studio Code (VSCode)](https://azure.microsoft.com/ja-jp/products/visual-studio-code "vscode") に [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer "liveserver") プラグインをインストールしてHTML, CSSの修正結果をブラウザで確認しつつサイトを作成しました。  
VSCodeとLive Serverの使い方については以下を参照してください。

- [【VSCode】Live Serverを使ってWeb制作でライブリロードを活用しよう](https://webdesign-trends.net/entry/14461 "vscode_liveserver")

### Font Awesomeでアイコン表示

Miniportテンプレートではアイコンに [Font Awesome](https://fontawesome.com/ "fontawesome") を利用していました。  

Font Awesomeはアイコンフォントを簡単にサイトに埋め込めるサービスです。  
自分でアイコンを一から作る必要が無いので助かりますね。  
Font Awesomeの使い方は以下を参照してください。  

- [今さら聞けないFont Awesomeの使い方やカスタマイズ方法](https://webdesign-trends.net/entry/14327 "fontawesome_usage")  

Miniportでは `assets/css/fontawesome-all.min.css` の中に使えるアイコンのリストが入っていました。  
Font Awesomeでアイコンを検索し、使いたいものがファイルに含まれていることを確認してHTMLのタグ内にアイコンのタグを埋め込んでサイト上で表示できるようにしました。  
自分のサイトだとこの部分の本やドライバーのアイコンですね。  
![icons](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676041565/shirakamo_lab_tech_blog/portfolio_html5up_github_pages/icons_sy1rhi.webp "icons")

### Canvaでアイコン作成

サイトに載せる自分のアイコンはCanvaで作成しました。  
Canvaは様々なデザインをブラウザで作成できるサービスです。  
Canvaでのアイコン作成については以下を参照してください。  

- [アイコン作成を無料で！素材や画像、デザインテンプレートでおしゃれなデザインが簡単に作れる！](https://www.canva.com/ja_jp/create/icon/ "canva")

今後個人ブログなど作る際はアイキャッチなどもCanvaで作成すると便利そうですね。

## 3.Google Analyticsの設定

こちらの作業にはGoogleアカウントが必要です。  
Google Analyticsの設定についてはこちらを参照してください。  

- [Googleアナリティクスの導入・設定方法と活用のポイント【初心者向け】](https://wacul-ai.com/blog/access-analysis/google-analytics-setting/ga-beginner/)

基本的にはプロフィール名とサイトのURLを設定し、トラッキング用のコードをサイトの `<head>` タグ直下にコピーするだけです。  
今回はシンプルなサイトなので [イベントトラッキング](https://wacul-ai.com/blog/access-analysis/google-analytics-setting/eventtracking/ "ga_event") なども設定せず、アクセス解析のみを行う想定です。

### Google Analyticsで自分のアクセスを除外

Google Analyticsを設定したわけですが、このままだと自分のアクセスもGoogle Analyticsでカウントされてしまいます。  
修正結果の確認などのために自分でサイトにアクセスすることはあると思いますが、それがアクセス解析対象になってしまうのは困ります。  
そこで自分のアクセスはGoogle Analyticsのアクセス解析から除外します。  
以下のサイトを参考にしました。

- [Googleアナリティクスで自分のアクセスを除外する2つの方法｜PCとスマホに分けて解説](https://wacul-ai.com/blog/access-analysis/google-analytics-setting/exclude-your-access/#s0501 "ga_optout")

自分はブラウザにGoogle Chromeを使っているので、 [Google アナリティクス オプトアウト アドオン](https://tools.google.com/dlpage/gaoptout?hl=ja "ga_optout_adon") を拡張機能としてインストールすることで対応しました。  

ここまでで基本的なサイトの作成は完了です。  

## 4.Github Pagesで公開

最後にGithub Pagesでサイトを公開します。  
こちらの作業にはGithubのアカウントが必要です。  
Github Pagesの公開方法はこちらが詳しいです。  

- [GitHub Pagesの始め方](https://qiita.com/tomotlab/items/0a8f2db2781d023aac4a "github_pages")

大まかな作業の流れとしては以下となります。

1. Githubにリポジトリ作成
2. リポジトリの上部タブの `settings` → 左のバーの `Pages` で公開設定

自分の場合は公開ブランチを以下のように `main` とし、ディレクトリを `/(root)` とした上でファイル一式をそのままリポジトリにpushすることで公開しました。  
![github_pages_settings](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676041565/shirakamo_lab_tech_blog/portfolio_html5up_github_pages/github_pages_settings_f0gbix.webp "gp_settings")

なぜかこうしないとCSSなどが反映されなかったり修正したのに反映されなかったりしたんですよね...何か間違っていなければ良いのですが...  
Github Pagesはパスが独特なようなので、この辺りが原因かもしれません。  

- [【GitHub】GitHub PagesでCSSが反映されないときはパスを変えるべし](https://daiblog923.com/github-pages-css-reflect/ "github_pages_path")

これで `https://<アカウント名>.github.io/<リポジトリ名>` でサイトが公開されました。  

これでポートフォリオサイトの公開は完了です。  
自分のようなサイト制作の経験がない人間でも、仕事終わりにちまちま作業して一週間くらいで公開までできました。  
テンプレートや簡単なサイトの公開方法を提供してくださる方々に感謝ですね。
