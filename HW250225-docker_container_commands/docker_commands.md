# HW250225-docker_container_commands

## Assignment Requirements

upload a video showing you are familiar with the following docker container commands:

|No. |                  |            |
|:--:|:-----------------|:-----------|
|  1 | docker container | run [-itd] |
|  2 |                  | create     |
|  3 |                  | start      |
|  4 |                  | stop       |
|  5 |                  | pause      |
|  6 |                  | unpause    |
|  7 |                  | ps [-l -a] |
|  8 |                  | logs       |
|  9 |                  | attach     |
| 10 |                  | exec       |
| 11 |                  | rm         |


## Demo Video on YouTube
[<img src="https://img.youtube.com/vi/_xMTxGzy-xk/maxresdefault.jpg" width="50%">](https://youtu.be/_xMTxGzy-xk)

## 實驗

### Run a Container
* 在容器內執行命令  
    * ```docker container run alpine echo “Hello World”```  
        * ```docker``` Docker CLI
        * ```container``` context
        * ```run``` command
        * ```alpine``` container image
        * ```echo “Hello World”``` process to run inside the container
    * 利用 ```docker container ps -l``` 查看時會發現容器已經結束(Exited)

* -itd
    * ```docker container run -itd ubuntu bash```  
        * ```-i``` Keep STDIN open even if not attached
        * ```-t``` Allocate a pseudo-TTY
        * ```-d``` Run container in background and print container ID
    * 單獨 ```-i``` 接不上bash。無法利用```exit```結束，需要```ctrl + c```。
    * 單獨 ```-t``` 接不上STDIN。無法打字，需要```ctrl + c```三次退出。
    * 單獨 ```-d``` 背景執行。本案例因無任務而關閉，無法利用```docker container exec -it <ID> bash```進入。

### Create a Container
* 建立一個容器
    * ```docker container create -it ubuntu bash```
    * 可以利用```docker container start <id>```啟動所建立的容器。

### Start/Stop/Restart a Container
* 啟動容器
    * ```docker container start <id>```
    * \<id>不需要全名，可識別即可。
* 停止容器
    * ```docker container stop <id>```
    * 安全關閉。
* 重新啟動容器
    * ```docker container restart <id>```
    * 用於崩潰，更新設定。

### Pause/Unpause a Container
* 暫停容器
    * ```docker container pause id```
    * 用來釋放資源，可以給其他容器使用。
    * 可以從Docker Desktop看到資源被釋出。
* 解除暫停
    * ```docker container unpause id```

### Process Status (PS) in Docker
* 列出容器狀態
    * ```docker container ps -l (-a)```
    * ```-l``` 最新的容器
    * ```-a``` 所有容器
    * ```-q``` 僅顯示ID

### logs
* 顯示容器的紀錄
    * ```docker container logs <id>```
    * ```docker logs <id>```

### Attach The Console Back to a Container
* 連接上容器
    * ```docker container attach <id>```

### Exec
* 執行命令
    * ```docker container exec -it <id> bash```
    * ```docker container exec <id> ps -aef```

### Remove Container
* 刪除容器
    * ```docker container rm (-f) <id>```
    * ```-f``` 強制刪除
* 刪除所有容器
    * ```docker container rm -f $(docker container ps -aq)```
    * 搭配```ps -aq```指令，回傳所有容器ID


## References
* 上課教材
* Docker 說明文件