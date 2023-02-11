---
title: "Windows PCのセットアップした手順"
date: 2023-02-11T22:46:46+09:00
description: "Windows PCセットアップの作業ログ"
draft: false
categories:
- "ガジェット"
tags:
- "Windows"
---

やんごとなき理由によりWindows PCが必要になってしまいました。  
ということでマウスコンピュータのBTOで購入。  
この記事ではWindowsマシンでブログ執筆環境 + 開発環境を構築する手順を備忘録として残しておきます。  

## PC のスペック

スペックはこの後の作業には影響しないと思うのですが、一応書いておきます。  
作業としては普通の用途 + プログラミング、ブログ執筆程度。  
そのため一般的なスペックかつ GPU なしの構成にしました。  
その中で予算ギリギリまでメモリなどをカスタマイズしています。  

スペック
|----|----|
| 機種 | mouse DT5 |
| OS | Windows11 Home 64 bit |
| CPU | Core i5-11400 |
| メモリ | 16GB |
| ストレージ | 256GB NVM Express SSD |

今はもうOSはWindows11なんですね。  
時代の移り変わりを感じます......  
ついでにバックアップ用に512GBのHDDで安いのを買いました。  

実は初めてデスクトップPCを買ったんですよね。  
デスクトップPCの場合、電源ユニットが内蔵されています（当然）。  
結果、電源ユニットとコンセント結ぶケーブルだけではコンセントに届かず、追加で延長ケーブルを購入する羽目になりました......  

ノートPCの場合、電源ユニットのケーブルのおかげで延長ケーブルいらなかったんですよね。  
(伝わっているか心配ですが)  
今後もメイン PC はデスクトップにする予定なので、これは新たな発見でした。

<!-- START MoshimoAffiliateEasyLink -->
<script type="text/javascript">
(function(b,c,f,g,a,d,e){b.MoshimoAffiliateObject=a;
b[a]=b[a]||function(){arguments.currentScript=c.currentScript
||c.scripts[c.scripts.length-2];(b[a].q=b[a].q||[]).push(arguments)};
c.getElementById(a)||(d=c.createElement(f),d.src=g,
d.id=a,e=c.getElementsByTagName("body")[0],e.appendChild(d))})
(window,document,"script","//dn.msmstatic.com/site/cardlink/bundle.js?20220329","msmaflink");
msmaflink({"n":"エレコム 電源タップ 1m シャッタータップ 3個口 ほこり防止 コンセント 延長コード タップ OAタップ オフィス 延長ケーブル コード ホワイト T-ST02N-2310WH","b":"","t":"","d":"https:\/\/thumbnail.image.rakuten.co.jp","c_p":"\/@0_mall\/elecom\/cabinet","p":["\/s500_03\/t-st02n-2310wh_03.jpg","\/a200_10\/t-st02n-2310wh_01.jpg","\/s500_03\/t-st02n-2310wh_04.jpg"],"u":{"u":"https:\/\/item.rakuten.co.jp\/elecom\/4549550103107\/","t":"rakuten","r_v":""},"v":"2.1","b_l":[{"id":1,"u_tx":"楽天市場で見る","u_bc":"#f76956","u_url":"https:\/\/item.rakuten.co.jp\/elecom\/4549550103107\/","a_id":3829408,"p_id":54,"pl_id":27059,"pc_id":54,"s_n":"rakuten","u_so":1},{"id":2,"u_tx":"Yahoo!ショッピングで見る","u_bc":"#66a7ff","u_url":"https:\/\/shopping.yahoo.co.jp\/search?first=1\u0026p=%E3%82%A8%E3%83%AC%E3%82%B3%E3%83%A0%20%E9%9B%BB%E6%BA%90%E3%82%BF%E3%83%83%E3%83%97%201m%20%E3%82%B7%E3%83%A3%E3%83%83%E3%82%BF%E3%83%BC%E3%82%BF%E3%83%83%E3%83%97%203%E5%80%8B%E5%8F%A3%20%E3%81%BB%E3%81%93%E3%82%8A%E9%98%B2%E6%AD%A2%20%E3%82%B3%E3%83%B3%E3%82%BB%E3%83%B3%E3%83%88%20%E5%BB%B6%E9%95%B7%E3%82%B3%E3%83%BC%E3%83%89%20%E3%82%BF%E3%83%83%E3%83%97%20OA%E3%82%BF%E3%83%83%E3%83%97%20%E3%82%AA%E3%83%95%E3%82%A3%E3%82%B9%20%E5%BB%B6%E9%95%B7%E3%82%B1%E3%83%BC%E3%83%96%E3%83%AB%20%E3%82%B3%E3%83%BC%E3%83%89%20%E3%83%9B%E3%83%AF%E3%82%A4%E3%83%88%20T-ST02N-2310WH","a_id":3829423,"p_id":1225,"pl_id":27061,"pc_id":1925,"s_n":"yahoo","u_so":2}],"eid":"WY21S","s":"s"});
</script>
<div id="msmaflink-WY21S">リンク</div>
<!-- MoshimoAffiliateEasyLink END -->

## 初回立ち上げ後の作業

電源を入れた後に初期設定として行った作業を列挙します。  
1 以外は順不同です。  

1. PC 立ち上げ時の設定
    1. 国、言語の設定、PC 名の設定、Microsoft アカウントへのログインなど
    2. 基本的に指示された内容に従えばO.K.
2. 背景の変更
    1. 背景が真っ黄色だったので......
    2. デスクトップ上で右クリック → 個人用設定 → 背景をカスタマイズから好きなのを選ぶ
    3. 個人用設定 → 色 → モードを選ぶでダークモードを選択
    4. 個人用設定 → システム → ディスプレイ → 夜間モードからスケジュールをオン。夜間に色温度を下げるよう設定
3. エクスプローラーでファイルの拡張子を表示するように設定
    1. 表示 → 表示で表示されるメニューから「ファイルの拡張子」にチェック
    2. ついでに「隠しファイル」にもチェック
4. Google Chrome をインストール
    1. Edgeで「google chrome インストール」などで調べれば一発
    2. ダウンロードして実行すればインストールされます
5. Windows Terminal の起動
    1. Windows11 だとデフォルトでインストールされていました
    2. 検索 → windows terminal で起動してタスクバーにピン止め
6. VScode のインストール
    1. 「vscode インストール」などで調べれば一発
    2. [VSCode公式](https://code.visualstudio.com/) からダウンロードして exe ファイルを実行
    3. インストールして起動すると日本語用の言語パックをインストールするポップアップが出ました。言われるままにインストールして再起動。ありがたや

まあブラウザは Edge でも良いんですが。  
使い慣れている Chrome を使ってしまいますね。  

### VScode の拡張機能

VScode にいくつか拡張機能をインストールします。  
とりあえずセットアップ段階で必要なものを挙げます。  

- Vim
  - 好み
- Remote Development
  - この後で WSL2, Docker の設定を行うので
- Docker
- Python

## WSL2のセットアップ

WSL2をインストールし、Linux環境を構築します。  

### WSL2のインストール

[公式ページ](https://docs.microsoft.com/ja-jp/windows/wsl/install  "wsl2_install") の手順に従うだけです。  

1. タスクバーのアプリ検索で Windows Terminal を検索 → 右クリックで「管理者として実行」
2. 表示される Power Shell のプロンプトで `wsl --install` と入力
    - このコマンドだとデフォルトでwsl2、Ubuntuがインストールされます
    - なんかGUI関連もインストールされてるっぽいのですが、もしかしてGUIでも開ける？
3. 画面で再起動を指示されるので、再起動
4. 再起動すると自動的にWSL (Ubuntu) が起動する。ユーザ名とパスワードを聞かれるので各々入力

上記の方法でインストールすると、Windows Terminal から Ubuntu が開けるようになっているはずです。  
(タブ横の "+" から Ubuntu を選択)  

Ubuntu 以外のディストロを使う場合についても [公式ページ](https://docs.microsoft.com/ja-jp/windows/wsl/install  "wsl2_install") に記述があります。  

### Ubuntuのセットアップ

Ubuntuの初期設定をします。  
Windows TerminalでUbuntuを開きbashで以下を入力していきます。  

```bash
# パッケージアップデート
sudo apt update
sudo apt upgrade

# 日本語化
sudo apt -y install language-pack-ja
sudo update-locale LANG=ja_JP.UTF8
sudo apt -y install manpages-ja manpages-ja-dev
```

ロケールはデフォルトで日本になっている...気がします...  

ちなみに、Windows側のホームディレクトリは `/mnt/c/Users/<user name>` となります。

## Dockerのインストール

[Docker Desktop](https://docs.docker.com/desktop/ "docker_desktop") を利用して Docker 環境を構築します。  
Docker Desktop は商用利用では有料化されています。詳しくは [こちら](https://www.docker.com/blog/updating-product-subscriptions/ "docker_subscription")。  
しかし個人利用等では今まで通り無料で使えるので、今回はDocker Desktopを使用します。  

このブログで年間1,000万ドル以上の収益があがったら有料化することにします。  

作業自体は以下のサイトを参考にすればあっさりとできました。

- [Docker Desktop公式](https://docs.docker.com/desktop/windows/install/ "docker_install")
- [Microsoft公式](https://docs.microsoft.com/ja-jp/windows/wsl/tutorials/wsl-containers "microsoft_docker")

以下手順  

1. Docker Desktop公式から exeファイルをダウンロードして実行
2. `Configuration` の画面で `Install required Windows components for WSL2` にチェックが付いていることを確かめて `OK`
3. インストールが終了するとログアウトを求められるので、ログアウト
4. 再度ログインすると勝手にDocker Desktopが起動するので、右上の歯車ボタンから設定画面を開く
5. `General` で `Use the WSL 2 based engine (Windows Home can only run the WSL 2 backend)` にチェックがついていることを確認
6. `Resorces` -> `WSL Integration` でWSLインストール時のディストロを選択して `Applay&Restart`（Ubuntuをインストールしたので、`Ubuntu`と表示されているはず）
7. なぜか再起動が始まらなかったので、デスクトップ画面右下の `^` でアイコンバーを開いて Docker を強制再起動
8. Windows Terminal で Ubuntu を開いて `docker --version` と入力して Docker のバージョンが帰ってきたらO.K.

Docker Desktop を使わずに、 Ubuntu 内で CLI の Docker を使うこともできるようです。  
[Windows 10 + WSL 2 でDocker環境を構築する（Docker Desktop有料化対策）](https://blog.ecbeing.tech/entry/2021/09/07/150000 "wsl_docker")

## Hugoブログ執筆環境構築

このブログは [Hugo](https://gohugo.io/ "hugo") でウェブサイトを作成しているので、HugoをWSL2にインストールします。  
UbuntuのデフォルトのリポジトリではHugoのバージョンが古いらしいので、公式が配布しているものをとってきてインストールします。

- [Hugo Releases](https://github.com/gohugoio/hugo/releases "hugo_releases")

作業時点の最新版の `hugo_0.98.0_Linux-64bit.deb` をダウンロードしました。  
WSLも普通のLinuxので良いのか...？と思いましたが、今のところ問題なく使えています。  

Ubuntu での作業手順は以下です。  

``` bash
# ダウンロードしたdebファイルをホームに移動
mv /mnt/c/Users/<user name>/hugo_0.98.0_Linux-64bit.deb ./
# aptでインストール
sudo apt install ./hugo_0.94.2_Linux-64bit.deb
# バージョンが返ってきたらインストール成功
hugo version
```

ブログ本体はgithubのリポジトリとしてあげているので、githubにssh接続できるようにします。  
この作業は [githubのドキュメント](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/about-ssh "git_ssh") を参照してください。  

## Python のセットアップ

Python に関しては何か試したいときは Colaboratory、ローカルで作業するときは Docker を使うつもりなので、そこまで環境は作りこみません。  
デフォルトで python3.8.10 が入っていたので、とりあえず pip と venv を使えるようにしておきます。  

1. pip, venv をインストール

    ```bash
    sudo apt install python3-pip
    sudo apt install python3.8-venv
    ```

2. venv で仮想環境が作れるか確認

    ```bash
    python3 -m venv sample_env
    source sample_env/bin/activate
    # (sample_env) $ # こんな風に先頭に仮想環境名が出たらO.K.
    # 仮想環境終了
    deactivate
    ```

pip が使えて仮想環境を作れることがわかったのでO.K.

## Rのセットアップ

R は WSL のローカルにインストールし、RStudio Server から使います。  
Python の Jupyter Lab と同じ要領ですね。  
ということで R, RStudio Server の最新版をダウンロードしてインストールします。  

参考にした URL

- [RStudio公式](https://support.rstudio.com/hc/en-us/articles/360049776974-Using-RStudio-Server-in-Windows-WSL2)
- [WSL2でcmdstanrを動かす環境を整備しよう](https://norimune.net/3548)

以下手順  

1. 最新の R をインストール

    ```bash
    # 公開鍵のインストール
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9

    # リポジトリの追加（日本のミラーサイトを使う場合）
    sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/'

    # データーベースの更新
    sudo apt update

    # RとRStudio serverなどその他必要なライブラリのインストール
    sudo apt install -y r-base r-base-core r-recommended r-base-dev gdebi-core build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev

    # 動作確認
    R 
    > R version 4.1.3 ...
    ```

2. RStudio Server をインストール

    ```bash
    wget https://rstudio.org/download/latest/stable/server/bionic/rstudio-server-latest-amd64.deb
    sudo gdebi rstudio-server-latest-amd64.deb

    # 動作確認
    sudo rstudio-server start
    # http://localhost:8787へアクセス
    # sudoで立ち上げているため、sudo権限のユーザ名とパスワードを入力する
    # Rstudioの画面にアクセスできることを確認する

    # Rstudio終了
    sudo rstudio-server stop
    ```

3. 必要なパッケージをインストール。RStudio Server の R プロンプトで作業する

    ```bash
    # cmdstanrをインストール
    > install.packages("devtools")
    > install.packages("rstan")
    > devtools::install_github("stan-dev/cmdstanr")
    > library(cmdstanr)
    > install_cmdstan()
    > set_cmdstan_path()

    # tidyverseインストール
    > install.packages("tidyverse")
    ```

`tidyverse` のインストール中に以下のようなエラーが出ました。  

```bash
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to create bus connection: ホストが落ちています
```

これは WSL と Linux システムで PID のつけ方？が違うことに起因するようです。  
R の動作に限れば問題なさそうなので放っておきます。  

ベイズモデリング用に `rstan` というか `cmdstanr` をインストールしています。  
`cmdstanr` については [こちら](https://norimune.net/3548 "wsl_cmdstanr") を参照してください。

## バックアップ

HDD買ってきたのでバックアップを作成しておきます。  

### システムイメージ保存

PCが故障するなど、ヤバい時用。  
以下の記事を参考に HDD にシステムイメージを保存しました。  

- [無料で手軽に自動バックアップができる、Windows標準搭載のバックアップ機能とは](https://www.buffalo.jp/topics/trouble/detail/recovery_0020.html "windows_backup")

30分くらいかかりました。  

### WSL 環境のエクスポート

WSL環境を簡単にエクスポートできると分かったので、tar ballで書き出しておきます。  
以下の URL を参考にしました。

- [Windows 10 WSL2 Ubuntuのデータをバックアップするには](https://denor.jp/windows-10-wsl2-ubuntu%E3%81%AE%E3%83%87%E3%83%BC%E3%82%BF%E3%82%92%E3%83%90%E3%83%83%E3%82%AF%E3%82%A2%E3%83%83%E3%83%97%E3%81%99%E3%82%8B%E3%81%AB%E3%81%AF "wsl_backup")

以下手順

1. ディストロ確認

    ```bash
    (PowerShell) > wsl -l 
    Ubuntu (既定)
    docker-desktop
    docker-desktop-data
    ```

    なんかdockerのも混じってる。とりあえずUbuntuが正解だろう...

2. tar ballで書き出し

    ```bash
    (PoweShell) > wsl --export Ubuntu Ubuntu_20220319.tar
    ```

3. 上のコマンドだとユーザのフォルダ以下に保存されるので、それを外付けHDDにポイして保存

この作業は定期的にやってもよさそうですね。  
