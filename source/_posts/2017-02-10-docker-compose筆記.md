title: docker compose筆記
categories: computer-science
tags: [docker]
date: 2017-02-10 16:37:57
---

Container之間有關係時可以用docker command中的--link搞定
當有多個Container需要連結時我們會希望能撰寫一份設定檔定義Container之間的關係然後執行就好
這就是docker-compose在做的事
撰寫docker-compose.yml定義各Container link relation
然後run docker-compose up -d

## Command
``` sh
docker-compose -f ./docker-compose.yml up -d
```
## docker-compose.yml example
以下設定表示
faceblock是ap server
zombodb_postgres是postgres database
zombodb_elastic是elasticsearch database
faceblock相依於zombodb_postgres
zombodb_postgres相依於zombodb_elastic
``` sh
faceblock:
  build: ./faceblock # DockerFile location
  ports: # export ports
   - "3001:3000"
   - "3043:443"
  links: # dependency
   - zombodb_postgres
  entrypoint: /execute.sh

zombodb_postgres:
  build: ./postgres
  ports:
   - "5432:5432"
  links:
   - zombodb_elastic
  restart: always # auto restart while exit

zombodb_elastic:
  build: ./elasticsearch
  ports:
   - "9200:9200"
   - "9300:9300"
  restart: always
```
