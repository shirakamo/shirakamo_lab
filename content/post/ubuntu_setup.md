---
title: "新PCにUbuntuをインストールしてセットアップした手順"
date: 2023-02-11T22:36:23+09:00
description: "Ubuntuセットアップの作業ログ"
draft: false
categories:
- "エンジニアリング"
tags:
- "Linux"
---

5年選手の旧PCがずっとファンファン言うようになったため、思い切って新PCとしてミニPCのLenovo Think Centre M73 Tinyの再生品をアマゾンから購入しました。  

CPUがCore-i5、メモリ8GB、ディスクSSD 256GBなのでデフォルトのwindows10でそのまま使っても良かったのですが、再生品のためかすでにwindowsのユーザ設定が行われていた状態でちょっと気持ち悪かったのでUbuntuをクリーンインストールすることにしました。  

今までも何度かWindows PCにUbuntuをインストールしてきたのですが、そのたびに毎回同じことを同じページで調べているような気がしてきたので、これを期に手順をまとめて備忘録として残しておきます。  
といってもほとんど偉大な先人たちが残してくれたウェブ上の情報をコピペしているだけなので、参考URL集みたいになってしまいました。

<!-- START MoshimoAffiliateEasyLink -->
<script type="text/javascript">
(function(b,c,f,g,a,d,e){b.MoshimoAffiliateObject=a;
b[a]=b[a]||function(){arguments.currentScript=c.currentScript
||c.scripts[c.scripts.length-2];(b[a].q=b[a].q||[]).push(arguments)};
c.getElementById(a)||(d=c.createElement(f),d.src=g,
d.id=a,e=c.getElementsByTagName("body")[0],e.appendChild(d))})
(window,document,"script","//dn.msmstatic.com/site/cardlink/bundle.js?20220329","msmaflink");
msmaflink({"n":"レノボ 超ミニPC ThinkCentre M700 Tiny、Microsoft Office 2019搭載\/Intel Core i5-6500T\/ RAM:DDR4 8GB\/高速SSD:256GB\/ 静音性\/Bluetooth\/光学ドライブ\/4K HDR対応\/\/Windows 11搭載(整備済み品)","b":"karamel","t":"Lenovo ThinkCentre M700 Tiny","d":"https:\/\/m.media-amazon.com","c_p":"\/images\/I","p":["\/41BeLVrFqdL._SL500_.jpg","\/41SVz5BvQBL._SL500_.jpg","\/41jsF3C+3zL._SL500_.jpg","\/41NqfifQqFL._SL500_.jpg","\/41tgUTDaiTL._SL500_.jpg","\/41A5FajawlL._SL500_.jpg","\/41fwHMyXaGL._SL500_.jpg"],"u":{"u":"https:\/\/www.amazon.co.jp\/dp\/B0B528FWCP","t":"amazon","r_v":""},"v":"2.1","b_l":[{"id":1,"u_tx":"Amazonで見る","u_bc":"#f79256","u_url":"https:\/\/www.amazon.co.jp\/dp\/B0B528FWCP","a_id":3885217,"p_id":170,"pl_id":27060,"pc_id":185,"s_n":"amazon","u_so":1},{"id":2,"u_tx":"楽天市場で見る","u_bc":"#f76956","u_url":"https:\/\/search.rakuten.co.jp\/search\/mall\/%E3%83%AC%E3%83%8E%E3%83%9C%20%E8%B6%85%E3%83%9F%E3%83%8BPC%20ThinkCentre%20M700%20Tiny%E3%80%81Microsoft%20Office%202019%E6%90%AD%E8%BC%89%2FIntel%20Core%20i5-6500T%2F%20RAM%3ADDR4%208GB%2F%E9%AB%98%E9%80%9FSSD%3A256GB%2F%20%E9%9D%99%E9%9F%B3%E6%80%A7%2FBluetooth%2F%E5%85%89%E5%AD%A6%E3%83%89%E3%83%A9%E3%82%A4%E3%83%96%2F4K%20HDR%E5%AF%BE%E5%BF%9C%2F%2FWindows%2011%E6%90%AD%E8%BC%89(%E6%95%B4%E5%82%99%E6%B8%88%E3%81%BF%E5%93%81)\/","a_id":3829408,"p_id":54,"pl_id":27059,"pc_id":54,"s_n":"rakuten","u_so":2},{"id":3,"u_tx":"Yahoo!ショッピングで見る","u_bc":"#66a7ff","u_url":"https:\/\/shopping.yahoo.co.jp\/search?first=1\u0026p=%E3%83%AC%E3%83%8E%E3%83%9C%20%E8%B6%85%E3%83%9F%E3%83%8BPC%20ThinkCentre%20M700%20Tiny%E3%80%81Microsoft%20Office%202019%E6%90%AD%E8%BC%89%2FIntel%20Core%20i5-6500T%2F%20RAM%3ADDR4%208GB%2F%E9%AB%98%E9%80%9FSSD%3A256GB%2F%20%E9%9D%99%E9%9F%B3%E6%80%A7%2FBluetooth%2F%E5%85%89%E5%AD%A6%E3%83%89%E3%83%A9%E3%82%A4%E3%83%96%2F4K%20HDR%E5%AF%BE%E5%BF%9C%2F%2FWindows%2011%E6%90%AD%E8%BC%89(%E6%95%B4%E5%82%99%E6%B8%88%E3%81%BF%E5%93%81)","a_id":3829423,"p_id":1225,"pl_id":27061,"pc_id":1925,"s_n":"yahoo","u_so":3}],"eid":"sJnSp","s":"s"});
</script>
<div id="msmaflink-sJnSp">リンク</div>
<!-- MoshimoAffiliateEasyLink END -->
<br>

## 旧PCでUbuntuの起動ディスクを作成

起動ディスクと言っていますがUSBメモリでやりました。  

旧PC (Windows10) にてまずはUSBメモリにUbuntuのインストールメディアを作成します。  
今回は20.04LTSを選択しました。  

参考URL：[WindowsでUbuntuのインストールUSBメディアを作成する](https://diagnose-fix.com/topic2-003/ "make_ubuntuUSBmedia")  

詰まりポイントはなかったのですが、旧PCに自分で [Rufus](https://rufus.ie/ja/ "Rufus") をインストールする手順は前にやってなかったような...？
自分の環境だと1時間ちょいくらいかかったので気長に待ちます。

## 新PCにUbuntuをインストール

__新PCをネットに接続して__ 事前に先ほど作成したインストールUSBメディアを挿した後で新PCを起動します。  
Ubuntuインストール時にネットから色々ダウンロードするので、有線で接続。後から分かったことですが、自分が購入したPCだとubuntuを入れた際に必要な設定をしないとwifiを認識しないことが分かったので、これはナイス判断自分、でした。  

BIOSでいろいろやらなきゃ...と思っていたら何もせずともUbuntuの立ち上げ画面になりました。便利。  
インストールするか、お試しでUSBメモリでUbuntuをブートするかを選べるので、今回はインストールを選択。  
デュアルブートにする予定はないので、Windows10を削除してインストールを選択。  
あとは画面上の指示（というか必要な選択はデフォルトでされていたので次へをクリックするだけ）に従えばインストールできました。  

インストールが終了すると再起動を求められるので、言われるままに再起動。  
起動時にUSBメディアを抜いた上でエンターを押せと言われるので、言われるままに実行。  
これでUbuntuのインストールは終了しました。

Ubuntuインストール直後は色々アップデートを求められるので、言われるままにアップデート。  
自分で端末を開いて、そこでも `sudo apt update && apt upgrade -y` で諸々パッケージ更新。  
これでUbuntuの最低限のセットアップは終了、今日は遅いし後は明日からやるか...と思ったら問題が発生しました。

## 問題1: シャットダウンできない問題

なぜかPCをシャットダウンしても再起動されるという問題が発覚。  
[こちら](https://guernicq.hatenablog.com/entry/2019/02/09/003111 "reboot_pci") のような対処をするも解決せず。  

どうやらOSの問題ではなく、BIOSでWake on LANが設定されていた模様。BIOSはいじってないので、もともとそのような設定がされていたようです。( [参考](https://plaza.rakuten.co.jp/nwnote/diary/201810140002/ "wake_on_lan") )  

このPCはサーバで使われることが多いのでしょうか？  

ということでBIOS画面を開こうとするも起動時にF12押したままにしたり画面上の指示に従ってエンター押したりするもなぜかBIOSへ入れず。  
これは詰んだかと思ったのですが、 [こちら](https://linux.just4fun.biz/?Ubuntu/%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%A7UEFI%E3%81%AE%E7%94%BB%E9%9D%A2%E3%82%92%E8%A1%A8%E7%A4%BA%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95 "reboot_bios") の記事にあるように起動後に端末から `sudo systemctl reboot --firmware-setup` と打つと再起動時にBIOSに入れました。  

BIOSで何をどうしたかはちょっと覚えていないのですが、電源オプション的なところから "Wake on LAN" を `disable` にするとかそのような操作をすると電源OFFできちんとシャットダウンできるようになりました。  

この問題には初めて遭遇したのでテンパりました...

## Ubuntuの必須ソフトのインストール

ブラウザはGoogle Chromeの方で色々設定して共有できるようにしてあるので、Chromeをインストールします。  
また、自分はブログを書いたりプログラミングをしたりするので、それ用に必要になるソフトをインストールします。

1. Ubuntu Softwareセンターで以下をインストール
    1. `clamtk` : ウイルス検知ソフト。`apt` でもインストールできるのですが、たまたま見つけたのでこちらで。
    1. `GNU Image Manupilation Program` : いわゆるGIMP。画像編集ソフト。ブログを書くときに簡単な画像編集を行うために。別に必須ではないのですが、このタイミングで一応入れておきました。
1. ホーム以下のディレクトリ名を英語にする。
    1. 日本語でUbuntuをセットアップするとホーム以下のディレクトリ（ダウンロードやらデスクトップやら）が日本語になっています。別に日本語でも良いのですが、端末から移動する際にわざわざ日本語変換したくないので英語に直します。
    1. 参考：[Ubuntuでホームディレクトリの中身を英語にする](https://qiita.com/taiko19xx/items/d1a001bfc25245b91354 "dir_eng")
    1. 下部の "never ask again" みたいなところにチェックを入れないと、Ubuntuを起動するたびに日本語に戻そうとされるのでチェックを入れます。  
1. Google Chromeのインストールとセットアップ
    1. FireFoxでchromeのダウンロード画面に行き、64bit ubuntu (debian)版をダウンロード。このとき、「実行」ではなく「保存」するようにする。（実行にすると「インストールできません」と言われる）
    1. `Download` ディレクトリにdebファイルがダウンロードされるので、 `~/Downloads` へ行き、 `sudo apt install -y ./<ダウンロードしたdebファイル>` とします。これでGoogle chromeがインストールされます。
    1. あとはchromeを開いてアカウント認証をすればもとの環境が再現されます（chrome側で共有設定をしていれば）。素晴らしいですね。
1. 開発に必要なパッケージのインストール
    1. `apt` で以下をインストールします。
        - `build-essentials`
        - `git`
        - `curl`
        - `wget`
        - `vim`
        - `tmux`
        - `emacs` : 自分はlisp (gauche scheme) で [project euler](https://projecteuler.net/ "euler") の数学の問題を解くのがちょっとした趣味です。なので、emacsをlisp専用IDEとして使っています。エディタは後でも説明しますがvscode使いです。
1. 設定ファイルをgitからclone
    1. 自分は.bashrcなどの設定ファイルをgitで管理しているので、それらをリポジトリからcloneします。
    1. cloneしたディレクトリ内の設定ファイルからシンボリックリンクを貼ります。 `ln -s ~/dotfiles/.bashrc ~/.bashrc` みたいな感じです。
1. 諸々のリポジトリをclone
    1. 作業しているリポジトリをcloneしておきます。

## 問題2：Wifiに繋げない

自分が購入したマシンは無線LAN非対応なので、外付けのwifi受信機が必要になります。  
もともと購入時についてきていたのですが、それをつけてもwifiに繋げない、というかwifi設定そのものが設定画面にないことに気づきました。  

どうもUbuntuは無線LAN非対応機に無線LANのドライバは入れてくれないようで、後から設定する必要があるようです。  
ここまで重いダウンロードが続くので有線に接続していたため、気づきもしない問題でした...  
まあ別に有線でつなぎ続けても良いのですが、サブ機として持っていたChromebookをサブモニターとして使いたいと思っていたため、wifiの設定も行うことにしました。  

こちらのページを参考に設定しました。 [Linuxで使えるUSB Wifiの探索 (Ubuntu 20.04LTS)](https://qiita.com/takaf05/items/5034e9234331ed6ecf2f "ubuntu_wifi")  
無線LANのレシーバもこちらのページに記載されている "TP-Link T2U" を新たに購入して使用しましたが、もしかしたら付属のレシーバでもできたのかもしれません...  

<!-- START MoshimoAffiliateEasyLink -->
<script type="text/javascript">
(function(b,c,f,g,a,d,e){b.MoshimoAffiliateObject=a;
b[a]=b[a]||function(){arguments.currentScript=c.currentScript
||c.scripts[c.scripts.length-2];(b[a].q=b[a].q||[]).push(arguments)};
c.getElementById(a)||(d=c.createElement(f),d.src=g,
d.id=a,e=c.getElementsByTagName("body")[0],e.appendChild(d))})
(window,document,"script","//dn.msmstatic.com/site/cardlink/bundle.js?20220329","msmaflink");
msmaflink({"n":"TP-Link WiFi 無線LAN 子機 AC600 433Mbps + 200Mbps Windows\/Mac OS 対応 ナノ設計 デュアルバンド 3年保証 Archer T2U Nano","b":"TP-Link","t":"Archer T2U Nano","d":"https:\/\/m.media-amazon.com","c_p":"\/images\/I","p":["\/41vJOq33DcL._SL500_.jpg","\/41yc-MA+J+L._SL500_.jpg","\/51ADKE3q9lL._SL500_.jpg","\/51ritwIatRL._SL500_.jpg","\/41HoVmmRnAL._SL500_.jpg","\/41tcHJcVNkL._SL500_.jpg","\/41-zHCZl6ZL._SL500_.jpg"],"u":{"u":"https:\/\/www.amazon.co.jp\/dp\/B07MXHJ6KB","t":"amazon","r_v":""},"v":"2.1","b_l":[{"id":1,"u_tx":"Amazonで見る","u_bc":"#f79256","u_url":"https:\/\/www.amazon.co.jp\/dp\/B07MXHJ6KB","a_id":3885217,"p_id":170,"pl_id":27060,"pc_id":185,"s_n":"amazon","u_so":1},{"id":2,"u_tx":"楽天市場で見る","u_bc":"#f76956","u_url":"https:\/\/search.rakuten.co.jp\/search\/mall\/TP-Link%20WiFi%20%E7%84%A1%E7%B7%9ALAN%20%E5%AD%90%E6%A9%9F%20AC600%20433Mbps%20%2B%20200Mbps%20Windows%2FMac%20OS%20%E5%AF%BE%E5%BF%9C%20%E3%83%8A%E3%83%8E%E8%A8%AD%E8%A8%88%20%E3%83%87%E3%83%A5%E3%82%A2%E3%83%AB%E3%83%90%E3%83%B3%E3%83%89%203%E5%B9%B4%E4%BF%9D%E8%A8%BC%20Archer%20T2U%20Nano\/","a_id":3829408,"p_id":54,"pl_id":27059,"pc_id":54,"s_n":"rakuten","u_so":2},{"id":3,"u_tx":"Yahoo!ショッピングで見る","u_bc":"#66a7ff","u_url":"https:\/\/shopping.yahoo.co.jp\/search?first=1\u0026p=TP-Link%20WiFi%20%E7%84%A1%E7%B7%9ALAN%20%E5%AD%90%E6%A9%9F%20AC600%20433Mbps%20%2B%20200Mbps%20Windows%2FMac%20OS%20%E5%AF%BE%E5%BF%9C%20%E3%83%8A%E3%83%8E%E8%A8%AD%E8%A8%88%20%E3%83%87%E3%83%A5%E3%82%A2%E3%83%AB%E3%83%90%E3%83%B3%E3%83%89%203%E5%B9%B4%E4%BF%9D%E8%A8%BC%20Archer%20T2U%20Nano","a_id":3829423,"p_id":1225,"pl_id":27061,"pc_id":1925,"s_n":"yahoo","u_so":3}],"eid":"zRc0w","s":"s"});
</script>
<div id="msmaflink-zRc0w">リンク</div>
<!-- MoshimoAffiliateEasyLink END -->

## pyenvでpythonインストール

デフォルトのpython環境を汚したくなかったので [pyenv](https://github.com/pyenv/pyenv "pyenv") でプログラミングをする際のpythonをインストールすることにしました。  
最近は `pipenv, poetry` などpipで入れられるパッケージ管理システムやdockerを使うほうがイマドキ感がある気もしますが、今までpyenvを使っていたのでなんとなく惰性で...  
とはいえUbuntu20.04特有の問題なのか、結構面倒くさかったです。  

参考URL : [Ubuntu 20.04 で pyenv を使用して Python を導入する](https://qiita.com/MusicScience37/items/4537233b840e2133cd32 "ubuntu_pyenv")  

pyenvのインストールした後にパスを通して `pyenv init` するとどのようなコマンドを `.bashrc` などに書けばよいかを教えてくれます。  
`.profile` に  

```bash
# pyenv
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init --path)"
fi
```

`.bashrc` に  

```bash
# pyenv
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```

を追記します。これでログアウト -> 再ログイン するとpyenvが使えるようになります。  
自分はここで何度か  

```bash
pyenv init no longer set path
```

みたいに怒られたのですが、再起動や再ログインしているうちに解消していました...謎です...

pythonは3.9.5をインストールしました。以下のようにインストールしました。  

```bash
pyenv install 3.9.5 # 数１０分くらい時間がかかります
pyenv global 3.9.5
```

私の記憶だと、ログイン時に実行されるのは `.bash_profile` だったはずなのですが、いつの間にか `.profile` なるファイルに置き換わっていたようです  
...やばい...世界に置いていかれている...  

## Visual Studio Codeのインストールとセットアップ

エディタは基本VSCodeを使っているのでインストールします。基本はGoogle Chromeと同じです。

1. [ダウンロードサイト](https://code.visualstudio.com/download "vscode_download") からマシンにあった `deb` パッケージをインストール
1. `~/Downloads` へ移動し、 `sudo apt install -y ./<ダウンロードしたdebファイル>`
1. 必要な拡張機能をインストール
    1. VSCodeを開いて以下の拡張機能をインストール。(他にも必要なものがあった気がするのですが、とりあえず思いついたものだけ)
        - `Vim`
        - `Python`
        - `Remove-development` (Remote-WSLは多分必要ありませんでした...)
        - `Git History`
        - `GitLens`
    1. `GitLens` はGithubの認証を必要とするので、出てくるアラートに従って認証。もしかしたら拡張機能をインストールした後VSCodeを再起動しないと認証が要求されないかもしれません。

場合によっては [日本語設定](https://qiita.com/nanamesincos/items/5c48ff88a4eeef0a8631 "japanize_vscode") や、[キーボードの設定](https://qiita.com/iceman5499/items/e651dcc7c4abb350d86e "vscode_key") をしたほうが良いかもしれません。  

## Node.js, npmのインストール

Javascript実行環境もそうですが、PythonのJupyteLabの拡張機能を入れるためにもNode.js, npmの最新版（安定版）をインストールします。  
例の如く `apt` でインストールされるのは古いバージョンなので、一度 `apt` でインストールした後に最新版にアップデートします。  

参考URL : [Ubuntuに最新のNode.jsを難なくインストールする](https://qiita.com/seibe/items/36cef7df85fe2cefa3ea "node_npm")

## Hugoのインストール

このブログはHugoでサイトを生成しています。  
なのでHugoがないとこのブログも書けない!!  
のでHugoをインストールします。  

[linux版のHomebrew](https://docs.brew.sh/Homebrew-on-Linux "homebrew") でインストールするのが一番楽そうなのですが、そこまできちっと管理する必要も無かろうということでリリースされたdebファイルからインストールします。  
基本はGoogle chromeやVSCodeと同じです。  

1. debファイルをダウンロード
    1. Hugoの [ダウンロードサイト](https://github.com/gohugoio/hugo/releases "hugo_releases") から自分のマシンにあったファイルをダウンロードする。今は64bit ubuntuなので、 `hugo_extended_0.83.1_Linux-64bit.deb` をダウンロード。(`extended` はSASS/SCSSコンパイルが可能です。おそらく自作のテンプレートを作成する際に必要になるかと。この機能が必要ない場合は `extended` でなくとも良いです。)
    1. ダウンロードディレクトリで `sudo apt install -y ./<ダウンロードしたdebファイル>
1. `hugo versions` などでインストールできたかテスト

ここまでで自分にとって必須の設定はほぼ終了しました。後は趣味の範囲なのでのんびりとやっていきます。  

## spacemacsのインストールと設定

前述の通り時々shcemeを書くのですが、今の所VSCodeよりemacsのほうがlisp系は快適に書ける&実行できると感じています（emacs自体、emacs lispで動いてますしね）。  

問題は基本私はVim使いということです。  
なので、 Emacs + Vimである [spacemacs](https://www.spacemacs.org/ "spacemacs") をlispのIDEとしてセットアップします。

参考URL  

- [Spacemacsに乗り換えてみた（１）導入編](https://qiita.com/shotakaha/items/da1282327f0dbc1b6289 "spacemacs_install")
- [Macへのspacemacsインストールメモ](https://qiita.com/crossroad0201/items/ca419e4dc2646667e669 "spacemacs_settings")

## gauche schemeのインストールとspacemacsの設定

schemeの処理系は色々あるのですが、作者が日本人の方で日本語のドキュメントが充実している `gauche scheme` を使っています。  
また、spacemacsにgauche用の設定をします。`gauche-mode` もあるのですが、とりあえずデフォルトのschemeモードを使っています。  

参考URL  

- [gaucheのインストール](http://practical-scheme.net/gauche/download-j.html "gauche_install")
- [EmacsでのGauche環境設定(Emacs for Gauche for Scheme for SICP)](https://propyon.hateblo.jp/entry/emac "gauche_emacs")

注意するのは、今回は spacemacs を使っているので、設定は `~/.spacemacs` の `user-init()` 関数内に記述します。  

## 今後やりたいこと

- Docker関係のセットアップ
- Android Studio関係のセットアップ
- [kite](https://www.kite.com/ "kite") の導入
  - AIベースのすごい賢い補完をしてくれるそうです。VSCode版があるそうなので、いずれ試してみたいです。
