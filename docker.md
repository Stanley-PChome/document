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

#建立並啟動容器
docker run -it <images-name>

-it  #進入container shell下指令
-d  #container後台運作
-p <port>:<container-port>  #container內部使用的port映射至主機
--name  #別名

#啟動容器
docker start <container-id>

#停止容器
docker stop <container-id>

#重啟容器
docker restart <container-id>

#進入後台
docker attach <container-id>  #退出後會停止容器
docker exec <container-id>  #退出後不會停止容器

#刪除容器
docker rm -f <container-id>

#查詢容器log
docker logs

-f  #持續更新輸出

```
