---
title: "Chromebookをセットアップした手順"
date: 2023-02-11T22:26:31+09:00
description: "Chromebook PCセットアップの作業ログ"
draft: false
categories:
- "ガジェット"
tags:
- "Chromebook"
---

この記事ではChromebookで使えるLinux環境 "crostini" の日本語化とDockerのセットアップ方法を解説します。  
個人的な作業の備忘録です。  

皆さん、Chromebook使ってますか？  
安価なPCながらブラウジングから簡単なプログラミングまでこなせる案外使えるヤツです。  

最近はメイン機としてもChromebook を使っているのですが、  
Chromebook搭載のLinux環境 "crostini" は日本語に対応しておらず、辛いなーと思っていたので  
この度とうとう日本語化に臨むことにしました。  

ついでにDockerもインストールしてみます。  
けっこうChromebookの開発環境作るのって試行錯誤すること多いんですよね...  
そこでDocker上に開発環境を乗っけることにしました。  

## 環境

使用しているChromebookの環境です。  
OSのバージョンは設定時の最新にアップデートしておきました。  
Chromebook の環境構築は結構マシン依存する...? 気がします。  

| もの | 詳細 |
| ---- | ---- |
| マシン | ASUS Chromebox4 |
| CPU | Intel Core-i3-10110U (メモリ容量 8GB) |
| ストレージ | SSD 128GB |
| OSバージョン | ChromeOS バージョン: 91.0.4472.102（Official Build） （64 ビット） |

はい、ChromebookではなくChromeboxでした。  

<!-- START MoshimoAffiliateEasyLink -->
<script type="text/javascript">
(function(b,c,f,g,a,d,e){b.MoshimoAffiliateObject=a;
b[a]=b[a]||function(){arguments.currentScript=c.currentScript
||c.scripts[c.scripts.length-2];(b[a].q=b[a].q||[]).push(arguments)};
c.getElementById(a)||(d=c.createElement(f),d.src=g,
d.id=a,e=c.getElementsByTagName("body")[0],e.appendChild(d))})
(window,document,"script","//dn.msmstatic.com/site/cardlink/bundle.js?20220329","msmaflink");
msmaflink({"n":"ASUS Chromebox4 ミニPC 小型PC (Chrome OS\/コンパクト\/4K\/USB Type-C\/WiFi6\/DDR4-2400\/64GB SSD\/) ブラック","b":"ASUS","t":"CHROMEBOX4-GC018UN","d":"https:\/\/m.media-amazon.com","c_p":"\/images\/I","p":["\/31k-OvsPCIS._SL500_.jpg","\/411FeqTiJrS._SL500_.jpg","\/41YObjrPQ5S._SL500_.jpg","\/41+HviRkuGS._SL500_.jpg","\/41spi6KuTWS._SL500_.jpg","\/41sNcWLgXZS._SL500_.jpg","\/41+9OKYrsXS._SL500_.jpg","\/21-zLD4DXyS._SL500_.jpg","\/215qQ9mC-RS._SL500_.jpg","\/21BN9EWFXFS._SL500_.jpg","\/21wzQlaVRCS._SL500_.jpg","\/21H4QVXRuUS._SL500_.jpg","\/213kKzx+ADS._SL500_.jpg"],"u":{"u":"https:\/\/www.amazon.co.jp\/dp\/B093Q83D78","t":"amazon","r_v":""},"v":"2.1","b_l":[{"id":1,"u_tx":"Amazonで見る","u_bc":"#f79256","u_url":"https:\/\/www.amazon.co.jp\/dp\/B093Q83D78","a_id":3885217,"p_id":170,"pl_id":27060,"pc_id":185,"s_n":"amazon","u_so":1},{"id":2,"u_tx":"楽天市場で見る","u_bc":"#f76956","u_url":"https:\/\/search.rakuten.co.jp\/search\/mall\/ASUS%20Chromebox4%20%E3%83%9F%E3%83%8BPC%20%E5%B0%8F%E5%9E%8BPC%20(Chrome%20OS%2F%E3%82%B3%E3%83%B3%E3%83%91%E3%82%AF%E3%83%88%2F4K%2FUSB%20Type-C%2FWiFi6%2FDDR4-2400%2F64GB%20SSD%2F)%20%E3%83%96%E3%83%A9%E3%83%83%E3%82%AF\/","a_id":3829408,"p_id":54,"pl_id":27059,"pc_id":54,"s_n":"rakuten","u_so":2},{"id":3,"u_tx":"Yahoo!ショッピングで見る","u_bc":"#66a7ff","u_url":"https:\/\/shopping.yahoo.co.jp\/search?first=1\u0026p=ASUS%20Chromebox4%20%E3%83%9F%E3%83%8BPC%20%E5%B0%8F%E5%9E%8BPC%20(Chrome%20OS%2F%E3%82%B3%E3%83%B3%E3%83%91%E3%82%AF%E3%83%88%2F4K%2FUSB%20Type-C%2FWiFi6%2FDDR4-2400%2F64GB%20SSD%2F)%20%E3%83%96%E3%83%A9%E3%83%83%E3%82%AF","a_id":3829423,"p_id":1225,"pl_id":27061,"pc_id":1925,"s_n":"yahoo","u_so":3}],"eid":"n2WaR","s":"s"});
</script>
<div id="msmaflink-n2WaR">リンク</div>
<!-- MoshimoAffiliateEasyLink END -->
<br>

## corostini の日本語化

色々なサイト様を参考にしたのですが、うまく行かないことが多々あり...  
最終的にこちらのサイト様を参考にしたらできました。  

[Crostiniで日本語環境設定:健康で文化的な最低限度の生活のためのChromebook設定(2)](https://scrapbox.io/hada/Crostini%E3%81%A7%E6%97%A5%E6%9C%AC%E8%AA%9E%E7%92%B0%E5%A2%83%E8%A8%AD%E5%AE%9A:%E5%81%A5%E5%BA%B7%E3%81%A7%E6%96%87%E5%8C%96%E7%9A%84%E3%81%AA%E6%9C%80%E4%BD%8E%E9%99%90%E5%BA%A6%E3%81%AE%E7%94%9F%E6%B4%BB%E3%81%AE%E3%81%9F%E3%82%81%E3%81%AEChromebook%E8%A8%AD%E5%AE%9A(2) "crostini_japanise")  

以下、ほぼ上記サイト様に書いてあることと同様ですが...  
バックアップとして手順だけメモしておきます。  

### 手順

(1)crostiniターミナル環境の日本語化

```bash
# タイムゾーンの変更
sudo dpkg-reconfigure tzdata
# 矢印キーやマウスで "Asia" -> "Tokyo" を選択 -> "OK"

# ロケールや日本語フォントのインストール
sudo apt -y install task-japanese locales-all fonts-ipafont

# ロケールの変更
sudo localectl set-locale LANG=ja_JP.UTF-8 LANGUAGE="ja_JP:ja"
source /etc/default/locale
```

(2)日本語入力エンジンMOZCのインストール

```bash
sudo apt install fcitx-mozc
```

(3)fcitxの自動起動設定
`/etc/systemd/user/cros-garcon.service.d/cros-garcon-override.conf` に以下を追記。  

```bash
Environment="GTK_IM_MODULE=fcitx"
Environment="QT_IM_MODULE=fcitx"
Environment="XMODIFIERS=@im=fcitx"
```

`~/.sommelierrc` に以下を追記。  

```bash
/usr/bin/fcitx-autostart
```

(4)MOZCを入力メソッドに追加
以下のコマンドで入力メソッドの設定を起動。  

```bash
fcitx-configtool
```

GUIで設定ツールが立ち上がるので、入力メソッドに "MOZC 日本語" を追加。  
ここまでの設定がうまく行っていたら何もしなくてもすでに追加されていると思います。  

### crostiniの日本語化：補足

どうも一番最初の `dpkg-reconfigure tzdata` をしないと自分の環境ではうまく行かなかったようです。  
このコマンドは最近の Debian 系統の OS のタイムゾーンを変更する方法らしいです。  

[Ubuntu 16.04 (Xenial) 以降のタイムゾーン設定方法](https://please-sleep.cou929.nu/xenial-localtime.html "debian_locale")  

## Docker セットアップ

Docker のセットアップは [公式ページ](https://docs.docker.com/engine/install/debian/ "docker_debian")に従うと普通にできました。  
注意点としてはUbuntu 用のセットアップではなく Debian 用のセットアップ方法にするというくらいでしょうか。  

Docker をインストールしたら自分を docker グループに追加して sudo なしで docker を使えるようにしましょう。  

```bash
sudo usermod -aG docker `whoami`
```

ついでに docker-compose もインストールしておきました。  
こちらも[公式ページ](https://docs.docker.com/compose/install/ "docker_compose") に従うと普通にできました。  
