---
title: MacBookPro環境構築メモ
tags:
  - 'macOS'
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
2019年モデルのMacBookAirを買った1ヶ月後くらいにM1が発売されたショックをいまだに覚えています。[にわのわ](https://twitter.com/niwa_nowa)です。
M3になり、かなり熟れてきた頃合いと判断し、MacBookProを購入しました。
設定の備忘録を兼ねて公開します。niwanowaM3カスタムです。

# 設定
- スクショの設定
やらないとデスクトップが地獄となる。
```command + shift + 5```でスクリーンショットを起動
オプションから保存先をクリップボードに変更する。

- システム設定 > マウス
 - 軌道の速さをデフォルトよりも速くする
 早ければ早いほど良いとされています

 - ナチュラルなスクロール をオフに
 windows脳だと頭がバグる

 - 詳細設定 > ポインタを加速 をオフに
 これをしないとマウスがおっそい

# インストールしたもの

## VSCode

https://code.visualstudio.com/docs/?dv=osx

コマンドパレットから以下を実行することでパスが通る。
```
>shell command: Install 'code' command in PATH
```
## git

https://git-scm.com/download/mac

いつもの
ちなみに https://github.com/settings/emails から
```114514+niwanowa@users.noreply.github.com ```のような誰に見られても良いメールアドレスが確認できる。

  ```bash
  git config --global user.name "Your name"
  git config --global user.email "Your email"
  ```

## Homebrew

https://brew.sh

パス通すのを忘れずに
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/{ユーザ名}/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

## HHKB用ドライバ
入れないとcommandキーが使えない...
https://happyhackingkb.com/jp/download/macdownload.html

## iTerm2

https://iterm2.com/

### 設定
- iTerm2 -> Settings -> General -> windows　-> Native full screen windows のチェックを外す
- iTerm2 -> Settings -> Profiles
  - window -> Transparency : 30 
  - window -> style : Fullscreen
  - window -> screen : screen with cursor
  - Session -> Prompt before closing? : If there are jobs besides 
  - Keys -> A hotkey opens a dedicated window with this profile. : option + space


## npm
```bash
brew install n
sudo n latest
sudo chown -R 501:20 "/Users/{ユーザ名}/.npm"
```

## Qiita CLI
[こちら](https://github.com/increments/qiita-cli)参照
```bash
npm install @qiita/qiita-cli --save-dev
npx qiita login
```
## フォントの設定
こちらをダウンロード

https://www.jetbrains.com/ja-jp/lp/mono/

launchpadからフォントブックを開き、ダウンロードしたフォントをドラッグ&ドロップ
```JetBrains Mono NL```を基本的に使っていく。

## Docker

https://matsuand.github.io/docs.docker.jp.onthefly/desktop/mac/apple-silicon/

一応以下も実行しておく。
```bash
softwareupdate --install-rosetta
```

# その他
- スキンシール
wraplusのスキンシールがいい感じ。
https://wraplus.jp/products/macbook-pro14_2021-orange-none-cut

# おわりに
フォントとスキンシールで内と外を綺麗にしましょうね。