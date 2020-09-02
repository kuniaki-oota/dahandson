# コピー＆ペースト用コードスニペット

## P.30 Web Server を自動実行する
`Dockerfile` ファイルの内容

```Dockerfile
FROM alpine:3.12
LABEL maintainer="dreamarts.co.jp"

RUN apk add --update nodejs npm yarn && \
    mkdir -p /dadandson/http

COPY server.js /dadandson/http/server.js

ENTRYPOINT ["/usr/bin/node","/dadandson/http/server.js"]
```

## P.32 Dockerfile のビルド

```bash
docker build -t alpine-nodejs .
```

## P.34 ビルドしたイメージの実行
さくっとの共有した環境で使用している場合は `8080` のポート番号をそれぞれ変更してポート番号が重ならいようにしてください。

```bash
docker run -it -d --name http_server -p 8080:80 alpine-nodejs:latest
```

## P.36 デタッチドモードで起動したコンテナの停止

```bash
docker stop  http_server
```

## P.37 デタッチドモードで起動したコンテナの削除

```bash
docker rm http_server
```

## P.42 Docker イメージを改修します
`Dockerfile` ファイルの内容

```Dockerfile
FROM alpine:3.12
LABEL maintainer="dreamarts.co.jp"

RUN apk add --update nodejs npm yarn && \
    mkdir -p /dadandson/http && \
    mkdir -p /var/www/html

COPY server.js /dadandson/http/server.js

COPY index.html /var/www/html/index.html

ENTRYPOINT ["/usr/bin/node","/dadandson/http/server.js"]
```

## P.43 Docker イメージを改修します
`server.js` ファイルの内容

```javascript
const http = require('http');
const html = require('fs').readFileSync('/var/www/html/index.html');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
  res.end(html);
});

const port = 80;
server.listen(port);
console.log('Server listen on port ' + port);
```

## P.50 index.html を配置する
`index.html` ファイルの内容

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8">
    <title>Welcome to the world of Docker!</title>
  </head>
  <body>
    <h1> Dockerの世界へようこそ！！ </h1>
  </body>
</html>
```

## P.51 ボリュームをマウントして起動する

```bash
docker run -it -d --name http_server -p 8080:80 -v /tmp/html_home:/var/www/html alpine-nodejs:latest
```

## P.53 正しくマウントされているか確認する
起動中のコンテナの情報を表示する

```bash
docker inspect http_server
```

起動中のコンテナのボリュームマウント情報のみを表示する

```bash
docker inspect --format='Source:{{range .Mounts}}{{.Source}}{{end}}, Destination:{{range .Mounts}}{{.Destination}}{{end}}' http_server
```
