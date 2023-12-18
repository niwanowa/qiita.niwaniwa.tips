---
title: myApplications使ってみた
tags:
  - 'AWS'
  - 'myApplications'
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
[にわのわ](https://twitter.com/niwa_nowa)です。
AWS Re:Invent 2023で発表されたmyApplicationsを使ってみました。

# myApplicationsとは
こちらがリリースノートです。

https://aws.amazon.com/jp/about-aws/whats-new/2023/11/myapplications-view-manage-applications-aws/

# 使ってみた
2023/12/18現在、マネコンを開くと以下のような画面が表示されます。
一番上の通知か、右上のウィジェットのアプリーケーションを作成から設定を行います。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/04dcc23b-dfa6-997a-2844-d5ca8c509efc.png)

今回は、[こちら](https://qiita.com/niwanowa/items/d8136c5f476f8596288d)で作成したもので設定していこうと思います。
構成としては以下のようになっています。
![serverlessland.com_patterns_apigw-lambda-dynamodb-terraform.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/bdfcd6fd-7883-8fb8-5204-2bf7a71eedca.png)

Application nameのみ入力して、Nextを押します。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/a51bd6b6-de18-02ce-7b77-9f137dbbcdf1.png)

Resource Explorerを有効化していないと、セットアップするよう求められるので有効化します。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/e3c7f2b7-78b2-c046-8112-955fcae36dc1.png)

作成したリソースを選択して、Nextを押します。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/40d63833-fc44-3027-e5c3-188e57b2eec2.png)

最後にCreate Applicationを押すと、アプリケーションが作成されます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/be0027bc-ceed-5bfe-fe07-5c8ea1a9e6fe.png)

こんな画面になれば一旦完了です
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/701f97f3-a3f3-444f-c933-92cec9edfad6.png)

# 確認
マネコンに戻るとウィジェットにアプリケーションが追加されています。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/3a549a36-47ea-10f7-fc53-9a979a2bcd6c.png)
その他、myApplicationsに移動するとコストなどが確認できます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/693b74c4-dbf4-e9b1-6449-9069cfa6de13.png)

# おわりに
アプリケーション、サービスごとに管理しやすくなっていいですね！
どしどし利用していこうと思います。
