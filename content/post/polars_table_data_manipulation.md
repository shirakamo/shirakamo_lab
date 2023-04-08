---
title: "Polarsでのテーブルデータ操作方法解説"
date: 2023-04-08T00:05:46+09:00
description: "Polarsでテーブルデータを操作するための主要な関数を説明します"
draft: false
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1680964935/shirakamo_lab_tech_blog/polars_table_manipulation/blog_thumbnails_v7ltji.webp"
categories:
- "データ分析"
tags:
- "Python"
- "Polars"
---

[前回の記事]({{< ref "/post/polars_string_processing.md" >}}) ではPolarsでの文字列処理をメインに解説しました。  
今回はPolarsが本領を発揮するテーブルデータに対する操作について説明していきます。  

テーブルデータ操作の内容は時と場合とデータによって様々考えられますが、おおよそ次に挙げる操作を押さえておけば多くの場面で適用できると思います。  

1. 条件に応じたデータ抽出
2. テーブル同士の結合操作
3. 特定のカテゴリ内での集計操作（顧客ごとに売上実績を合計するなど）
4. 計算結果を新たな列として作成する操作

初めにPolarsをインストールしてインポートしておきましょう。  

```Python
!python -m pip install polars

import polars as pl
```

## 列の追加

Polarsでデータに列を追加するには `with_columns()` を用います。  
既存の列から何かしらの計算をした結果を新たな列として追加するなど、多くの場面で用いる関数です。  
以下では1つの列に1を足した新たな列を作成する例です。

```Python
# データフレームの作成
df = pl.DataFrame({
    'a': [1, 2, 3],
    'b': [4, 5, 6]
})

# 新しい列を追加する
df = df.with_columns([
    (pl.col('b') + 1).alias('c')
])

print(df)
```

出力結果

```Python
│ a   ┆ b   ┆ c   │
│ --- ┆ --- ┆ --- │
│ i64 ┆ i64 ┆ i64 │
╞═════╪═════╪═════╡
│ 1   ┆ 4   ┆ 5   │
│ 2   ┆ 5   ┆ 6   │
│ 3   ┆ 6   ┆ 7   │
```

## 条件に応じたデータの抽出

Polarsにおいてデータフレームから特定の列を抽出するには `select` を、条件に応じて行を抽出するには `filter` を用います。  

### select

Polarsで特定の列を抽出するために用います。  

[polars.DataFrame.select](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.select.html#polars.DataFrame.select "select")  

以下は上記の `with_columns` と同様のデータを用いた例です。  
`select` は既存の列を抽出するだけでなく、既存の列に何かしらの演算を加えた列を新たな名前で抽出することもできます。

```Python
# データフレームの作成
df = pl.DataFrame({
    'a': [1, 2, 3],
    'b': [4, 5, 6]
})

# 列を選択する
df = df.select([
    'b',
    (pl.col('b') + 1).alias('c')
])

print(df)
```

出力結果

```Python
│ b   ┆ c   │
│ --- ┆ --- │
│ i64 ┆ i64 │
╞═════╪═════╡
│ 4   ┆ 5   │
│ 5   ┆ 6   │
│ 6   ┆ 7   │
```

もしかしたら `with_columns` と `select` の違いが分からなくなるかもしれませんが、 `with_columns` は __データフレームに列を追加__ するのに対して `select` は __データフレームから列を抽出する__ という違いがあります。

### filter

filterとは、条件に合致する行を抽出する機能です。  

[polars.DataFrame.filter](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.filter.html#polars.DataFrame.filter "filter")  

以下は、商品名が「A」の行だけを抽出する例です。

```Python
# データの作成
data = {'product': ['A', 'B', 'A', 'C', 'B', 'A'],
        'sales': [100, 200, 150, 300, 250, 200]}
df = pl.DataFrame(data)

# filterによる行の抽出
result = df.filter(pl.col('product') == 'A')

print(result)
```

出力結果:

```Python
shape: (3, 2)
┌─────────┬───────┐
│ product ┆ sales ┆
│ ---     ┆ ---   ┆
│ str     ┆ i64   ┆
╞═════════╪═══════╡
│ "A"     ┆ 100   ┆
├─────────┼───────┤
│ "A"     ┆ 150   ┆
├─────────┼───────┤
│ "A"     ┆ 200   ┆
└─────────┴───────┘
```

## テーブルの結合

Polarsにおけるテーブルの結合には `join` を用います。  

[polars.DataFrame.join](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.join.html#polars.DataFrame.join "join")  

### inner join

以下は商品情報と売り上げデータを結合する例です。  
厳密には以下の例はinner joinと呼ばれ、両方のテーブルに存在するレコードを紐づけます。  
(joinのデフォルトがinner joinです)

```Python
# 商品情報のデータ作成
product_data = {'product_id': [1, 2, 3],
                'product_name': ['A', 'B', 'C']}
product_df = pl.DataFrame(product_data)

# 売り上げデータのデータ作成
sales_data = {'product_id': [1, 2, 2, 3, 3, 4],
              'sales': [100, 200, 150, 300, 250, 200]}
sales_df = pl.DataFrame(sales_data)

# joinによるデータの結合
result = sales_df.join(product_df, on='product_id')

print(result)
```

出力結果:

```Python
shape: (5, 3)
│ product_id ┆ sales ┆ product_name │
│ ---        ┆ ---   ┆ ---          │
│ i64        ┆ i64   ┆ str          │
╞════════════╪═══════╪══════════════╡
│ 1          ┆ 100   ┆ A            │
│ 2          ┆ 200   ┆ B            │
│ 2          ┆ 150   ┆ B            │
│ 3          ┆ 300   ┆ C            │
│ 3          ┆ 250   ┆ C            │
```

`join`メソッドによって、商品情報と売り上げデータが結合されました。  

テーブルの結合はその結合方法によっていくつかバリエーションがあります。  
以下ではよく用いるものについて解説します。  
全ての結合方法については公式ページを参照ください。  

[polars.DataFrame.join](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.join.html#polars.DataFrame.join "join")  

### left join

left joinは、左側のテーブルにある全てのレコードと、右側のテーブルにある該当するレコードを結合します。  
右側に該当するレコードがない場合は、NULL値が入ります。

以下は、売り上げデータと商品情報をleft joinする例です。

```Python
# 商品情報のデータ作成
product_data = {'product_id': [1, 2, 3],
                'product_name': ['A', 'B', 'C']}
product_df = pl.DataFrame(product_data)

# 売り上げデータのデータ作成
sales_data = {'product_id': [1, 2, 2, 3, 3, 4],
              'sales': [100, 200, 150, 300, 250, 100]}
sales_df = pl.DataFrame(sales_data)

# left joinによるデータの結合
result = sales_df.join(product_df, on='product_id', how='left')

print(result)
```

出力結果

```Python
│ product_id ┆ sales ┆ product_name │
│ ---        ┆ ---   ┆ ---          │
│ i64        ┆ i64   ┆ str          │
╞════════════╪═══════╪══════════════╡
│ 1          ┆ 100   ┆ A            │
│ 2          ┆ 200   ┆ B            │
│ 2          ┆ 150   ┆ B            │
│ 3          ┆ 300   ┆ C            │
│ 3          ┆ 250   ┆ C            │
│ 4          ┆ 100   ┆ null         │
```

### cross join

cross joinは、左側のテーブルの全てのレコードに、右側のテーブルの全てのレコードを結合する機能です。  
個人的には、何らかのマスタデータのようなものを作成したいときに使うことが多いでしょうか。  
以下は、商品情報と売り上げデータをcross joinする例です。

```Python
# 行が省略されないようにする
pl.Config.set_tbl_rows(-1)

# 商品情報のデータ作成
product_data = {'product_id': [1, 2, 3],
                'product_name': ['A', 'B', 'C']}
product_df = pl.DataFrame(product_data)

# 売り上げデータのデータ作成
sales_data = {'product_id': [1, 2, 2, 3, 3],
              'sales': [100, 200, 150, 300, 250]}
sales_df = pl.DataFrame(sales_data)

# cross joinによるデータの結合
result = product_df.join(sales_df, how="cross")

print(result)
```

出力結果:

```Python
shape: (15, 4)
│ product_id ┆ product_name ┆ product_id_right ┆ sales │
│ ---        ┆ ---          ┆ ---              ┆ ---   │
│ i64        ┆ str          ┆ i64              ┆ i64   │
╞════════════╪══════════════╪══════════════════╪═══════╡
│ 1          ┆ A            ┆ 1                ┆ 100   │
│ 1          ┆ A            ┆ 2                ┆ 200   │
│ 1          ┆ A            ┆ 2                ┆ 150   │
│ 1          ┆ A            ┆ 3                ┆ 300   │
│ 1          ┆ A            ┆ 3                ┆ 250   │
│ 2          ┆ B            ┆ 1                ┆ 100   │
│ 2          ┆ B            ┆ 2                ┆ 200   │
│ 2          ┆ B            ┆ 2                ┆ 150   │
│ 2          ┆ B            ┆ 3                ┆ 300   │
│ 2          ┆ B            ┆ 3                ┆ 250   │
│ 3          ┆ C            ┆ 1                ┆ 100   │
│ 3          ┆ C            ┆ 2                ┆ 200   │
│ 3          ┆ C            ┆ 2                ┆ 150   │
│ 3          ┆ C            ┆ 3                ┆ 300   │
│ 3          ┆ C            ┆ 3                ┆ 250   │
```

`cross_join`メソッドによって、商品情報と売り上げデータの全てのレコードが結合されました。

## 特定カテゴリ内での集計操作

特定のカテゴリでデータをグループ化して集計するには `groupby` を用います。  

[polars.DataFrame.groupby](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.groupby.html#polars.DataFrame.groupby "groupby")  

以下は、商品情報と売り上げデータを作成し、商品IDごとに売り上げを集計する例です。  

```Python
# 商品情報のデータ作成
product_data = {'product_id': [1, 2, 3, 4, 5],
                'product_name': ['A', 'B', 'C', 'D', 'E']}
product_df = pl.DataFrame(product_data)

# 売り上げデータのデータ作成
sales_data = {'product_id': [1, 2, 2, 3, 3, 3, 4, 5],
              'sales': [100, 200, 150, 300, 250, 200, 50, 100]}
sales_df = pl.DataFrame(sales_data)

# 商品IDごとの売り上げを集計する
result = sales_df.groupby('product_id').agg(pl.sum("sales"))

print(result)
```

出力結果

```Python
shape: (5, 2)
│ product_id ┆ sales │
│ ---        ┆ ---   │
│ i64        ┆ i64   │
╞════════════╪═══════╡
│ 3          ┆ 750   │
│ 1          ┆ 100   │
│ 4          ┆ 50    │
│ 2          ┆ 350   │
│ 5          ┆ 100   │
```

上記は「product_idごとに、salesの値の和を取る」という操作を実現しています。  
`pl.sum("sales")` は `pl.col("sales").sum()` と書いても同様です。  

`sum()` の部分を他の計算に変えることで様々な計算を実現できます。  
よく使うのは以下でしょうか。  

- `mean()` : 平均を計算
- `var()` : 分散を計算
- `std()` : 標準偏差を計算
- `max(), min(), quantile(0.25)` : 最大値、最小値、第一分位点を計算

`agg(...)` にリストとして複数の集計操作を渡すことで複数の計算を行うこともできます。

## まとめ

よく使うテーブルデータ操作をPolarsで行う例を紹介しました。  
少しでも皆様のお役に立てば幸いです。  
