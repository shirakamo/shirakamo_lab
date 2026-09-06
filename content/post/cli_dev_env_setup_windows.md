---
title: "Windows(WSL) CLI開発環境のセットアップ"
date: 2026-08-20T23:05:06+09:00
draft: false
description: "Windows(WSL)版のCLI開発環境セットアップ作業手順"
thumbnail:
  src: "https://res.cloudinary.com/dda9f1d6p/image/upload/v1787296849/shirakamo_lab_tech_blog/cli_dev_setup_windows.webp"
categories:
- "エンジニアリング"
tags:
- "Terminal"
- "Shell"
- "Neovim"
---

CLI開発環境としてAlacritty + Zsh + oh-my-zsh + Neovim の環境をセットアップした際の作業手順です。  


## なぜ今CLI開発環境か
開発環境は基本VSCodeを使っていたのですが、色々なエクステンションを入れた結果動作が重いと感じるようになりました。  
また、最近外出時のサブPCとしてChromebookを購入したのですが、Chromebook内でVSCodeを使うのはちょいと面倒です。  
(code-serverを使えばいけるのですが、これを機に新しい環境に切り替えてみようとも思い。)  

そこでZsh + NeovimのCLI環境に手を出してみようと思い立ちました。  

あまり意識はしていなかったのですが、最近話題のコーディングエージェント (Claud Codeとか) を使うにあたっても、
CLIに閉じつつリッチな環境を使えるのはメリットがあるのではないかと思っています。  
まだこの環境でのコーディングエージェントの使い勝手は分からないので、後々使用感などを別記事にしたいです。  

ということで早速やったことをまとめていきたいと思います。  
自分の手持ちの環境はWindows (WSL2 Ubuntu) とChromebook (Chrostini) なので、その両方で環境構築していきます。  
本記事はWindows WSL編です。次記事でChromebookでの設定方法をまとめます。


## WSL2での環境構築
基本は各アプリ公式ページで解説されているインストール方法に従うだけでできました。


### Alacritty
軽量&爆速ターミナルエミュレータです。  
必要な機能をかなり絞り込んで実装されている分、軽量でサクサク動きます。  
どれくらい機能が絞り込まれているかというと、例えばタブや画面分割ができません。  
こういうのはtmuxなどの別アプリでやってね、ということです。  

自分はターミナルエミュレータには多くの機能を求めておらず、
とにかくサクサク動いてほしいと思っている人間なので、Alacrittyはドンピシャでした。  

インストール方法は以下で説明されています。  

[Alacritty](https://alacritty.org/)

Windowsの場合はインストーラーをダウンロードして実行するだけです。  
起動するとコマンドプロンプトが立ち上がるので、デフォルトのシェルをWSLにするよう設定変更しました。  
設定はtomlで書きます。  
設定ファイルはWindowsの場合、 `%APPDATA%\\alacritty\\alacritty.toml` です。  
（なければ新規作成）    
以下の設定により、デフォルトで立ち上がるシェルをWSLに変更することが出来ました。  

```toml
[terminal.shell]
program = "wsl.exe"
args = ["~"]
```

`args` を指定することで初めからホームディレクトリに移動するようにしました。  
そうしないと.bashrcなどの設定ファイルが読み込まれません。

カラーテーマも変更しようと思ったのですが、
Alacrittyではカラーテーマのように一括で色変更する設定は無いようです。  
そこで以下のリポジトリからAlacritty向けのカラーテーマをダウンロードして設定しました。

[Alacritty Theme](https://github.com/alacritty/alacritty-theme)

インストール方法は上記のリンクに書いてある通りで、WSL内で

```zsh
mkdir -p ~/.config/alacritty/themes
git clone https://github.com/alacritty/alacritty-theme ~/.config/alacritty/themes
```

としてまずはリポジトリをダウンロード。  
その後、以下の設定をWindows側の`%APPDATA%\\alacritty\\alacritty.toml`に記載。  
この時、Alacrityが参照しているのはWSLではなくWindows側のパスなので、
Windows側から見たダウンロード先のパスを設定するようにしました。

```alacritty.toml
[general]
import = [
    "\\\\wsl$\\Ubuntu\\home\\hmoro\\.config\\alacritty\\themes\\themes\\nord.toml"
]
```

`nord` を設定しているのは私の好みです。リポジトリに様々なカラーテーマが載っているので、
お好きなものを設定してください。

最後に、設定ファイルがwindows側にあるのは編集したいときに面倒なので、
WSL側にシンボリックリンクを貼っておきました。

```bash
ln -s /mnt/c/Users/hashr/AppData/Roaming/alacritty/alacritty.toml ~/.config/alacritty/alacritty.toml
```


### Zsh
補完をはじめ、とにかく便利と噂のシェルです。  
インストール方法は以下で説明されています。  

[Installing ZSH](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)

とはいっても `sudo apt install zsh` するだけで特に問題なく使えました。


### oh-my-zsh
Zshのカスタマイズや管理のための拡張機能です。  
これがあるとプラグイン等も簡単に導入でき、Zshがさらに便利になるとのこと。  
インストール方法は以下で説明されています。  

[oh my zsh](https://ohmyz.sh/)

こちらも下記のインストールコマンドをコピペ実行するだけで特に問題なく使えるようになりました。  

```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

さらに、oh-my-zsh経由で補完とハイライトをより良い感じにするプラグインも入れてみました。

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/tree/master)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

この二つのインストール方法は上記のgithubリポジトリにも書いてあります。  
まず、githubからリポジトリをクローン。

```zsh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

次に、`.zshrc`の`plugins`の部分を以下のように書き換え。

```.zshrc
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
)
```

gitは多分最初から書いてあると思います。  
これがoh-my-zsh流のプラグイン管理方法なのでしょうか。こちらも今後勉強ですね。  

とりあえず、今の段階ではzshへのプラグイン導入はこのくらいにしておきます。


### Neovim
Neovimについての私の認識は「Vimをモダンに実装し直したエディタ」というものです。  
基本機能はVimと同等ですが、lua言語による設定、LSPへの標準対応など、
Vimがより使いやすくなっているものと考えています。  

インストール方法は以下。  

[neovim Intall](https://neovim.io/doc/install/)

私は今回楽できそうなAppImageを用いたインストールを行いました。  

```zsh
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.appimage
chmod u+x nvim-linux-x86_64.appimage
./nvim-linux-x86_64.appimage
```

これでNeovimがちゃんと実行されたので問題なさそうです。  
適当な所にAppImageを保存してPATHを通しておきました。  

```zsh
mkdir -p $HOME/opt/nvim
mv nvim-linux-x86_64.appimage $HOME/opt/nvim/nvim
```

`.zshrc`に以下を追記。

```.zshrc
# nvim
export PATH="$PATH:$HOME/opt/nvim/"
```

これで`nvim`コマンドでNeovimを実行できるようになりました。


### lazy.nvim
lazy.nvimはNeovimのプラグインマネージャーです。  
インストール方法は以下に説明があります。  

[lazy.nvim Installation](https://lazy.folke.io/installation)

まずNeovim起動時に読み込まれる設定ファイル`~/.config/nvim/init.lua`に以下を記載。  
該当するディレクトリ、ファイルが無い場合は新規作成します。  

```lua
require("config.lazy")
```

次に、lazy.nvim用の設定を `~/.confing/nvim/lua/config/lazy.lua` に記載。  
この場合も該当するディレクトリ、ファイルが無い場合は新規作成します。  

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

最後に、プラグイン追加設定を行うためのディレクトリ `~/.config/nvim/lua/plugins/` を作成。  
このディレクトリ内に追加するプラグインの設定などを書いていきます。  

このままプラグインの追加設定なしでも良いのですが、
lazy.nvimの動作確認もかねてシンプルなプラグインを追加してみました。  

追加するプラグインは [lualine.nvim](https://github.com/nvim-lualine/lualine.nvim) 。  
このプラグインはNeovim下部に表示されるのステータスバーを良い感じにします。  

`~/.config/nvim/lua/plugins/lualine.lua` を作成し、以下の内容を記述。  
以下の内容は[lualine.nvim](https://github.com/nvim-lualine/lualine.nvim)に記載されています。

```lua
return {
  'nvim-lualine/lualine.nvim',
  dependencies = { 'nvim-tree/nvim-web-devicons' },
  opts = {},
}
```

その後、Neovimを再起動するとインストール画面が表示され、lualineがインストールされました。  
ちなみにlazy.nvimのインストール画面は `:q` で閉じる必要があります。

このように、 `~/.confing/nvim/lua/plugins/` 以下に必要なプラグインとその設定を書いていくことで
Neovimの機能を拡張できるようです。

これで一旦Windows (WSL)での環境構築が一段落しました。
NeovimにおけるPython開発環境のセットアップなど、まだやりたいことはありますが、それは別記事にて。  
次の記事ではChromebookでのセットアップ手順をまとめていきます。

