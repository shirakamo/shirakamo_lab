---
title: "HugoとCloudFlare Pagesでブログ公開"
date: 2023-02-12T13:04:02+09:00
description: "Hugoでブログを作成しCloudFlare Pagesで公開した際の作業ログ"
draft: false 
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1676177485/shirakamo_lab_tech_blog/blog_hugo_cloudflare_pages/blog_hugo_cloudflare_pages_b9hwhg.webp"
categories:
- "エンジニアリング"
tags:
- "サイト制作"
---

このブログを公開するまでに行った作業をまとめました。  
このブログは [Hugo](https://gohugo.io/ "hugo") のテンプレートを用いて作成し、[CloudFlare Pages](https://www.cloudflare.com/ja-jp/products/pages/ "cloudflare_pages") で公開しています。  
また画像の配信に [Cloudinary](https://cloudinary.com/ "cloudinary") を利用することで容量の削減+表示スピードアップも狙っています。  
この構成ならある程度簡単にブログ公開まで辿り着くのでおススメです。  

今の所どのサービスも無料枠で利用しているので、かかっている費用としてはドメイン代のみとなります。  
独自ドメインに拘らないのなら無料で運用できますね。  

詳細な作業内容については先達の素晴らしい記事がネット上にたくさんあるので、そちらの記事のリンクを紹介しつつこの記事では作業手順はさらっとだけ触れるのみにします。  
その分自分が詰まったところは詳しめに書いていこうと思います。  

## 利用するサービス

ブログ公開に際して利用しているサービスを以下にまとめました。  
アカウント作成が必要なサービスが多いので、事前にアカウントを作ってサイトをブックマークしておくと後の作業が楽です。

- 必須のサービス

| サービス | 用途 | アカウント作成 |
| ---- | ---- | ---- |
| [Hugo](https://gohugo.io/ "hugo") | メインで使用するツール<br>記事を含むサイト全体を製作するためのツール | 不要 |
| [GitHub](https://github.co.jp/ "github") | サイト全体のコード、記事を格納するリポジトリとして利用 | 必要 |
| [CloudFlare Pages](https://www.cloudflare.com/ja-jp/products/pages/ "cloudflare_pages") | サイトをネット上に公開するためのサービス | 必要 |

- 必要であれば利用するサービス

| サービス | 用途 | アカウント作成 |
| ---- | ---- | ---- |
| [Cloudinary](https://cloudinary.com/ "cloudinary") | 公開されたサイトに画像を配信するためのサービス<br>画像もGitHub上で管理するなら不要 | 必要 |
| [お名前ドットコム](https://www.onamae.com/ "onamae") | ドメインを購入するサービス<br>独自ドメインを設定しないなら不要 | 必要 |
| [Canva](https://www.canva.com/ja_jp/ "canva") | 画像を作成するときに利用 | 必要 |

## 全体の構成

構成の概要は記事の初めで触れましたが、全体の構成を図にすると以下のようになります。  
今回利用したサービスの類似サービス ( [Netlify](https://www.netlify.com/ "netlify") など) を利用する場合でも同様の構成になると思います。  
一度やっておくと類似サービスに乗り換える際も楽にできるようになるかもです。  

![blog_structure](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676200394/shirakamo_lab_tech_blog/blog_hugo_cloudflare_pages/blog_structure_resize_zo0pvs.webp "structure")

ブログ記事の公開について具体的には、

1. ローカルPCでHugoを利用してブログを作成し、
2. 作成したブログを含むコード全体をGitHubのリポジトリにアップして、
3. CloudFlare PagesとGitHubのブログリポジトリを連携して、
4. CloudFlare Pagesでリポジトリのコードからブログサイトをビルドする設定をして公開

という流れとなります。  
またお名前ドットコムで取得したドメインをCloudFlare Pagesで設定して独自ドメインとして運用しています。  
画像配信については

1. ローカルPCでCanva等を利用して画像を作成し、
2. Cloudinaryに画像をアップロードして配信用URLを取得
3. URLをブログに埋め込んでアクセス時に画像配信

という流れとなります。  

## 1. GitHubの準備

上記の構成の場合、サイトを構成するコードも記事自体もGitHubで管理します。  
そのためGit, GitHubの使い方には慣れておいた方が良いです。  
基本個人で利用すると思うので複雑な機能を使うことはないとは思いますが...  
Git, GitHubの使い方については公式のチュートリアルやサル先生に教えてもらいましょう。  

- [公式のチュートリアル](https://docs.github.com/ja/get-started/quickstart/hello-world "git_tutorial")
- [サル先生のGit入門](https://backlog.com/ja/git-tutorial/ "git_saru")

ブログ開設の準備としては、この段階では以下3点をやっておきます。

1. ローカルPCにgitをインストール
2. GitHubのアカウントを作成
3. GitHub上にブログサイトのリポジトリを作成

GitHubを利用する際はSSH接続を利用すると便利です。  
以下で公式が方法を説明しているのでそちらを参照して設定しておくと良いと思います。

- [SSH を使用した GitHub への接続](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh "github_ssh")

GitHubで複数アカウントを所持している場合はSSH接続の時に少しだけ別途ローカルPCで設定が必要です。  
以下の記事で詳しく説明されているのでそちらを参照してください。  

- [複数のGitHubアカウントをSSHで使い分けるための設定](https://zenn.dev/ejointjp/articles/8a5e81b6eded88 "github_multi_account")

## 2. Hugoでブログ制作

Hugoは [静的サイトジェネレータ](https://www.cloudflare.com/ja-jp/learning/performance/static-site-generator/ "static_site_generator") なので
HTMLなどのコード群からウェブサイトをビルドするのが中心的な機能です。  
ただしHugoには既に豊富なウェブサイトのテンプレートがテーマとして用意されており、それらを利用することでサイト制作の手間を一気に削減することができます。  
もちろん、テンプレートを修正して自分なりのサイトを作ることも可能です（後で簡単な例を示します）。  

- [Hugoのテーマ一覧](https://themes.gohugo.io/ "hugo_themes")

Hugoでのテーマを利用したサイト制作についてはまずは公式のチュートリアルを一読するのが良いと思います。  
インストール方法も公式サイトを見れば分かるようになっています。  

- [Hugo公式のチュートリアル](https://gohugo.io/getting-started/ "hugo_tutorial")

サイトのカスタマイズ方法などは使用するテーマに依存するのでテーマごとのドキュメントなどを参考すると良いです。

<!-- START MoshimoAffiliateEasyLink -->
<script type="text/javascript">
(function(b,c,f,g,a,d,e){b.MoshimoAffiliateObject=a;
b[a]=b[a]||function(){arguments.currentScript=c.currentScript
||c.scripts[c.scripts.length-2];(b[a].q=b[a].q||[]).push(arguments)};
c.getElementById(a)||(d=c.createElement(f),d.src=g,
d.id=a,e=c.getElementsByTagName("body")[0],e.appendChild(d))})
(window,document,"script","//dn.msmstatic.com/site/cardlink/bundle.js?20220329","msmaflink");
msmaflink({"n":"Hugoで始める静的サイト構築入門　静的サイトジェネレーターで作る自作サイト","b":"","t":"","d":"https:\/\/m.media-amazon.com","c_p":"\/images\/I","p":["\/51DRDYXF0CL._SL500_.jpg","\/21zvhc3t76L._SL500_.jpg"],"u":{"u":"https:\/\/www.amazon.co.jp\/dp\/4844379208","t":"amazon","r_v":""},"v":"2.1","b_l":[{"id":1,"u_tx":"Amazonで見る","u_bc":"#f79256","u_url":"https:\/\/www.amazon.co.jp\/dp\/4844379208","a_id":3885217,"p_id":170,"pl_id":27060,"pc_id":185,"s_n":"amazon","u_so":1},{"id":2,"u_tx":"楽天市場で見る","u_bc":"#f76956","u_url":"https:\/\/search.rakuten.co.jp\/search\/mall\/Hugo%E3%81%A7%E5%A7%8B%E3%82%81%E3%82%8B%E9%9D%99%E7%9A%84%E3%82%B5%E3%82%A4%E3%83%88%E6%A7%8B%E7%AF%89%E5%85%A5%E9%96%80%E3%80%80%E9%9D%99%E7%9A%84%E3%82%B5%E3%82%A4%E3%83%88%E3%82%B8%E3%82%A7%E3%83%8D%E3%83%AC%E3%83%BC%E3%82%BF%E3%83%BC%E3%81%A7%E4%BD%9C%E3%82%8B%E8%87%AA%E4%BD%9C%E3%82%B5%E3%82%A4%E3%83%88\/","a_id":3829408,"p_id":54,"pl_id":27059,"pc_id":54,"s_n":"rakuten","u_so":2},{"id":3,"u_tx":"Yahoo!ショッピングで見る","u_bc":"#66a7ff","u_url":"https:\/\/shopping.yahoo.co.jp\/search?first=1\u0026p=Hugo%E3%81%A7%E5%A7%8B%E3%82%81%E3%82%8B%E9%9D%99%E7%9A%84%E3%82%B5%E3%82%A4%E3%83%88%E6%A7%8B%E7%AF%89%E5%85%A5%E9%96%80%E3%80%80%E9%9D%99%E7%9A%84%E3%82%B5%E3%82%A4%E3%83%88%E3%82%B8%E3%82%A7%E3%83%8D%E3%83%AC%E3%83%BC%E3%82%BF%E3%83%BC%E3%81%A7%E4%BD%9C%E3%82%8B%E8%87%AA%E4%BD%9C%E3%82%B5%E3%82%A4%E3%83%88","a_id":3829423,"p_id":1225,"pl_id":27061,"pc_id":1925,"s_n":"yahoo","u_so":3}],"eid":"7edgX","s":"s"});
</script>
<div id="msmaflink-7edgX">リンク</div>
<!-- MoshimoAffiliateEasyLink END -->
<br>

### 2.1 テーマ選択

今回は比較的カスタマイズしやすそうなブログ向けのデザインとして [Mainroad](https://themes.gohugo.io/themes/mainroad/ "mainroad") にしました。  
Mainroadはドキュメントもしっかり用意されており、カスタマイズ方法もある程度分かるようになっています。

- [MainroadのGithubリポジトリ](https://github.com/Vimux/Mainroad "mainroad_github")

Mainroadの導入方法とカスタマイズ方法は上記の公式ページ以外にも色々情報が見つかると思います。  
今回はこちらの記事を参考にさせていただきました。  

- [Hugo によるブログ作成と mainroad テーマのカスタマイズ](https://terashim.com/posts/create-hugo-blog-and-customize-mainroad-theme/ "mainroad_customize")

### 2.2 テーマのカスタマイズ

Hugoではテーマのカスタマイズもできるようになっています。  
ここではカスタマイズの例としてベースとなるHTMLにGoogle Analyticsのトラッキング用コードを入れ込んでみます。  

自分の場合も [こちらの記事](https://lesnounours.github.io/posts/2021_03_11_hugo_mainroad/ "mainroad_ga") と同様 `config.toml` に設定するだけではGoogle Analytics使えなかったんですよね...  
そこで記事にもあるようにHTMLのヘッダにGoogle Analyticsのトラッキングコードを直接埋め込みました。  
簡単な手順は以下となります。  

1. `(root)/themes/mainroad/layouts/_default/baseof.html` を `(root)/layouts/_default/baseof.html` にコピー
2. コピーした `baseof.html` の `<head>` 要素の直下にGoogle Analyticsのトラッキングコードをコピー

これでテーマの `baseof.html` が自分が作成したものにオーバーライドされ、GAによるトラッキングが有効化されます。  
HTML要素自体をカスタマイズしたいときは上記の方法を別のページで真似すれば大抵はできるはずです。  

### 2.3 CSSのカスタマイズ

色や文字の大きさなどをカスタマイズしたいときはCSSをいじることになります。  
基本的には `(root)/static/css/` 以下にカスタマイズ用のCSSファイルを配置することで自分なりのCSSを作成することができます。  
自分の場合は `(root)/static/css/custom.css` を作成し、 `config.toml` で

```toml
customCSS = ["css/custom.css"]
```

と指定することで有効化しています。  
ちなみにカスタムCSSはテーマのCSSを上書きします。  

個人的には文字の大きさとサムネイル画像の見え方を変えたかったので主にそちら関連をいじっています。  
以下の記事を参考にさせていただきました。  

- [【HUGO】Mainroadのリストページのカスタマイズ](https://sparrow-tune.work/posts/web/hugo/mainroad%E3%81%AE%E3%83%AA%E3%82%B9%E3%83%88%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA/ "list_customize")

### 2.4 インラインHTMLの有効化

Hugoはver0.60以降から記事のMarkdown内に直接HTMLを記述してもサイト上では表示されなくなりました。  

- [Hugo v0.60以上を使うと、Markdown中のHTMLタグが「raw HTML omitted」となって消えてしまう](https://budougumi0617.github.io/2020/03/10/hugo-render-raw-html/ "hugo_raw_html")
- [Hugo shortcode ignored saying "raw HTML omitted"](https://stackoverflow.com/questions/63198652/hugo-shortcode-ignored-saying-raw-html-omitted "hugo_raw_html_stack")
  
上記の記事にあるように `config.toml` に以下の要素を記述してインラインHTMLを有効化しました。

```toml
[markup] # インラインHTMLを有効化
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true
```

これが原因でもしもアフィリエイトの簡単リンクが反映されなくて焦りました...  

上記まで行った段階で先ほど作成したGitHubのリポジトリにPushしておきます。  
カスタマイズはいくらでも凝れる所なので、気が向いたら少しずつアップデートしていこうと思います。

## 3.Cloudinaryを利用した画像配信

画像の配信は [Cloudinary](https://cloudinary.com/ "cloudinary") を利用します。  
手順は簡単です。

1. [Cloudinary](https://cloudinary.com/ "cloudinary") のアカウントを作成してログインする
2. ローカルPCで画像を作成する
   1. リサイズや [WebPフォーマット](https://ja.wikipedia.org/wiki/WebP "webp") 変換もローカルで行っておきます
3. Cloudinaryに画像をアップして配信用のURLを取得する
4. 配信用URLを記事に埋め込む

CloudinaryのMedia Libaryにおいて配信用URLは以下の図の矢印の部分からコピーできます。  

![dist_url](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676211342/shirakamo_lab_tech_blog/blog_hugo_cloudflare_pages/cloudinary_resize_hbyyj9.webp "dist_url")

コピーしたURLを `![image](コピーしたURL "alt")` のように記事内に記述することで記事内に画像を埋め込めます。  

実のところ、Cloudinariは上記の配信用URL内にパラメータを記述することでリサイズなど様々な処理を行うことができます。  

- [Cloudinaryの始め方。画像や動画を配信するサービスを使えるようになるまでの手順](https://kanaxx.hatenablog.jp/entry/cloudinary/first-step#:~:text=%E3%81%8A%E3%81%8D%E3%81%BE%E3%81%97%E3%82%87%E3%81%86%E3%80%82-,%E7%94%BB%E5%83%8F%E3%82%92%E5%A4%89%E6%8F%9B%E3%81%97%E3%81%AA%E3%81%8C%E3%82%89%E9%85%8D%E4%BF%A1%E3%81%99%E3%82%8B,-%E3%81%93%E3%81%93%E3%81%8B%E3%82%89%E3%81%8C "cl_transform")

ただしFreeプランではアップロードできる容量や処理回数に上限があるため、ローカルでできるだけの処理は行いCloudinaryでは配信するだけにしています。  
Cloudinaryの料金体系は以下の記事にまとまっています。  

- [Cloudinaryの無料プランでどこまでやれるか？料金プラン調べてみた](https://zenn.dev/price/articles/fe1a06c7b04472 "cl_credit")

## 4.CloudFlare Pagesでブログ公開

GitHubのリポジトリをCloudFlare Pagesとリンクさせることでブログを公開します。  
基本的にログインして `Pages` のセクションに行けば画面の説明通りに設定していくだけです。  
詳細は以下を参照してください。  

- [公式のチュートリアル](https://developers.cloudflare.com/pages/get-started/ "pages_tutorial")
- [GitHubのリポジトリとCloudflare Pagesを接続して、ホスティングする](https://www.newt.so/docs/tutorials/connect-to-cloudflare-pages "pages_blog")

流れとしては以下の手順です。  

1. [CloudFlare](https://www.cloudflare.com/ja-jp/ "cloudflare") にアクセスしてアカウント作成&ログイン
2. ログイン後のダッシュボード右側のメニューから `Pages` を選択
3. 後は画面の指示に従ってGitHubのリポジトリと接続

基本的には上記の設定で公開できるのですが、CloudFlare Pagesでデフォルトで利用しているHugoがver0.54と大分古いため問題が生じる可能性もあります。  
そこで環境変数で自分のローカルPCにインストールしたHugoと同じバージョンに合わせておいた方が良いです。  

- [Hugo プロジェクトを Cloudflare Pages へデプロイ](https://peaceiris.com/ja/posts/deploy-hugo-project-to-cloudflare-pages/#:~:text=%E3%81%84%E3%81%A6%E3%81%8F%E3%82%8C%E3%82%8B-,Hugo%20Version,-Hugo%20%E3%81%AF%20extended "env_hugo_ver")

## 5.独自ドメインの設定

こちらの作業は必須ではありません。  

上記の手順でCloudFlare Pagesにてブログを公開した場合、URLは `https://<リポジトリ名>.pages.dev` となります。  
これを [お名前ドットコム](https://www.onamae.com/ "oaname") で取得した独自ドメインに変更します。  

- [お名前ドットコムでの独自ドメインの取得方法はこちら](https://www.onamae.com/service/domainregist/ "self_domain")
- [Cloudflare Pagesにカスタムドメインを設定する](https://qiita.com/akitkat/items/8aeaee639ba5f2bda141 "pages_self_domain")
