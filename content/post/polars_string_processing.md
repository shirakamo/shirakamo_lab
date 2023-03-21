---
title: "Polars文字列処理関数で100本ノックの問題を解いてみた"
date: 2023-03-21T16:48:24+09:00
description: "Polarsの文字列処理用関数について100本ノックの演習を通して解説します。"
draft: false
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1679407687/shirakamo_lab_tech_blog/polars_string_processing/polars_string_processing_thumbnails_g3sytu.webp"
categories:
- "データ分析"
tags:
- "Python"
- "Polars"
---

[Polars](https://github.com/pola-rs/polars "polars") とはPythonやRustで使える表計算のためのライブラリです。  
この記事ではPythonで利用できるPolarsの文字列処理について解説、100本ノックの演習問題の一部を実際にPolarsで解いてみます。  

## Polarsとは

Pythonの表計算ライブラリと言えばPandasが有名ですが、[Polars](https://github.com/pola-rs/polars "polars") はPandasに比べ以下のようなメリットがあります。

1. 高速な処理速度
   1. Polarsは、Rustで書かれているため、Pandasよりも高速であり、非常に大きなデータセットを処理する場合に特に有用です。
2. 分散処理
   1. Polarsは、Apache Arrowと呼ばれるメモリフォーマットを使用しており、分散処理のために最適化されています。したがって、複数のマシン上での並列処理が可能で、大規模なデータセットをより効率的に処理できます。GPUもサポートしているため、GPUが使用できる環境でも速度向上が見込めます。
3. SQLのサポート
   1. PolarsにはSQLクエリ言語をサポートする機能があり、SQLのクエリを使用してデータを抽出、加工、集計することができます。
4. シリアライズ
   1. Polarsは、データをシリアライズしてディスクに保存することができます。また、データを縮小してメモリ使用量を削減することもできます。

まだまだ発展途上で仕様の変更などが発生することもありますが、個人的には書き方と速度の点でPandasよりも使いやすいと感じています。  
Polarsの基本的な使い方については以下の記事がとても参考になります。  

- [pandasから移行する人向け polars使用ガイド](https://qiita.com/nkay/items/9cfb2776156dc7e054c8 "pandas_to_polars")

## Polarsの文字列処理

この記事Polarsで文字列処理を行う方法について解説します。  

現実のデータ分析の際にはデータ中に含まれる文字列を何らかの形で処理する場面に何度も出会います。  
（商品レビューのようなコメントのデータであったり、特定の住所のユーザのみを抽出したり）  
そこで、実データに近いサンプルデータを用いてPolarsでこれら文字列処理を行う関数の実行例をまとめてみました。  

Polarsの文字列処理関数（正確には、PolarsのExpression）全体については公式のAPIリファレンスを参照してください。  

- [Polars String API reference](https://pola-rs.github.io/polars/py-polars/html/reference/expressions/string.html "polars_string")

リファレンスを見ても分かるように、Polarsの文字列処理関数は基本的に `str` というプレフィックスをつけて呼び出します。  
このような感じですね(各々の関数については後で説明します)。

```Python
df.select([
    pl.col("A").str.starts_with("test"),
    pl.col("B").str.contains(r"cat|dog")
])
```

今のところ、個人的によく使う関数は以下の10個くらいでしょうか。  

| 関数名 | 説明 |
| --- | --- |
| `length()` | 文字列の長さを返す |
| `contains(pattern: str)` | 文字列が指定されたパターンを含むかどうかを判定する |
| `starts_with(prefix: str)` | 文字列が指定された文字列（ `prefix` ）で始まるかどうかを判定する |
| `ends_with(suffix: str)` | 文字列が指定された文字列（ `suffix` ）で終わるかどうかを判定する |
| `split_by_char(char: str)` | 指定された文字で文字列を分割し、リストとして返す |
| `slice(start: int, end: Optional[int])` | 指定された範囲内の文字列を抽出する |
| `replace(substring: str, replacement: str)` | 文字列内の指定された部分文字列（ `substring` ）を別の文字列に置き換える |
| `trim()` | 文字列の前後から空白を削除する |
| `to_lowercase()` | 文字列をすべて小文字に変換する |
| `to_uppercase()` | 文字列をすべて大文字に変換する |

上記全てとはいきませんが、この後で実データに近い形のデータでこれらの関数を使ってみます。  

## サンプルデータ

今回はサンプルデータとしてデータサイエンティスト協会が公開している [データサイエンス100本ノック（構造化データ加工編）](https://github.com/The-Japan-DataScientist-Society/100knocks-preprocess "100knocks") の問題を用います。  
このような質の良いデータと演習問題を公開してくれるのはとてもありがたいですね。  

一応書籍化もされています。手元に置いておきたい方はこちらをどうぞ。  

- [データサイエンス100本ノック構造化データ加工編ガイドブック](https://www.amazon.co.jp/dp/B0BVRHHK7P/ "100knocks_book")

公式の説明では、dockerイメージをビルドすることで演習用の環境を構築する手順が示されています。  
ただ、今はとりあえずPolarsでデータを弄ることができれば良いので、ローカル環境にデータだけ持ってきて作業します。  
上記リンク先のリポジトリを適当な場所にクローンすると、 `docker/work/data/` 以下にデータがcsvで入っています。  
今回はそちらを読み込んで作業しました。  
最大でも10MB程度のファイルなので、ローカルでも十分作業できると思います。  

ちなみに `docker/work/answer` 以下に正答例が入っているので、答えの確認もできますね。  
（ただしPythonでの正答例はPandasで書かれています。）  

上記の中で文字列処理関連の問題は `customer.csv` と `store.csv` のみを用いています。  
`customer.csv` は以下のようなデータとなっています。  

| customer_id | ... | birth_day | ... | application_date | ... |
| --- | --- | --- | --- | --- | --- |
| CS021313000114 | ... | 1981-04-29 | ... | 20150905 | ... |
| CS037613000071 | ... | 1952-04-01 | ... | 20150414 | ... |

`birth_day` と `application_date` 、同じ日付のはずなのにフォーマットが異なってますね...  
実データで遭遇しそうな面倒くささまで表現しているとても良いデータ、とも言えるでしょうか。  

`store.csv` は以下のようなデータとなっています。  

| store_cd | store_name | prefecture_cd | prefecture | ... |
| --- | --- | --- | --- | --- |
| S12014 | 千草台店 | 12 | 千葉県 | ... |
| S13002 | 国分寺店 | 13 | 東京都 | ... |

店舗の地域の抽出などで文字列処理が活躍しそうですね。  

## Polarsで100本ノックの問題を解いてみる

まずはPythonでPolarsを使えるようにしましょう。
動作環境はPythonが動けば何でも良いです。[Google Colaboratory](https://colab.research.google.com/?hl=ja "colab") がパッと使える環境でしょうか。  

Polarsのインストール自体はpipで一発です。  

```bash
python -m pip install polars
```

さて、では実際に100本ノック中の文字列処理関連の問題を解きつつ関数の解説を行っていきます。  

### P-010

店舗データ（df_store）から、店舗コード（store_cd）が"S14"で始まるものだけ全項目抽出し、10件表示せよ。

```Python
df_store.filter(
    pl.col("store_cd").str.starts_with("S14")
).head(10)
```

Polarsではデータ抽出に [filter](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.filter.html#polars.DataFrame.filter "filter") を用います。  
「 `store_cd` が"S14"で始まる」という条件なので、 `starts_with("S14")` でそれを表現しているわけですね。  

`pl.col("store_cd")` という列の指定方法が初見だと分かりづらいかもしれませんが、初めはPolarsの列選択の作法だと覚えてしまった方が良いでしょう。  
慣れてくるとこの指定方法のおかげでとても柔軟かつシンプルに処理を書けることに気づくと思います。  

### P-011

顧客データ（df_customer）から顧客ID（customer_id）の末尾が1のものだけ全項目抽出し、10件表示せよ。  

```Python
df_customer.filter(
    pl.col("customer_id").str.ends_with("1")
).head(10)
```

この問題は先ほどとほぼ同様です。  
条件が末尾の文字列の指定なので、 `ends_with()` を用います。  

### P-012

店舗データ（df_store）から、住所 (address) に"横浜市"が含まれるものだけ全項目表示せよ。  

```Python
df_store.filter(
    pl.col("address").str.contains("横浜市")
)
```

「特定の文字列が含まれるか否か」は `str.contains()` で表現します。  

### P-013

顧客データ（df_customer）から、ステータスコード（status_cd）の先頭がアルファベットのA〜Fで始まるデータを全項目抽出し、10件表示せよ。  

```Python
df_customer.filter(
    pl.col("status_cd").str.contains(r"^[A-F]")
).head(10)
```

`str.contains()` は引数に正規表現のパターンを取ることもできます。  
「先頭がアルファベットのA~Fで始まる」なので、対応する正規表現は `^[A-F]` となります。  

ここで、文字列が正規表現であることを明示するためにパターンの先頭に `r` を付けていますが、 `str.contains()` の引数はデフォルトで正規表現として解釈されるため必須ではありません。  

### P-014

顧客データ（df_customer）から、ステータスコード（status_cd）の末尾が数字の1〜9で終わるデータを全項目抽出し、10件表示せよ。  

```Python
df_customer.filter(
    pl.col("status_cd").str.contains(r"[1-9]$")
).head(10)
```

こちらも先ほどと同様ですね。  
末尾が数字という条件なので `[1-9]$` が対応する正規表現となります。  

### P-015

顧客データ（df_customer）から、ステータスコード（status_cd）の先頭がアルファベットのA〜Fで始まり、末尾が数字の1〜9で終わるデータを全項目抽出し、10件表示せよ。  

```Python
df_customer.filter(
    pl.col("status_cd").str.contains("^[A-F].*[1-9]$")
).head(10)
```

P-013, P-014の組み合わせですね。  
`.*` で任意の文字の繰り返しを表現します。  

### P-016

店舗データ（df_store）から、電話番号（tel_no）が3桁-3桁-4桁のデータを全項目表示せよ。  

```Python
df_store.filter(
    pl.col("tel_no").str.contains(r"^[0-9]{3}-[0-9]{3}-[0-9]{4}$")
)
```

`{3}` などで直前のパターンの指定回数の繰り返しを表現します。  

## まとめ

本記事ではPolarsの文字列処理の解説と実演を行いました。  
演習問題の解説がほぼ正規表現の説明になっていた気がしますが...  
実際正規表現を使うことも多いので良しとしておきましょう。  

この記事が皆様のお役に立てば幸いです。
