# 第３回　Webエンジニアリング演習　レポート
## 学籍番号
4724212

##動作確認
@4724212 ➜ /workspaces/web-eng-docker (class-03-docker) $ curl http://localhost:8080
<!doctype html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Web Engineering Class 03</title>
  </head>
  <body>
    <h1>Dockerで配信中</h1>
    <p>Web Engineering Class 03</p>
  </body>
</html>

@4724212 ➜ /workspaces/web-eng-docker (class-03-docker) $ docker compose ps
NAME                   IMAGE                COMMAND                  SERVICE   CREATED         STATUS         PORTS
web-eng-docker-web-1   web-eng-docker-web   "/docker-entrypoint.…"   web       4 seconds ago   Up 3 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
