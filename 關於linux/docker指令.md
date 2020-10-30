# 一些快捷指令

`$ sudo docker ps -a` 列出目前已結束的docker container

`$ sudo docker ps ` 列出目前**運行**的docker container

`$sudo docker  rm dockername` 移除dockername container

`$sudo docker  rmi dockerimagesname` 移除docker images

`$sudo docker run docker` 運行docker

`$sudo docker  pull dockerImage` 只下載不運行

`sudo docker run -d dockerImage` 在detach模式下運行

`sudo docker attach dockername` 接回detach的docker

`sudo docker exec ID command` 對運行中的container下指令
