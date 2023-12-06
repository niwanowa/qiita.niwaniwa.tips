---
title: にわのわさんと振り返るGitHub_Universe_2023
tags:
  - GitHub
  - githubcopilot
private: true
updated_at: '2023-12-06T18:13:22+09:00'
id: 540c314dd016c3918e7f
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
[にわのわ](https://twitter.com/niwa_nowa)です。
2023/11/09に行われたGitHub Universe 2023の内容について振り返っていこうと思います。
1ヶ月経って段々記憶が薄れてきたころあいだと思うので一緒に復習しましょう。

2日前にOpenAI DevDayを振り返ったので、こちらもよろしければどうぞ。

https://qiita.com/niwanowa/private/c0744fa4e9ffeeb258ec

# 目線
- GitHub Copilot Individualに入ってる
- チーム、組織、仕事では使ったことない

# まとめ
公式ツイッターさんがまとめてくれてるのでそちらを引用。
<blockquote class="twitter-tweet"><p lang="ja" dir="ltr"><a href="https://twitter.com/hashtag/GitHubUniverse?src=hash&amp;ref_src=twsrc%5Etfw">#GitHubUniverse</a> の基調講演の発表まとめ📣<br>✅GitHub Copilot Chat一般提供開始<br>✅新たなGitHub Copilot Enterpriseのプレビュー<br>✅AI活用した新しいセキュリティ機能<br>✅GitHub Copilotパートナープログラム<br>✅その他色々...<br>詳しくはブログをご覧ください👇</p>&mdash; GitHub Japan (@GitHubJapan) <a href="https://twitter.com/GitHubJapan/status/1722419761328591358?ref_src=twsrc%5Etfw">November 9, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

https://docs.github.com/ja/copilot/github-copilot-in-the-cli

# GitHub Copilot Chat一般提供開始
12月から一般提供開始らしい。
既にGitHub Copilot Individualプランでも利用可能なので、
既に使ってる人はあまり関係ないかもしれない。

GPT-3モデルからGPT-4モデルに変わるなど、パワーアップされるらしい。
ちなみに記事執筆時点だとGPT-3モデルのまま。GAに合わせてアップデートされるのかしら。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/f9e515aa-e0f3-2708-9443-cd4632723b42.png)

# GitHub Copilot ChatをGitHub.comとモバイルアプリケーションに実装
らしい。
[こちら](https://github.blog/jp/2023-11-09-universe-2023-copilot-transforms-github-into-the-ai-powered-developer-platform/#github-copilot-chat%e3%82%92github-com%e3%81%a8%e3%83%a2%e3%83%90%e3%82%a4%e3%83%ab%e3%82%a2%e3%83%97%e3%83%aa%e3%82%b1%e3%83%bc%e3%82%b7%e3%83%a7%e3%83%b3%e3%81%ab%e5%ae%9f%e8%a3%85)ではウェイティングリストに参加するって項目があるけど見当たらない。

# GitHub Copilot Enterpriseの登場
Copilot Businessのさらに上位プランが登場。
色々抽象的にすごいぜってことが書いてあるが具体的な内容は不明。

# GitHub Copilotパートナープログラムの登場
サードパーティーのツールと連携することができるようになるらしい。

# GitHub Advanced Securityの登場
登場でいいよね？
Organizations向け機能。
プルリクのタイミングでセキュリティチェックを行って修正内容の提案まで行ってくれるらしい。

# GitHub Copilot Workspaceの発表
ビルド、実行、テストといったCI/CDラインの他、
Issueを作るだけで自動でコードを書いてくれるらしい。
まだ試せない。

ちなみに[公式の記事](https://github.blog/jp/2023-11-09-universe-2023-copilot-transforms-github-into-the-ai-powered-developer-platform/#%e6%ac%a1%e3%81%ab%e6%9d%a5%e3%82%8b%e3%82%82%e3%81%ae%ef%bc%9agithub-copilot-workspace)でtypoしてる。

```
あなたのリードに従って、IsueからPull Requestまで、AIの力でリポジトリ全体に変更を加えることができます。
```

# おわりに
GitHub Copilot Workspace、GitHub Universeの名に恥じぬ強烈な内容でしたね。
マジで速く試してみたいです。
アイデアだけ書いて自動でビルドまでしてくれると信じてる。

# 参考

Universe 2023: CopilotがGitHubをAIを駆使した開発者プラットフォームへと変貌させる - GitHubブログ
https://github.blog/jp/2023-11-09-universe-2023-copilot-transforms-github-into-the-ai-powered-developer-platform/

GitHub Universe 2023 での AI に関する発表まとめ！Copilot Chat の12月からの一般提供開始など | DevelopersIO
https://dev.classmethod.jp/articles/summary-of-github-copilot-announcements-at-github-universe-2023/
