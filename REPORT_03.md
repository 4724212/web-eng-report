# 第3回　Webエンジニアリング演習　レポート
## 学籍番号
4724212

## イメージとコンテナの違い
イメージは読み取り専用の設計済みの型で、コンテナはそれを動かした実体のことを言う。同じイメージから複数のコンテナを作れ、コンテナを削除してもイメージは残る。

## Dockerfileの各行の役割
FROM：土台のイメージ（nginx:1.29-alpine）を指定する。
COPY：index.html をNginxの公開ディレクトリへ入れ、ページを置き換える。

## 動作確認
```
@4724212 ➜ /workspaces/web-eng-docker (class-03-docker) $ curl http://localhost:8080
<!doctype html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Web Engineering Class 03</title>
  </head>
  <body>
    <h1>Docker Composeで配信中</h1>
    <p>Web Engineering Class 03</p>
  </body>
</html>
@4724212 ➜ /workspaces/web-eng-docker (class-03-docker) $ docker compose ps
NAME                   IMAGE                                                                     COMMAND                  SERVICE   CREATED         STATUS         PORTS
web-eng-docker-web-1   sha256:837ef85228283724dd3f0a41ad023a478dcc5be79594fbec2beaf68811deb30e   "/docker-entrypoint.…"   web       8 minutes ago   Up 8 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
```
