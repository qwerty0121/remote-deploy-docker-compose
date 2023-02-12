# remote-deploy-docker-compose

## 概要

Docker ContextとDocker Composeを利用してリモートサーバーにdockerコンテナを起動するサンプル。

## 手順

### リモートサーバー用のcontextを登録

```bash
# contextを登録
# NOTE: ${username}/${remote-host}はそれぞれリモートサーバーのユーザーとホスト名(またはIPアドレス)に置き換える
docker context create remote --docker "host=ssh://${username}@${remote-host}" --default-stack-orchestrator swarm

# リモートでdockerコマンドを実行できることを確認する
# ※リモートで稼働しているdockerコンテナの一覧が出力されていればOK
# NOTE: ---
# 以下の前提条件を満たしている必要がある。
# * リモートサーバーにssh接続できる
# * ユーザーがsudoなしでdockerコマンドを実行できる
# ---------
docker --context remote ps
```

### リモートサーバー上でコンテナを起動する

```bash
# `docker-compose up -d`をリモートサーバー上で実行
docker-compose --context remote up -d

# リモートサーバー上でコンテナが起動していることを確認する
# ※該当のコンテナが表示されていればOK
docker --context remote ps
```

### リモートサーバー上のコンテナを停止する

```bash
# `docker-compose down`をリモートサーバー上で実行
docker-compose --context remote down

# リモートサーバー上でコンテナが起動していないことを確認する
# ※該当のコンテナが表示されていなければOK
docker --context remote ps
```

## 参考サイト

- [ローカルにあるdocker-composeプロジェクトをリモートのDockerで実行する | Qiita](https://qiita.com/legacyworld/items/0fb8507a8951e13f8061)
- [How to deploy on remote Docker hosts with docker-compose](https://www.docker.com/blog/how-to-deploy-on-remote-docker-hosts-with-docker-compose/)
- [Docker Contextsを使ってDocker Composeをデプロイする際の注意点](https://blog.p1ass.com/posts/docker-context/)
- [Docker コンテキストを切り替えてリモートホスト上で Docker コマンドを実行する](https://maku77.github.io/p/qatbs9p/)
