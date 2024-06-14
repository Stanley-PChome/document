```
查詢版本
docker version

登入docker hub
docker login

登出docker hub
docker logout

列出本機images
docker images

取得images
docker pull <images-name>

推至docker hub
docker push <images-name>

查看運行中的 container
docker ps

-a  #所有container

#建立容器
docker create <images-name>

#建立並啟動容器
docker run -it <images-name>

-it  #進入container shell下指令
-d  #container後台運作
-p <port>:<container-port>  #container內部使用的port映射至主機
-v  #掛載 volume 至容器中
-e  HOST=127.0.0.1  #設定環境變數
--name  #別名
--network <network-name>  #指定網路設定

#啟動容器
docker start <container-id>

-a  #前景觀看輸出

停止容器
docker stop <container-id>

重啟容器
docker restart <container-id>

進入容器
docker attach <container-id>  #退出後會停止容器
docker exec -it <container-id> /bin/bash  #退出後不會停止容器

刪除容器
docker rm <container-id>
-f  #強制刪除

刪除映像檔
docker rmi <image-name>

查看容器內的資訊
docker logs <container-id>

-f  #持續更新輸出

建立虛擬網路
docker network create <network-name>

顯示所有network
docker network ls

容器加入虛擬網路
docker network connect <network-name> <container-name or container-id>

查看env並刪除
docker run --rm <image-name> env

查詢映像檔
docker search ubuntu
-f is-official=true #過濾指返回官方映像檔

```

Dockerfile 
```
建立 Dockerfile
docker build -t <tag-name> <path>

FROM  #docker 映像檔名稱

MAINTAINER  #說明撰寫和維護人資訊

RUN  #執行指令

ARG  #設定參數(Dockerfile中使用)

ENV  #設定環境變數(容器中使用)

VOLUME  #持久化資料

範例
FROM ubuntu:20.04

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    apache2 \
    php7.4 \
    libapache2-mod-php7.4 \
    && rm -rf /var/lib/apt/lists/*

RUN sed -i 's/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf

COPY index.php /var/www/html/index.php

EXPOSE 80

CMD ["apachectl", "-D", "FOREGROUND"]
```

Compose
```
docker-compose up -d

docker-compose ps

yml格式
services
  <container-name>
    image: <image-name>  #鏡像名稱
    environment: <var>  #環境變數
    networks: - <network-name>  #虛擬網路名稱
    ports: - <local-port>:<container-port>
    depends_on: - <container-name>  #依賴的容器
    volumes: <path>  #共用的目錄位置
    working_dir: <paht>  #切至工作的目錄
networks
  <network-name>
    driver: <type>  #bridge:容器可以連接到這個網絡並與主機以及其他容器通信

yml範例
version: "3.8"

services:
  mysql:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: 123
      MYSQL_ALLOW_EMPTY_PASSWORD: 1
    networks:
      - my-network

  phpmyadmin:
    image: phpmyadmin
    ports:
      - 8080:80
    environment:
      PMA_HOST: mysql
    depends_on:
      - mysql
    networks:
      - my-network
    volumes:
      - ./php-app:/var/www/html

volumes:
  php-app:

networks:
  my-network:
    driver: bridge

```
