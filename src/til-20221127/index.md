# AWS Hands-on for Begginers Serverless #1

[AWS Hands-on for Begginers Serverless](https://pages.awscloud.com/event_JAPAN_Hands-on-for-Beginners-Serverless-2019_LP.html)を実施した時のメモ。

## メモ

- 全てがGUIから操作するためコードなどが残らない。方法を調査して残す。
- [AWS SAMのdeleteコマンドを試す](https://www.d-make.co.jp/blog/2021/08/21/try-aws-sam-delete-command/): SAMを使ってdeploy, deleteまでを最小限で行っていそう。
- [AWS SAM CLI のインストール](https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
  - docker内にsamをインストール
  - docker環境はDooD環境にすれば対応できそう。
  - インストール先はどこになる??マルチビルド構成でファイルを異動した方がよさそう。aptでunzipとかのインストールが必要。
  - multi stage buildでsamをインストールしたdockerfileを作成
