---
title: "Chromebook (Crostini) CLI開発環境のセットアップ"
date: 2026-08-21T17:20:58+09:00
draft: false
description: "Chromebook (Crostini)版のCLI開発環境セットアップ手順備忘録"
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1787321043/shirakamo_lab_tech_blog/cli_dev_env_setup_chromebook_z94fcr.webp"
categories:
- "エンジニアリング"
tags:
- "Terminal"
- "Shell"
- "Neovim"
- "Chromebook"
---

[前回の記事]({{% ref "post/cli_dev_env_setup_windows.md" %}})に引き続き、今度はChromebookに
CLI開発環境としてAlacritty + Zsh + oh-my-zsh + Neovim の環境をセットアップする手順をまとめました。

なぜそのようなことをしているのかは[前回の記事]({{% ref "post/cli_dev_env_setup_windows.md" %}})に書いたので、
本記事は本題の環境構築手順をメインに書いていきます。

## 今回用いたChromebookについて
私が持っているChromebookはこちらです。  
作業用サブPC兼読書デバイスとして購入しました。

<!-- START MoshimoAffiliateEasyLink -->
<script type="text/javascript">
(function(b,c,f,g,a,d,e){b.MoshimoAffiliateObject=a;
b[a]=b[a]||function(){arguments.currentScript=c.currentScript
||c.scripts[c.scripts.length-2];(b[a].q=b[a].q||[]).push(arguments)};

c.getElementById(a)||(d=c.createElement(f),d.src=g,
d.id=a,e=c.getElementsByTagName("body")[0],e.appendChild(d))})
(window,document,"script","//dn.msmstatic.com/site/cardlink/bundle.js?20220329","msmaflink");
msmaflink({"n":"【Amazon.co.jp限定】Lenovo Chromebook クロームブック Duet 11 10.95インチ MediaTek Kompanio 838 プロセッサー​搭載 メモリ8GB eMMC 128GB 重量1.0kg 83HH000TJP パソコン","b":"Lenovo","t":"83HH000TJP","d":"https:\/\/m.media-amazon.com","c_p":"\/images\/I","p":["\/41fh5OS1f9L._SL500_.jpg","\/51h-uJ+++dL._SL500_.jpg","\/51IwUQka6qL._SL500_.jpg","\/41sPm6oacYL._SL500_.jpg","\/51hKP6BT2ML._SL500_.jpg","\/41A+W0L-HIL._SL500_.jpg","\/51w-XFouelL._SL500_.jpg","\/41xSIyHWcJL._SL500_.jpg","\/41w3fFX4f7L._SL500_.jpg","\/41dn2kDZMfL._SL500_.jpg","\/41K3vEFrHUL._SL500_.jpg","\/210yX1tDlDL._SL500_.jpg","\/31-ghRzjtlL._SL500_.jpg","\/31x4mtIQZEL._SL500_.jpg","\/31C4cbsUcVL._SL500_.jpg","\/41ZH9NajfgL._SL500_.jpg","\/31VB4EKggiL._SL500_.jpg","\/21knUwAfOXL._SL500_.jpg"],"u":{"u":"https:\/\/www.amazon.co.jp\/dp\/B0DGXB37K5","t":"amazon","r_v":""},"v":"2.1","b_l":[{"id":1,"u_tx":"Amazonで見る","u_bc":"#f79256","u_url":"https:\/\/www.amazon.co.jp\/dp\/B0DGXB37K5","a_id":3885217,"p_id":170,"pl_id":27060,"pc_id":185,"s_n":"amazon","u_so":0}],"eid":"ytXH4","s":"s"});
</script>
<div id="msmaflink-ytXH4">リンク</div>
<!-- MoshimoAffiliateEasyLink END -->

このChromebookはキーボード着脱式のため、
キーボードを外せば手軽に持ち出せるタブレット端末として扱えます。  
この形態は読書にピッタリで、電車の中やカフェなどで本を読むのに便利です。  
AmazonのKindleも楽天Koboもアプリとして利用できるのが個人的にはポイント。

加えて、本記事で解説するように本格的なLinux環境も簡単にセットアップでき、
プログラミングもこなせます。  
Chromebookはサブ機としてとても優秀なマシンだと思います。

ということで、以降ではChromebook開発環境のセットアップについて解説していきます。  

## Crostiniの有効化と依存ライブラリのインストール
ChromebookでLinux環境を使う設定は、単に設定から有効化のボタンを押すだけです。  
詳細な方法はChromebookのヘルプページを参照してください。  

[ChromebookでLinuxをセットアップする](https://support.google.com/chromebook/answer/9145439?hl=ja)

ちなみにChromebook上のLinux環境はCrostiniとも呼ばれるので、本記事ではCrostiniの名称を使っていきます。

次にgitを使えるようにしておきます。  
今回の作業中、gitを多用するので必須です。
ついでにリポジトリのアップデート、ライブラリのアップグレードもしておきました。

```bash
sudo apt update
sudo apt upgrade
sudo apt install git
```


## Alacrittyのインストールとセットアップ
AlacrittyのLinux系へのインストール方法は以下で説明されています。

[alacritty / INSALL.md](https://github.com/alacritty/alacritty/blob/master/INSTALL.md)

上記リンク先に記載のコマンドをCrostiniで実行してAlacrittyをインストールします。

前回のWindowsと異なり、Linux版はRustのパッケージマネージャであるCargoを通してインストールします。  
そのため、単にインストールするだけではコマンドラインから起動するしかなく、
デスクトップから起動できなくなってしまいます。  
そこで、上記リンク先の `Manual Installation` の項目を順に実施し、
最終的にChromebookのデスクトップ（というかLauncher）から起動できるようにします。

まずはAlacrittyのリポジトリをクローンします。  

```bash
git clone https://github.com/alacritty/alacritty.git
cd alacritty
```

次にRustとCargoをインストールします。  
インストール方法は下記に記載されています。

[The Rust Programming Language日本語版 インストール](https://doc.rust-jp.rs/book-ja/ch01-01-installation.html)

記載の通り、以下のコマンドを実行します。

```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

多少時間がかかります。  
`Rust is installed now, Great!` という表示が出たらOKです。  
一度Crostiniを閉じて再起動し、Cargoが使えることを確認します。

```bash
cargo --version
cargo 1.98.0 ... # <-バージョン番号が表示されればOK
```

以下2つのコマンドを実行します。

```
rustup override set stable
rustup update stable
```

次にAlacrittyをビルドするための依存ライブラリをCrostiniにインストールします。  
CrostiniはDebianベースなので、

```bash
apt install cmake g++ pkg-config libfontconfig1-dev libxcb-xfixes0-dev libxkbcommon-dev python3
```

を実行します。インストールが完了したらAlacrittyのビルドです。

```bash
cargo build --release
```

こちらもしばらく時間がかかります。  
ビルドが終わったらAlacrittyをデスクトップから使えるようにするための設定です。  
ここで少しマニュアルの記載事項と違うことをします。

カレントディレクトリがクローンしたリポジトリの中であることを確認してください。

```bash
cd <alacrittyリポジトリへのパス>
```

リポジトリの中の `extra/linux/Alacritty.desktop` ファイルを以下のように書き換えます。  
元ファイルとの差分は、以下の `Exec` の部分で、 `env WAYLAND_DISPLAY= ` を追記しています。

```Alacritty.desktop
[Desktop Entry]
Type=Application
TryExec=alacritty
Exec=env WAYLAND_DISPLAY= alacritty
Icon=Alacritty
Terminal=false
Categories=System;TerminalEmulator;

Name=Alacritty
GenericName=Terminal
Comment=A fast, cross-platform, OpenGL terminal emulator
StartupNotify=true
StartupWMClass=Alacritty
Actions=New;

[Desktop Action New]
Name=New Terminal
Exec=alacritty
```

こうしないと自分の環境ではAlacritty画面右上の画面最小化、最大化、閉じるボタンが使えませんでした。  
以降はマニュアル通りにコマンドを実行していきます。

```bash
sudo cp target/release/alacritty /usr/local/bin
sudo cp extra/logo/alacritty-term.svg /usr/share/pixmaps/Alacritty.svg
sudo desktop-file-install extra/linux/Alacritty.desktop
sudo update-desktop-database
```

これでChromebookのLauncherからAlacrityを実行することが出来るようになりました。


## zshとoh-my-zshのインストール
zshはaptでインストールしました。  
ただ、 `.zshrc` などの設定ファイルが無い場合、zshを起動するとエラーが出てしまいます。  
zshをインストールする直前に`.zshrc`を空ファイルとして作っておいた方が良さそうです。  
（どうせ後でoh-my-zshに内容書き換えられます。）

```bash
touch ~/.zshrc
suto apt install zsh
```

zshをデフォルトシェルにします。

```bash
sudo chsh -s /usr/bin/zsh ${USER}
```

ターミナルを閉じて再起動するとzshが起動しているはずです。  
次にoh-my-zshをインストールします。  
こちらは以下に記載してあるコマンドのコピペで完了しました。

[oh-my-zsh](https://ohmyz.sh/)

```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

ここまででzshのセットアップは基本完了ですが、
Windowsと環境を同じにするために以下2つの拡張機能をoh-my-zshを通してインストールします。

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

インストール方法は以下です。

```zsh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
```

`~/.zshrc`の`plugin`セクションを以下のように書き換えます。

```.zshrc
plugins = (
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
)
```

これでzshの環境構築も一段落です。


## Neovimとlazy.nvimのインストール
Neovimとlazy.nvimのインストール方法はそれぞれ以下で説明されています。

- [neovim Install](https://neovim.io/doc/install/)
- [lazy.nvim Installation](https://lazy.folke.io/installation)

まずはNeovimからです。Windowsの時と同様、AppImageを使ってインストールします。  
私の環境はARMのCPUなので、arm64版とすることに注意します。

```zsh
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-arm64.appimage
chmod u+x nvim-linux-arm64.appimage

# 動作確認
./nvim-linux-arm64.appimage
```

上記でNeovimが起動すればOKです。  
適当な場所にAppImageを移動してパスを通しておきます。

```zsh
mkdir ~/opt/nvim
mv nvim-linux-arm64.appimage  ~/opt/nvim/nvim
```

`~/.zshrc`に以下を追記します。

```.zshrc
export PATH="$PATH:$HOME/opt/nvim/"
```

これでNeovimのインストールは完了です。  
続いてlazy.nvimをインストールします。
内容は前回のWindowsの時と全く同じなので、作成するファイル名とその内容のみ記載します。

`~/.config/nvim/lua/config/lazy.lua`

```lua
require("config.lazy")
```

`~/.confing/nvim/lua/config/lazy.lua`

```lua
-- Bootstrap lazy.nvim
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not (vim.uv or vim.loop).fs_stat(lazypath) then
  local lazyrepo = "https://github.com/folke/lazy.nvim.git"
  local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
  if vim.v.shell_error ~= 0 then

    vim.api.nvim_echo({
      { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
      { out, "WarningMsg" },
      { "\nPress any key to exit..." },
    }, true, {})
    vim.fn.getchar()
    os.exit(1)
  end
end
vim.opt.rtp:prepend(lazypath)


-- Make sure to setup `mapleader` and `maplocalleader` before
-- loading lazy.nvim so that mappings are correct.
-- This is also a good place to setup other settings (vim.opt)
vim.g.mapleader = " "
vim.g.maplocalleader = "\\"

-- Setup lazy.nvim
require("lazy").setup({
  spec = {
    -- import your plugins
    { import = "plugins" },
  },
  -- Configure any other settings here. See the documentation for more details.
  -- colorscheme that will be used when installing plugins.
  install = { colorscheme = { "habamax" } },
  -- automatically check for plugin updates
  checker = { enabled = true },
})
```

[lualine.nvim](https://github.com/nvim-lualine/lualine.nvim)の追加。  
`~/.config/nvim/lua/plugins/lualine.lua` 

```lua
return {
  'nvim-lualine/lualine.nvim',
  dependencies = { 'nvim-tree/nvim-web-devicons' },
  opts = {},
}
```

これでChromebookにもCLI開発環境が整いました。  
これからはNeovimでPython開発環境を構築していくことになりますが、
手順はWindows (WSL)もChromebook (Crostini)もほぼ同じなのでまとめて次回の記事にします。

