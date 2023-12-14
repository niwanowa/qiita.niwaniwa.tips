---
title: Github ActionsでIssueの追加をREADMEに反映させる
tags:
  - GitHubActions
private: false
updated_at: '2023-12-14T07:00:42+09:00'
id: d994ca63c63c7697ce95
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
[コピーレフト](https://www.gnu.org/licenses/copyleft.ja.html)寄りの[にわのわ](https://twitter.com/niwa_nowa)です。

ToDo管理にGithubのIssueを使っているのですが、READMEでIssueの一覧で表示できると嬉しいと思い作りました。

# 作ったもの
今回作ったものはこちらです。
詳しい使い方はREADMEに書いてあるのでそちらを参照してください。

https://github.com/niwanowa/ReadmeIssueSyncAction

# 内容
Issueの追加、編集、削除をトリガーにREADMEを更新するGithub Actionsです。
Github REST APIを使ってIssueの一覧を取得し、GHA上にクローンされたREADMEを更新します。
その後、予めSeacretsに設定したGithub情報を使ってコミット、プッシュを行います。

# つまづいたところ
GHA上でコミット、プッシュを行うためには、workflow permissions設定を変更する必要がありました。
[act](https://github.com/nektos/act)というGHAをローカルで実行できるツールを使ってテストしていたのですが、
これでは拾えないエラーだったのでかなり苦しみました。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/2ed021c7-7cb8-fe94-e96d-9a4ee5fe5154.png)

# おわりに
workflow permissionsなどのGithub Repositoryの設定は.githubフォルダの中で管理できないのでしょうか？
軽く調べてみても引っかからず、断念してしまいました...

# 参考

https://docs.github.com/ja/rest

https://github.com/nektos/act
