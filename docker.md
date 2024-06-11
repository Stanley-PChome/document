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
--name  #別名

#啟動容器
docker start <container-id>

-a  #前景觀看輸出

#停止容器
docker stop <container-id>

#重啟容器
docker restart <container-id>

#進入容器
docker attach <container-id>  #退出後會停止容器
docker exec -it <container-id> /bin/bash  #退出後不會停止容器

#刪除容器
docker rm <container-id>
-f  #強制刪除

#刪除映像檔
docker rmi <image-name>

#查看容器內的資訊
docker logs

-f  #持續更新輸出

#查詢映像檔
docker search ubuntu
-f is-official=true #過濾指返回官方映像檔

```

Dockerfile 
```
FROM  #docker 映像檔名稱

MAINTAINER  #說明撰寫和維護人資訊

RUN  #執行指令

ARG  #設定參數(Dockerfile中使用)

ENV  #設定環境變數(容器中使用)

VOLUME  #持久化資料
```

Compose
```

docker-compose up -d

docker-compose ps
```
