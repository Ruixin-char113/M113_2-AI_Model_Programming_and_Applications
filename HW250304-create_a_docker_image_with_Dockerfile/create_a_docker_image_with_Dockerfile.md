# HW250304-create_a_docker_image_with_Dockerfile

## Assignment Requirements

### 1. Lab A
How would you create a Dockerfile that inherits from Ubuntu 
version 22.04, and that installs ping and runs ping when a 
container starts? The default address used to ping should be 
127.0.0.1
> Note: ping is in the iputils-ping package

### 2. Lab B  
Create a Dockerfile that uses multiple steps to create an image of a Hello World app of minimal size, written in C

## Demo Video on YouTube
[<img src="https://img.youtube.com/vi/VA8ql94by_g/maxresdefault.jpg" width="50%">](https://youtu.be/VA8ql94by_g)

## 實驗

### 1. Lab A
#### 建立 Dockerfile
``` Dockerfile
FROM ubuntu:22.04
RUN apt update && apt install -y iputils-ping
ENTRYPOINT [ "ping" ]
CMD ["127.0.0.1"]
```
* ```FROM ubuntu:22.04``` base image: ubuntu:22.04。
* ```RUN apt update && apt install -y iputils-ping``` 安裝 ```ping``` 所需要的套件。
* ```ENTRYPOINT [ "ping" ]``` 定義 Command
* ```CMD [ "127.0.0.1" ]``` 定義 Parameter

#### Build Image
``` powershell
docker image build -t hw0304laba .
```
* ```-t``` tag，後面串 Image 名字、標籤。
* ```hw0304laba``` Image 的名字。
* ```.``` Dockerfile 所處路徑。
> 請注意終端機所處的工作目錄路徑

#### 建立新容器
##### 預設
```
docker container run --rm -it hw0304laba
```
* ```--rm``` Container 狀態 exit 時，自動刪除容器。
* ```hw0304laba``` Image 的名字。
##### 覆蓋
```
docker container run --rm -it hw0304laba 8.8.8.8
```
* 由於在 Dockerfile 裡有 ```CMD [ "127.0.0.1" ]```，因此我們可以覆蓋參數。
* 這裡覆蓋掉原來 ```ping``` 的位置 ```127.0.0.1``` 改為 ```8.8.8.8```，亦可改成其他位置。

### 2. Lab B
#### 建立對照組 Dockerfile 及 hello.c
* Dockerfile
``` Dockerfile
FROM alpine:3.12
RUN apk update && apk add --update alpine-sdk
RUN mkdir /app
WORKDIR /app
COPY . /app
RUN mkdir bin
RUN gcc -Wall hello.c -o bin/hello
CMD /app/bin/hello
```
* hello.c
``` c
#include <stdio.h>
int main(void){
    printf("Hello World!\n");
    return 0;
}
```
#### 建立對照組 Image
```
 docker image build -t hw0304labb-single_step .
```

#### 查看對照組 Image
```
 docker image ls hw0304labb-single_step
```
#### 建立對照組容器
```
docker container run --rm -it hw0304labb-single_step
```


#### 建立實驗組 Dockfile 及 hello.c
* Dockerfile
``` Dockerfile
FROM alpine:3.12 AS build
RUN apk update && apk add --update alpine-sdk
RUN mkdir /app
WORKDIR /app
COPY . /app
RUN mkdir bin
RUN gcc -Wall hello.c -o bin/hello

FROM alpine:3.12
COPY --from=build /app/bin/hello /app/hello
CMD /app/hello
```
* hello.c
``` c
#include <stdio.h>
int main(void){
    printf("Hello World!\n");
    return 0;
}
```

#### 建立實驗組 Image
```
docker image build -t hw0304labb-multi_step .
```

#### 查看實驗組 Image
```
docker image ls hw0304labb-multi_step
```

#### 建立對照組容器
```
docker container run --rm -it hw0304labb-multi_step
```

#### 實驗結果
```
docker image ls | Select-String -Pattern labb
```
* 注意所佔的容量，Multi-Step 容量小於 Single-Step。