# Dockerfile文件编写

## 常用的命令

### FROM命令

拉docker的镜像

注意：  可以先在docker client环境下拉ubuntu镜像, 运行这个镜像并安装python的环境，再将容器保存为镜像。在Dockerfile中直接FROM 保存的镜像。

提交容器为镜像:

```
docker commit -m 'spider的image' -a 'disen' spider spider/ubuntu:1.0
```

​     参数说明：

​		 -m 表示提交的镜像的说明

​		 -a 作者的姓名

​		 spider 是容器名

​	     spider/ubuntu:1.0 是镜像名

Dockerfile的From如:

```
FROM spider/ubuntu:1.0
```

###  MAINTAINER 命令

指定用户名和邮箱

MAINTAINER  disen  610039018@qq.com

### ADD 命令

将当前宿主机下的文件资源复制到容器中

如:

```
ADD dushu /usr/src
```

将与Dockerfile同级目录下的文件及子目录复制到容器下的/usr/src目录下

### RUN命令

执行容器的linux命令

### CMD 命令

镜像运行时执行的命令

### WORKDIR 命令

在容器中，切换到某一个工作目录, 之后的命令中文件路径是相对于当前工作目录的。

如:

```
WORKDIR  /usr/src/
```

## 编译Dockfile文件

```
docker build -t disen/book:1.0 .
```

## 注意

如果执行的linux命令较多，或者最后执行的命令比较复杂，可以编写shell脚本，将CMD执行这个shell脚本。

```
#!/bin/bash
# 当前目录是 /usr/src
source venv/bin/activate
pip install scrapy pymysql
cd dushu
scrapy crawl book
```

## 扩展

### 1. 查看容器运行的日志

```
docker logs 容器的名称或ID
```

###  2. 停止容器

```
docker stop 容器的名称或ID
```

### 3. 启动容器

```
docker start 容器的名称或ID
```

### 4. 执行容器的命令

```
docker exec 容器的名称或ID  命令 [命令参数]
```

