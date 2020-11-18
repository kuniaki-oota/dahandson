# Docker Compose

## 概要
[Docker Compose公式ドキュメント](https://docs.docker.jp/compose/overview.html) から
```
Compose とは、複数のコンテナを定義し実行する Docker アプリケーションのためのツールです。Compose においては YAML ファイルを使ってアプリケーションサービスの設定を行います。コマンドを１つ実行するだけで、設定内容に基づいたアプリケーションサービスの生成、起動を行います。
```

## Compose を使うための基本的な３つのステップ
1. アプリケーション環境を `Dockerfile` に定義します。
2. アプリケーションを構成するサービスを `docker-compose.yml` ファイル内に定義します。
3. 最後に、`docker-compose up` を実行する。


## ハンズオン
### 準備
`docker-compose` がインストールされているか確認します。（Docker for Mac と Docker for Windows には予めインストールされています。linux の場合はインストールされていない可能性があります。）
```
$ docker-compose --version
```

#### Linux における Docker Compose のインストール
以下のコマンドを実行して Docker Compose 最新版をダウンロードします。
```
sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

### docker-compose でコンテナを起動する
docker run で起動していたコンテナを docker-compose で起動してみます。

#### docker run で起動（前回のハンズオンの内容）
```bash
docker run -it -d --name http_server -p 8080:80 -v /tmp/html_home:/var/www/html alpine-nodejs:latest
```

#### docker-compose で起動
##### `docker-compose.yml` ファイルの作成
ファイル名を`docker-compose.yml`としてファイルを作成します。

```yaml
version: '3'
services:
  http_server:
    image: alpine-nodejs:latest
    ports:
      - "8080:80"
    volumes:
      - /tmp/html_home:/var/www/html
```

##### Compose 起動
- フォアグラウンドで起動
```
$ docker-compose up
```

- バックグラウンド（デーモン）で起動
```
$ docker-compose up -d
```

## docker-compose.yml ファイルの内容説明
### Compose ファイルフォーマットのバージョンについて
[Docker Compose公式ドキュメント](https://matsuand.github.io/docs.docker.jp.onthefly/compose/compose-file/) から
```
Compose ファイルフォーマットには 1、2、2.x、3.x という複数のバージョンがあります。
Compose ファイルのどのバージョンが Docker リリースをサポートしているかを示すものです。
```

### Service について
アプリケーションを動かすための各要素を Service と読んでいます。
```yaml
version: '3'

services:
  wordpress:
    image: wordpress
    container_name: some-wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_PASSWORD: my-secret-pw

  mysql:
    image: mysql:5.7
    container_name: some-mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: my-secret-pw
```
