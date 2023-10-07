---
title: "時系列データセットまとめ"
date: 2023-10-07T23:54:01+09:00
description: "検証用に使える時系列データの所在をまとめました"
draft: false
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1696691846/time_series_datasets_thumbnail_p6zy1o.webp"
categories:
- "データ分析"
tags:
- "時系列分析"
---

様々な時系列データのレポジトリや所在をまとめてみました。  
元々時系列分析に興味があってちょこちょこ分析してみたりしていたのですが、最近仕事でも時系列分析に関わることになりまして。  
そこで時系列分析の様々なアルゴリズムの検証用＆実データでの適用 ~~＆趣味~~ に向けて一念発起して集めてみました。  
個人的な備忘録として使うつもりですが、もしご覧の皆様にも参考になる部分があれば幸いです。  
※今後も良さげなデータを見つけ次第追記していく予定です。  

## 時系列データセットまとめ

以下の書籍からかなり引用させていただきました。  

<!-- START MoshimoAffiliateEasyLink -->
<script type="text/javascript">
(function(b,c,f,g,a,d,e){b.MoshimoAffiliateObject=a;
b[a]=b[a]||function(){arguments.currentScript=c.currentScript
||c.scripts[c.scripts.length-2];(b[a].q=b[a].q||[]).push(arguments)};
c.getElementById(a)||(d=c.createElement(f),d.src=g,
d.id=a,e=c.getElementsByTagName("body")[0],e.appendChild(d))})
(window,document,"script","//dn.msmstatic.com/site/cardlink/bundle.js?20220329","msmaflink");
msmaflink({"n":"実践 時系列解析 ―統計と機械学習による予測","b":"","t":"","d":"https:\/\/m.media-amazon.com","c_p":"\/images\/I","p":["\/51RYw2ZZUfL._SL500_.jpg","\/51Bts0DzzRL._SL500_.jpg","\/51jRPsFZYVL._SL500_.jpg","\/51rsaMcN-ZL._SL500_.jpg","\/51a5L2gAuIL._SL500_.jpg","\/41UHYSWHv9L._SL500_.jpg","\/41hxXTOBjIL._SL500_.jpg","\/51AxNTxsVoL._SL500_.jpg","\/41qMeM6q9tL._SL500_.jpg","\/4186McK3xCL._SL500_.jpg","\/418BOJwh9LL._SL500_.jpg","\/41hJVAoUmWL._SL500_.jpg","\/41ANkEZVEtL._SL500_.jpg","\/41K6OWKDSQL._SL500_.jpg","\/31Jg+vUAKvL._SL500_.jpg","\/51fHZQdPuTL._SL500_.jpg","\/41DXyVF7ZJL._SL500_.jpg","\/41+h++rXaSL._SL500_.jpg","\/51y0rS6XJiL._SL500_.jpg","\/51jAgwRn4CL._SL500_.jpg","\/51u+aDDIaLL._SL500_.jpg","\/51ZfYj4VhKL._SL500_.jpg","\/51FVoPGzrUL._SL500_.jpg","\/51O03r6dFxL._SL500_.jpg","\/51Yk23UIgGL._SL500_.jpg","\/515YeGMOBfL._SL500_.jpg","\/51WxpOcB2XL._SL500_.jpg","\/51nlmu1VhiL._SL500_.jpg","\/41OPkTaZ-OL._SL500_.jpg","\/518UNHgx4tL._SL500_.jpg","\/41HeH-VSjLL._SL500_.jpg","\/51vNgE58pPL._SL500_.jpg","\/51lS4JA6+fL._SL500_.jpg","\/51q8M+VXVbL._SL500_.jpg","\/51s1V3EKBmL._SL500_.jpg","\/518Jz3MH39L._SL500_.jpg","\/51lpRFB0WlL._SL500_.jpg","\/41mpriquCKL._SL500_.jpg","\/51unglSx+7L._SL500_.jpg","\/41aL7XV-p4L._SL500_.jpg","\/51ce8hrcxcL._SL500_.jpg","\/51h6LCjpQFL._SL500_.jpg","\/51Ebt5aS6sL._SL500_.jpg","\/41AlXpFRQeL._SL500_.jpg"],"u":{"u":"https:\/\/www.amazon.co.jp\/dp\/487311960X","t":"amazon","r_v":""},"v":"2.1","b_l":[{"id":1,"u_tx":"Amazonで見る","u_bc":"#f79256","u_url":"https:\/\/www.amazon.co.jp\/dp\/487311960X","a_id":3885217,"p_id":170,"pl_id":27060,"pc_id":185,"s_n":"amazon","u_so":1},{"u_bc":"#f76956","u_tx":"楽天市場で見る","u_url":"https:\/\/search.rakuten.co.jp\/search\/mall\/%E5%AE%9F%E8%B7%B5%20%E6%99%82%E7%B3%BB%E5%88%97%E8%A7%A3%E6%9E%90%20%E2%80%95%E7%B5%B1%E8%A8%88%E3%81%A8%E6%A9%9F%E6%A2%B0%E5%AD%A6%E7%BF%92%E3%81%AB%E3%82%88%E3%82%8B%E4%BA%88%E6%B8%AC\/","s_n":"rakuten","u_so":3,"a_id":3829408,"p_id":54,"pc_id":54,"pl_id":27059,"id":4},{"u_bc":"#66a7ff","u_tx":"Yahoo!ショッピングで見る","u_url":"https:\/\/shopping.yahoo.co.jp\/search?first=1\u0026p=%E5%AE%9F%E8%B7%B5%20%E6%99%82%E7%B3%BB%E5%88%97%E8%A7%A3%E6%9E%90%20%E2%80%95%E7%B5%B1%E8%A8%88%E3%81%A8%E6%A9%9F%E6%A2%B0%E5%AD%A6%E7%BF%92%E3%81%AB%E3%82%88%E3%82%8B%E4%BA%88%E6%B8%AC","s_n":"yahoo","u_so":4,"a_id":3829423,"p_id":1225,"pc_id":1925,"pl_id":27061}],"eid":"nU67J","s":"s"});
</script>
<div id="msmaflink-nU67J">リンク</div>
<!-- MoshimoAffiliateEasyLink END -->

説明欄に※がついているものは書籍からの引用です。

| サイト | 説明 |
| --- | --- |
| [UCI機械学習リポジトリ時系列データ集](https://archive.ics.uci.edu/datasets?skip=0&take=10&sort=desc&orderBy=NumHits&search=&Types=Time-Series) | UCI機械学習レポジトリに所蔵されている時系列データセット※ |
| [UEA&UCR時系列分類レポジトリ](http://www.timeseriesclassification.com/) | 時系列分類、時系列クラスタリング向けのデータセット※ |
| [Kaggle時系列データセット](https://www.kaggle.com/datasets?tags=13209-Time+Series+Analysis) | KaggleのDatasetsの内、時系列データに絞り込んだもの |
| [Mcomp](https://cran.r-project.org/web/packages/Mcomp/index.html), [M4comp2018](https://github.com/carlanetto/M4comp2018) | Rパッケージ。Mコンテスト（時系列予測のコンペティション）のデータ※ |
| [tscompdata](https://github.com/robjhyndman/tscompdata) | Rパッケージ。様々な時系列コンペティションのデータ※ |
| [CRAN時系列パッケージ一覧](https://cran.r-project.org/web/views/TimeSeries.html) | Rパッケージ。Rパッケージに含まれる様々な時系列データがまとめられている※ |
| [CompEngine](https://www.comp-engine.org/) | 時系列データの”自己組織化データベース”。高度比較時系列解析機能を提供しており、手元の時系列データと似たデータを探すこともできる※ |
| [米国海洋大気庁国立環境情報センター](https://www.ncdc.noaa.gov/cdo-web/datasets) | 気象関連のデータセット※ |
| [JAMSTECデータカタログ](https://www.godac.jamstec.go.jp/data_catalog/view/base?lang=ja) | 日本の海洋研究開発機構が採集した観測データやシミュレーションデータ |
| [セントルイス連邦準備銀行(FRED)](https://fred.stlouisfed.org/) | 様々な経済時系列データが得られるサイト※ |
| [米国労働省労働統計局](https://www.bls.gov/) | アメリカの労働市場に関する時系列データ※ |
| [日本銀行時系列統計データ](https://www.stat-search.boj.or.jp/) | 日本銀行が公表しているデータセット一覧 |
| [e-Stat](https://www.e-stat.go.jp/) | 日本政府統計のポータルサイト |
| [疾病予防管理センターのインフルエンザデータ](https://www.cdc.gov/flu/weekly/fluviewinteractive.htm) | アメリカのインフルエンザ流行期の週ごとのインフルエンザ感染者数※ |

UCI機械学習リポジトリやKaggleのデータセットは定番ですね。  
このような大規模なデータセットは大体絞り込みができるので、時系列データに絞り込んで上のリンクを埋め込んでみました。  

[UEA&UCR時系列分類レポジトリ](http://www.timeseriesclassification.com/)は時系列分類に特化している点がユニークですね。  
ワインのスペクトル（化学成分を分析したグラフ）などは時系列ではありませんが、系列データの分類によってワインを分類するなど、タスクとしても面白いデータがそろってます。  

[CompEngine](https://www.comp-engine.org/)は規模もすごいです。  
ユーザが手元のデータと似たデータを調べるために、手元のデータをアップロードするわけですが、ユーザが望めばそのデータをデータセットに追加することもできるようです。  
まさに”自己組織化”ですね。  

実データで言うとやはり経済関連や気象関連が中心になってきますね。  
FREDは経済の様々なデータを取得できるのでよくお世話になっています。  
こちらとか↓  
[Polarsでの時系列データ処理]({{< ref "/post/polars_timeseries_apis.md" >}})

## 更新履歴

2023/10/07作成
