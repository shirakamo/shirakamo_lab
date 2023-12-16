---
title: "【Googleデータポータル入門第2回】Googleデータポータルでデータ分析"
date: 2023-02-10T19:21:53+09:00
description: "Googleデータポータルでテストデータを実際に分析してみます。"
draft: false
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/google_dataportal_analysis_thumbnail_uxjkuh.webp"
categories:
- "データ分析"
tags:
- "Googleデータポータル"
---

[前回の記事]({{< ref "/post/google_dataportal_setup.md" >}}) にて [Googleデータポータル](https://marketingplatform.google.com/intl/ja/about/data-studio/ "google_dataportal") にて分析をする準備を行いました。  
この記事では実際にテストデータの分析を行ってみます。  
まだデータをアップロードしていない場合は、以下の手順に従ってデータをGoogleデータポータルにアップロードしてください。  
（詳細は前回の記事をご覧ください。）  

1. Google Drive上に [テストデータ](https://www.kaggle.com/ruchi798/movies-on-netflix-prime-video-hulu-and-disney "test") をアップロードして、右クリック → アプリで開く → スプレッドシートを選択してGoogleスプレッドシート形式で保存
1. Googleデータポータルにログインし、左側のメニューの "作成" → データソース → Googleスプレッドシートを選択
1. 先程Google Driveで作成したスプレッドシートを選択
1. （これは本記事の内容ですが）右上の "レポートを作成" をクリック

## "Movies on Netflix, Prime Video, Hulu and Disney+" データセットについて

本記事で扱うデータについて少し解説します。  
詳しい説明は [こちら](https://www.kaggle.com/ruchi798/movies-on-netflix-prime-video-hulu-and-disney "test") を参照してください。  

本データには放映された映画についての評価（ [IMDb](https://ja.wikipedia.org/wiki/%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%8D%E3%83%83%E3%83%88%E3%83%BB%E3%83%A0%E3%83%BC%E3%83%93%E3%83%BC%E3%83%BB%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9 "imdb") や [Rotten Tomatoes](https://ja.wikipedia.org/wiki/Rotten_Tomatoes "rotton_toamtoes") のそれぞれの評価）と、動画視聴サービスNetflix, Amazon Prime Video, Hulu, Desney+において視聴可能かどうか、ジャンル等が格納されています。  
このデータを用いて、どの動画視聴サービスで評価の高い映画を観ることができるのか分析してみましょう。  

### __注意__  

言うまでもありませんが、あくまでこの分析はデモです。  
現実において単純に「評価点が高い映画がたくさん見られるから良いサービスなんだ！！」と判断して、この記事の内容をもとにして動画視聴サービスに加入しないように。  
（しても構いませんが、そのサービスが期待通りでなかったとしても責任は取りません。）  

実際には映画のラインナップ以外にも各々の視聴サービスで特色があるので、各自の好みにあったものを選べば良いと思います。  
例えば、Netflixではオリジナルのドラマや映画を楽しみにしているユーザも多いと聞きますが、それらはおそらく映画評論サイトでの評価対象外のものもあるでしょうし。  
（そもそもドラマは映画評論サイトの対象外でしょう。）  

このように、あくまでデータは現実世界の一部のみを切り取ったものです。  
他のデータで実際に分析を行う際もこの点は頭に入れ、「こういう状況で取得されたこんなデータのもとではこういうことが言える。」と、結論には常に前提条件がついて回ることを忘れないようにしましょう。  

## 分析の目的

データはいかようにも分析できてしまうので、まずは「自分が何を知りたいか / 何が分かったらハッピーか」を決めましょう。  

今回はGoogleデータポータルのセットアップを最初に説明した都合上、データありきのような説明になってしまっていますが、  
本来は「自分が何を知りたいか / 何が分かったらハッピーか」を決めてからそれに見合ったデータを探す / 集める、という順序が良いかと思います。  
（実際は自分もテーマを決め、テーマにあったデータを見繕ってから前回と今回の記事を書いています。自分が映画好きなもので...）  

データによっては分析を進める中で自分が期待していた以上のことが分かる可能性もあるので、目的は暫定的なものになることもあります。そこはデータ次第ですね。  

とりあえず、ここでは以下のような設定のもと、分析の目的を定めましょう。  
__「あなたはこれから動画視聴サービスに加入しようとしていますが、どれに加入するか迷っています。まずは、人気作が多く視聴できるサービスがどれか知りたいと思っています。」__  
ということで「人気作」がどのサービスでどれだけ視聴できるのか、分析してみましょう。  

## データを眺める

分析に踏み込む前に、まずはデータをざっくりと眺めてみましょう。  
これはGoogleデータポータルよりもGoogleスプレッドシートのほうが適しています。  
（表形式で見られればエクセルでもなんでも良いです）  

Googleアナリティクスのようにログが大量にある場合でも、一部だけでもファイルに出力して眺めてみることをオススメします。  
これにより、

- データの大まかな構造
- どんな列（カラム）にどんな量が入っているのか
  - 数値なのか、文字列なのかなど
- 欠損している値はあるのか
  - こちらについてはデータの一部のみを見ていては気づかないこともあるので、最終的にはSQLやR, Pythonなどできちっと調べたほうが良いですが...

あたりが分かります。  

今回のデータでは、映画ごとのIMDb、Rotten Tomatoesの評価と各サービスで視聴可能かどうか（0: 視聴不可、1: 視聴可）が分かりますね。  
人気作 = 評価点が高い作品 として扱えそうです。  

他にも映画の監督、ジャンル、国、言語、放映時間も分かりますね。  
![data spreadsheet](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026740/shirakamo_lab_tech_blog/google_dataportal_analysis/data_spreadsheet_qnntvv.webp "data_spreadsheet")  

お気に入りのジャンルや贔屓の監督などの人気作に絞り込んでより自分の興味にあった情報を取得できそうです。  

ただ、ジャンルや言語はいくつかの要素がカンマで区切られているので、単純に要素を指定するだけではダメそうです。  
ジャンルの `Action` だけを指定すると `Action,Sci-Fi` のように複数要素の中に `Action` も含まれる、という場合を見逃してしまいます。  
「特定の文字列 (`Action`) がジャンルに含まれるか」をどうにか表現しないとジャンルや言語をうまく扱えなさそうである、と分かります。

また、値の欠損があることも分かります。  
監督やジャンルなどはしょうがないとして、IMDbやRotten Tomatoesの評価点にも欠損が含まれています。  
今回の分析では評価点が欠損しているデータは残念ながら使えなさそうです。  

とはいえ、今回は人気作 = 評価点の高い作品に興味があるので、このような評価点が欠損しているデータは無視してしまっても良いかもしれません。  
なにか分析の際に問題を引き起こすようなら対処しましょう。一旦欠損は無視して進めます。  

## データの重複をチェック、削除する

ここで一つ注意です。  
__Googleデータポータルで分析するデータは重複を含まないようにしましょう。__  

重複を含んだデータはうまく集計できないことがあります。  
慣れてくれば [関数](https://support.google.com/datastudio/table/6379764?hl=ja "functions") や [このような方法](https://fm-aid.com/bbs2/viewtopic.php?id=3701 "del_dup") で対処できるかもしれませんが、間違いを防止するためにも、重複は除いておいたほうが無難です。  

さて、「何を重複とみなすか」は分析の目的や方法によります。  
完全に同じ行が存在していると当然それは重複ですが、場合によってはカラムの一部が同じだけでも重複とみなすべきときもあります。  

今の映画データの場合もいくつか重複の定義が思いつきますが、「タイトルが重複している行を重複とみなす」としましょう。  
評価点の上位5位くらいまでのリストを作成した際に、同じタイトルが3つ入っていたりしたら困りますよね。  

今回のデータは "ID" 列が映画1つにつき1つの値となっているので、IDの最大値（最終行になります）とスプレッドシート上の列数を見れば重複がないことがすぐに分かります。  
ぱっとわからない場合にはGoogleスプレッドシート上でもできる方法がいくつかあるので、参考にしてみてください。

- [表内の重複するデータを取り除きたい！ COUNTIF関数でダブりを発見・UNIQUE関数でクリーンな表を作る方法](https://forest.watch.impress.co.jp/docs/serial/googlesps/1216354.html "del_dup_countif")
- [Googleスプレッドシートで重複チェックをスマートにする方法](https://moto.hatenadiary.com/entry/2018/12/02/192131 "del_dup_cond")
- [【新機能】スプレッドシートで重複を削除する](https://spread-sheets.com/useful/data/remove-duplicates/ "del_dup_spreadsheet")

実際は「重複があった場合、どの行を残すか / どのように単一の行にするか」という問題が残ります。  
場合によってはプログラミング言語で柔軟に対処したほうが良い場合もあるかもしれません。  

## Googleデータポータルによるデータ分析

それではGoogleデータポータルを用いて分析してみましょう。  

この記事の最初に述べたような手順でデータソースに接続すると、右上に "レポートを作成" と "探索" のボタンが現れます。  
今回は "レポートの作成" をクリックします。  
すると、次のような画面が新しいタブで開かれます。  
![report first](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026740/shirakamo_lab_tech_blog/google_dataportal_analysis/report_first_mgccs6.webp "report_first")  

これは画面右側の "グラフ" から表が選択され、データ内の "Title" がそれぞれ何行ずつあるか（"Title"ごとのレコード数) を表示するグラフが既に作成されていることを表しています。  
今回はタイトルごとに重複がないため、全ての値が1になることが分かっています。なので、この表は削除してしまいましょう。　　
表をクリックしてバックスペースで直感的に削除することができます。  

また、画面左上が "無題のレポート" となっていますが、クリックすると修正することができるので任意の名前を付けてあげましょう。  
今回は "movie_test" という名前にしました。  

使い方ですが、Googleデータポータルは基本的に、

1. グラフを選択する（円グラフ、折れ線グラフなど）

1. "ディメンション" (データのカラムのことと思って大丈夫です) を選択する

1. ディメンションを集計する指標を作成、選択する

1. 必要に応じてフィルタで絞り込む

1. グラフもしくは表がレポート内に作成される

という流れでレポートを作成していきます。  
さて、ここからは実際に映画データを分析していくわけですが、以下のような流れで行っていきます。  

1. IMDb, Rotten Tomatoesそれぞれで評価点の高い映画を選出

1. 各視聴サービスが評価点の高い映画のうち何%を視聴できるのか集計

1. （発展）言語、ジャンルによるフィルタリングを追加

それでは、やっていきましょう。  

### 評価点の高い映画を選出

まず、IMDb, Rotten Tomatoesのそれぞれで評価点の高い作品を抽出てみましょう。  
IMDbの場合でやってみます。  

IMDb評価点の上位10%を表として抽出し、評価点の高い順に並べてみましょう。  

今必要なグラフは表なので（ちょっとおかしな言い回しですが）、画面上部のメニューバーから "グラフを追加" をクリックし、 "表" をクリックします。  
![graph_table](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026740/shirakamo_lab_tech_blog/google_dataportal_analysis/make_table_h9uuxp.webp "table")  
クリックすると表が作成されます。マウス操作で直感的に表の大きさを調整できるので、適当な大きさに調整します。  

表を作成すると、画面右側にディメンション（表やグラフで表示する対象）や指標（表で並べ替えなどをするための数値）を設定する画面が現れます。  

今は表に表示したいのは映画のタイトルで、IMDbの評価点によって並べ替えたいのでディメンションに "Title"、指標に "IMDb" を選択します。  
また、IMDbの評価点で並べ替えるので、 "データ" タブの下の方にある "並べ替え" でIMDbを選択、降順をクリックして並べ替えます。  
![set_dimensions](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/set_dimensions_n2mlbe.webp "dimensions")  

これで全タイトルをIMDb順で並べて表示する表が完成しました。  
次に、この表のタイトルのうち、IMDbの評価点が上位10%のものに絞って表示してみましょう。  

IMDbの評価点をIMDbの最高評価点で割り算し、その値が0.9以上のものを抽出すれば良さそうです。  
手順としては以下のようになります。  

1. 最高評価点で割り算したIMDbを表す指標 `IMDb_devidecd_by_MAX` を作成する。
1. `IMDb_devided_by_MAX` が0.9以上となるフィルタを作成する。

まず、IMDbの最高評価点ですが、表を見ると最高評価点が 9.3 であることがひと目で分かります。  
まずはこの値を "パラメータ" として登録しておき、後で指標 `IMDb_devided_by_MAX` を作るときに参照できるようにしましょう。  

画面右下の "パラメータを追加" というところをクリックします。  
![add_param](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/add_params_ltr4au.webp "add_param")  

すると、画面下部に以下のようにパラメータを入力する画面が現れるので、  
 "パラメータ名" に `MAX_IMDb` を入力、"データタイプ" として `数値（小数）` を選択、"デフォルト値" として `9.3` を入力します。  
 入力したら、画面下部の "保存" をクリックし、"完了"をクリックします。  
 ![make_param](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026740/shirakamo_lab_tech_blog/google_dataportal_analysis/make_params_yswhgi.webp "make_param")  

次に、指標 `IMDb_devided_by_MAX` を作成します。  

画面右下の "フィールドの追加" をクリックします。  
![add_field](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/add_field_vzzwhj.webp "add_field")  

画面下部に以下のような入力画面が現れるので、"フィールド名"に `IMDb_devided_by_MAX`、”計算式"に `IMDb / MAX_IMDb` と入力します。  
この `MAX_IMDb` は先程登録したパラメータですね。  

入力したら、先ほどと同様に "保存" → "完了" とクリックします。  
![make_field](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026740/shirakamo_lab_tech_blog/google_dataportal_analysis/make_field_dqccaj.webp "make_field")  

では、フィルタを作成します。  

画面右側から、 "フィルタを追加" をクリックします。  
![add_filter](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/add_filter_ssikyc.webp "add_filter")  

画面下部に以下のようにフィルタを入力する画面が現れるので、  
"名前" に `IMDb_TOP10%`を入力し、"一致条件"を選択、"フィールド" として先ほど作成した `IMDb_devided_by_MAX` を選択、  
"次の値以上" を選択して"値" に 0.9 を入力します。  

入力したら、"保存" をクリックします。  
![make_filter](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026740/shirakamo_lab_tech_blog/google_dataportal_analysis/make_filter_vtzf9t.webp "make_filter")  

これでIMDbの評価点が上位10%以内の映画タイトルだけが表に表示されました。  
表の下部にある全体の数値が減っていることからも確認できると思います。  
![table_filtered](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/table_filtered_enwazs.webp "table_filtered")  

画面右上の "表示" ボタンをクリックすると、レポートを閲覧モードで確認できます。  
閲覧モードでは編集モードではできなかった表のスクロールやページ送りなどができます。
評価点の高い作品がどんなものか見てみましょう。おそらく見知った映画が多く存在していると思います。  

### 評価点の高い映画を各視聴サービスでどれだけ見られるかを集計

では、このレポートの主目的である各視聴サービスで評価点の高い映画をどれだけ観られるかをグラフにしてみましょう。  
先程に引き続き、こちらもIMDbの評価点で作成します。  

グラフとしては視聴可能、不可能を割合で表した円グラフや棒グラフなどが考えられますが、今回は棒グラフで可視化してみます。  

まずは `Prime Video` から初めましょう。  

画面上部のメニューバーの "グラフの追加" → "棒グラフ" と選択し、"ディメンション"  に `Prime Video` を選択します。  
映画がある視聴サービスで観られるかどうかは各視聴サービスの名前を示すカラムに 1 (視聴可能)、 0（視聴不可能）で表されているので、  
1 の件数と 0 の件数がそのまま視聴可能数、不可能数となります。  
なので、"指標" には "Record Count" を選択します。  

さて、このままでも良いのですが、指標には任意の名前を付けられます。  
"指標" として選択した "Record Count" の右側に "AUT" と書いてあるボタンがあるので、そこをクリックすると名前を入力することができます。  
ここでは "映画のタイトル数" と付けました。  

ここまで行うと、次のような棒グラフが作成されます。  
 `Prime Video` は視聴可能な映画が 1万タイトル以上ありそうですね。  
 ここで注意すべきなのは、今はまだフィルタは付けていないので、この数字は全データ中で `Prime Video` で視聴可能なタイトル数となります。  
 評価点が高いものへの絞り込みは後で行います。  
 ![bar plot](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/bar_plot_sdtigj.webp "bar_plot")  

次に、視聴可能、不可能が1、0のフラグになっていて分かりづらいので、  
1 → "視聴可能"、0 → "視聴不可能" と置き換えたフィールドを作成してディメンションをこちらのフィールドに置き換えます。  

先程 `IMDb_devided_MAX` 指標を作成したときと同様に、 画面右下の "フィールドの追加" をクリックし、  
"フィールド名" に `is_viewable_Prime_Video` (任意の名前で構いません)を入力します。  
さらに "計算式" に以下を入力し、 "保存" → "完了" でフィールドを追加します。

```sql
CASE
    WHEN Prime Video = 1 THEN "視聴可能"
    ELSE "視聴不可能"
END 
```

これはデータポータルで使用できる関数の中でも CASE式と呼ばれるもので、条件分岐を実現する関数です。  

[CASE関数](https://support.google.com/datastudio/answer/7020724?hl=ja&ref_topic=10490726#zippy=%2C%E3%81%93%E3%81%AE%E8%A8%98%E4%BA%8B%E3%81%AE%E5%86%85%E5%AE%B9 "case")  

このように、データポータルには様々な集計を実現するための関数があるので、必要に応じて使用しましょう。  
関数は [こちら](https://support.google.com/datastudio/table/6379764?hl=ja "funcs") にまとまっています。  

この集計の最後の手順として、IMDbの評価点の上位10％に入るタイトルのみに絞り込みを行います。  

表を作成したときと同様に画面右側の "フィルタの追加" をクリックします。  
すると、表の絞り込みのときに作成した `IMDb_TOP10%` フィルタが選択できます。  
今回はこのフィルタを選択するのみで絞り込みが実現できます。  

ここまで行うと、以下の画像に示すような棒グラフを作成できます。  
![bar plot filtered](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/bar_plot_filtered_obfhde.webp "bar_plot_filtered")  

これで `Prime Video` の集計は終了しました。  
他の視聴サービスについても同様の手順で棒グラフを作成してみてください。  

### ジャンルの絞り込み

最後に、ジャンルの絞り込みを行ってみましょう。  

ジャンルを絞り込むためには、単に要素を選択するだけではダメで、「ジャンルを表す文字列を持つ」という条件で抽出しなければならないのでしたね。  
私がアクション映画が好きなので、アクション映画に絞り込みをしてみます。  
データではアクション映画は「カラム `Genres` に `Action` という文字列を含む」という条件で抽出できそうです。  

まずは表でIMDbの評価点が高いアクション映画のタイトルを見てみましょう。  
最初に作成した表をクリックし、"フィールドの追加" から `Genres` に `Action` を含むか否かを 1, 0 で表した `has_action` というフィールドを作成します。  
今回は計算式を以下のように設定します。  

```sql
CONTAINS_TEXT(Genres, "Action")
```

次に、 "フィルタの追加" をクリックし、下の方にある "フィルタの作成" ボタンをクリックします。  
フィルタの作成画面で、 "名前" を `action_movies`、 "一致条件"、`has_action`、"真" を選択し、"保存"をクリックします。  

IMDbの上位10%だと16作品しかありませんね。  
ジャンルの絞り込みをする際は評価点の範囲をもっと広げたほうが良さそうです。  
![action](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/table_action_qm0box.webp "action")  

## レポートの作成、考察

お疲れ様でした！！  
これでIMDbの評価点が高い映画タイトルをいくつ `Prime Video` で視聴できるか、さらにアクション映画への絞り込みを可能としました。  

同様の手順で他の視聴サービスについての集計や、 Rotten Tomatoesの評価点ベースでの集計も行えるはずです。  
ぜひ皆様も手を動かしてやってみてください。  

最終的に、IMDbベースでの集計は次の画像のようになりました。  
![IMDb](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026740/shirakamo_lab_tech_blog/google_dataportal_analysis/imdb_pui7sj.webp "imdb")  

Rotten Tomatoesベースの集計は次の画像のようになりました。  
![RT](https://res.cloudinary.com/dda9f1d6p/image/upload/v1676026739/shirakamo_lab_tech_blog/google_dataportal_analysis/rt_ytgcml.webp "rt")  

今回は完全に自分の趣味なので考察は特に必要ありませんが、  
見てみると映画評論サイトでだいぶ上位タイトルも視聴サービスでのラインナップも異なることが分かりますね。  

特にIMDb上位のPrime Videoのラインナップは突出しています。  
とはいえPrime Videoの場合、月額で見放題の作品の他にレンタルサービスもあるので、そちらでラインナップが強化されているのでしょうか？  
データセットの説明にもその辺り触れられていないので、そこは分からないところです。  

Huluは2021年現在は日本企業が運営しているので、日本の作品のほうが多いのかもしれませんね。  
それが理由でIMDb上位で絞り込んだときはあまり件数が出ないのかも...  
データには国の情報も入っているので、日本に絞り込んでみたらまた違った結果になるのかもしれません。  

このようにより深く広く分析をすることができるので、皆様も自分なりの分析を行ってみると面白いと思います。  

## 最後に

これでGoogleデータポータルを用いた映画データの分析は終了としたいと思います。  
この内容を見て皆様もGoogleデータポータルでなにかやってみたい！！と思っていただけたら幸いです。  

さて、今回の手順ですが、棒グラフをほぼ同じ手順で何回も作るのが面倒だと思った方もおられたかと思います。  
これはデータポータルで可視化をする前にデータそのものを変形しておくと一度に全視聴サービスについてのグラフを作成できます。  

こちらの内容は番外編として [次回の記事]({{<ref "google_dataportal_trans_data.md" >}}) に投稿しています。
