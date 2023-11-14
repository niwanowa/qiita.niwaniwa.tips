---
title: MacBookPro環境構築メモ
tags:
  - ''
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: true
---
# はじめに
[にわのわ](https://twitter.com/niwa_nowa)です。

# 設定
- スクショの設定
やらないとデスクトップが地獄となる。

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

## git

https://git-scm.com/download/mac

## Homebrew
https://brew.sh

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/{ユーザ名}/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

## HHKB用ドライバ

https://happyhackingkb.com/jp/download/macdownload.html

## iTerm2

https://iterm2.com/

- iTerm2 -> Settings -> General -> windows　-> Native full screen windows のチェックを外す
- iTerm2 -> Settings -> Profiles
  - window -> Transparency : 30 
  - window -> style : Fullscreen
  - window -> screen : screen with cursor
  - Session -> Prompt before closing? : If there are jobs besides 
  - Keys -> A hotkey opens a dedicated window with this profile. : option + space


## npmのインストール
```bash
brew install n
sudo n latest
sudo chown -R 501:20 "/Users/{ユーザ名}/.npm"
```

## Qiita CLIのインストール
[こちら](https://github.com/increments/qiita-cli)参照
```bash
npm install @qiita/qiita-cli --save-dev
```

# その他
- スキンシール
wraplusのスキンシールがいい感じ。
https://wraplus.jp/products/macbook-pro14_2021-orange-none-cut
-   