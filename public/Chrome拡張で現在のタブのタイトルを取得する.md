---
title: Chrome拡張で現在のタブのタイトルを取得する
tags:
  - HTML
  - CSS
  - chrome-extension
private: true
updated_at: '2023-12-05T17:44:10+09:00'
id: 7cae00f9a0c84c0272ae
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
Chrome拡張HelloWorldの[にわのわ](https://twitter.com/niwa_nowa)です。
前日、前々日とRSSフィードとRSSリーダを作りました。
このRSSフィードにお好みの記事を追加するためのChrome拡張を作りたいと思います。

# 作ったもの
今回作ったものはこちらで公開しています。

https://github.com/niwanowa/niwanowa-rss-post-extension

# ソース
```background.js
/* コンテキストメニューを作成 */
const parent = chrome.contextMenus.create({
    id: "share",
    title: "ページを共有",
    contexts: ["all"],
});

chrome.contextMenus.create({
    parentId: parent,
    id: "title",
    title: "ページタイトルとURLを送信",
    contexts: ["all"],
});


/* コンテキストメニューがクリックされた時の処理 */
chrome.contextMenus.onClicked.addListener((info, tab) => {
    switch (info.menuItemId) {
        case "title":
            chrome.scripting.executeScript({
                target: { tabId: tab.id },
                function: title,
            });
            break;
    }
});
function title() {
    // タイトルとURLを取得
    const url = "https://hogehoge.com";
    const link = location.href;
    const title = document.title;

    // POSTリクエストを送信
    fetch(url, {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
        },
        body: JSON.stringify({ link, title }),
    })
    .then(response => response.json())
    .then(data => console.log("POST successful:", data))
    .catch(error => console.error("Error:", error));
}

```
# Feature
> const url = "https://hogehoge.com";
とURLをベタ書きしているのですがこれはオプションで設定できるようにする予定です。
（ソースにoption.htmlを書き散らしている。）

# おわりに
初めてChrome拡張でものを作りました。
jsではあるんですが拡張特有のものが多く、全くわからんとなっています。
全くわからん。

# 参考

https://qiita.com/FrogApp/items/cd64894721a0e4723047
