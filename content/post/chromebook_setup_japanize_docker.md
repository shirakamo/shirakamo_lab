---
title: "Chromebookをセットアップした手順"
date: 2023-02-11T22:26:31+09:00
description: "Chromebook PCセットアップの作業ログ"
draft: true
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
