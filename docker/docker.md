# 			docker基础学习

[toc]

### 一.安装docker（linux环境下）

```shell
#1.检查docker内核环境 如果内核版本大于10版本则符合docker的安装环境 
uname -a 
#2. yum 更新道最新
yum update
#3. 现在docker 也可以下载具体版本好的dcoker
sudo yum install docker-ce 
#sudo yum install docker-ce-18.06.1.ce  
#4. 查看docker版本
docker -v
### 卸载docker 查看docker是否在运行
systemctl status docker
## 写在docker 首先停止docker
systemctl stop docker
## 查看docker安装
yum list installed | grep docker
## 卸载docker 
yum remove docker-* -y
## 删除docker的下载镜像
rm -rf /var/lib/docker
```

### 二.使用加速器（以阿里云为例）

#### 1.docker镜像说明

​	docker 陌生默认的下载地址时dockerhub 其服务器在国外，对于国内来说下载速度相当慢。国内知名加速器有阿里云、腾讯云等

#### 2.使用阿里云作为docker下载加速器

1).登录[阿里云](https://www.aliyun.com/),然后搜索镜像加速

![](.\iamges\docker_ali_search.png)

+++

2).选择镜像加速器，阿里云会生成唯一一个加速器地址

![](.\iamges\docker_images.config.png)

+++

3).将如下配置复制到shell脚本中执行

```sh
# 查看是否执行成功 可以看刚才配置的镜像加速地址即可
cat etc/docker/daemon.json
```

+++

### 三.docker的命令

#### 1. docker 服务相关命令

```shell
#1.docker 启动命令
systemctl start docker
#2.查看docker的运行状态
systemctl status docker
#3.停止doker服务
systemctl stop docker
#4.重启docker服务
systemctl restart docker
#5.开机自动启动docker服务
systemctl enable docker
```

#### 2. docker 镜像（images）相关命令

* **说明**

  > docker 下载镜像时，如果指定tag的话，tag必须存在。查阅去hub.docker查询；[传送门](https://hub.docker.com/)

```shell
#1. 查看本地镜像
docker images
#1.1 查看本地镜像id
docker images -q
#2.搜索镜像
docker search image
#3.docker镜像下载 tag 标识版本号  不写默认为最新版本
docker search image:tag
#4.docker删除本地镜像
#4.1 通过镜像名+版本号删除
docker rmi image:tag
#4.2 通过镜像id删除
docker rmi image_id
#5.删除所有的镜像
docker rmi `docker images -q`
```

++++

#### 3. docker 容器（container）相关命令

> 镜像和容器的关系 就好比 实体类和实例的关系一样 镜像运行（创建）后就会变成容器

+++

- 查看容器

- 创建容器

- 进入容器

- 启动容器

- 停止容器

- 删除容器

- 查看容器信息

  ```shell
  #1. 查看容器
  # 1.1 查看进行中的容器
  docker ps
  # 1.2 查看所有的容器
  docker ps -a
  # 2.启动容器 
  #2.1启动并进入容器(交互式容器)
  #docker run -it --name=yourname image:tag /bin/bash
  docker run -it --name=yourname image_id /bin/bash
  # 2.2 后台创建并启动容器 (守护式容器)
  #docker run -id  --name=yourname image:tag /bin/bash
  docker run -id --name=yourname image_id
  #3. 退出容器
  exit
  #4. 进入容器
  docker exec -it your_container_name /bin/bash
  #5. 启动容器
  docker start container_name
  #docker start container_id
  #6. 关闭容器
  docker stop container_name
  #docker stop container_id
  #7. 删除容器
  # 7.1 删除单个容器
  docker rm container_id
  #docker rm container_name
  # 7.2 删除所有的容器
  docker rm `docker ps -aq`
  # 8.查看容器的信息
  docker inspect container_name/container_id
  ```

**参数说明**：

> **-i:保持容器运行。通常与-t同时使用。加入-it参数后，容器创建后会自动进入容器，退出容器后,容器会自动关闭。**
> 	**-t:为容器分配一个伪终端，通常与-i同时使用。**
> 	**-d:以守护进程（后台）模式运行容器。创建一个容器在后台运行,需要执行docker exec 进入容器。退出后，容器不会自动关闭。**
> 	**-it：创建的容器一般称为交互式容器，-id创建的容器一般称为守护式容器**
> 	**--name:为创建的容器命名**
>
> 

### 四.容器的数据卷

#### 1.数据卷的作用

>数据券：是宿主机的一个目录或者文件;当数据券和容器的目录绑定后，对方的修改会立即同步；一个数据卷可以同时被多个容器挂载；
>
>作用：容器数据持久化；宿主机和容器间的通信；容器之间数据交换；

+++



#### 2.配置数据卷

>在docker创建容器时，使用-v参数设置数据卷

```shell
docekr run ... -v 宿主机目录（文件）:容器内目录（文件）.....
```

+++



#### 3.数据卷容器

> 创建一个容器，挂在一个目录，让其他容器集成改容器，事项数据共享;(--volumes-from)

```shell
#1.创建数据卷容器,不填写宿主机的目录会自动生成宿主机的目录
docker run ... -v 容器目录 ... --name data_container_name
# 2. 集成数据券容器 实现容器之间的数据共享
docker run ... --volumes-from data_container_name ...
```

### 五.docker 应用的部署

#### 1. mysql部署

```shell
#1.搜索镜像
docker search mysql

#2.拉去镜像
docker pull mysql:5.7

#3.创建容器
docker run -id \
-p 3306:3306 \
--name=docker_mysql5.7 \
-v $PWD/conf:/etc/mysql/conf.d \
-v $PWD/logs:/logs \
-v $PWD/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=sdbs \
mysql:5.7
# 3.1
docker run -p 3308:3306 --name mysql5.7_01 -v $PWD/conf:/etc/mysql/conf.d  -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=sdbs -d mysql:5.7
#4.进入容器，操作mysql
docker exec –it c_mysql /bin/bash
#5.授权所有的ip都能访问
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'sdbs' WITH GRANT OPTION;
# 刷新权限
flush privileges;
# 查看mysql的字符集
SHOW VARIABLES LIKE 'character%';
```

* **-p 3307:3306**：将容器的 3306 端口映射到宿主机的 3307 端口。
* **-v $PWD/conf:/etc/mysql/conf.d**：将容器的 /etc/mysql/my.cnf挂在到宿主机主机当前目录conf下。配置目录
* -v $PWD/logs:/logs**：将主机当前目录下的 logs 目录挂载到容器的 /logs。日志目录
* **-v $PWD/data:/var/lib/mysql** ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。数据目录
* **-e MYSQL_ROOT_PASSWORD=sdbs：**初始化 root 用户的密码

#### 2.tomcat部署

```shell
#1. 搜索镜像
docker search tomcat
#2. 拉去镜像
docker pull tomcat:8.5
#3. 创建容器
docker run -id --name tomcat8.5\
-p 8080:8080\
-v $PWD/webapps:/usr/local/tomcat/webapps\
tomcat:8.5
#4.启动容器
docker start tomcat8.5;
```

* **参数说明**
  + **-p 8080:8080：**将容器的8080端口映射到主机的8080端口
  + **-v $PWD/webapps:/usr/local/tomcat/webapps：**将容器的webapps 挂在到宿主机的webapps下

#### 3.nginx部署

```shell
#1.搜索镜像
docker search nginx
#2. 拉去镜像
docker pull nginx
#3. 创建镜像
mkdir -p ~/nginx/config
cd ~/nginx/config
vim nginx.config
```

+++

```shell
# nginx配置文件
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

+++

```shell
docker run -id --name=m_nginx \
-p 80:80 \
-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf \
-v $PWD/logs:/var/log/nginx \
-v $PWD/html:/usr/share/nginx/html \
nginx
```

* **参数说明**：
  * **-p 80:80**：将容器的 80端口映射到宿主机的 80 端口。
  * **-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf**：将主机当前目录下的 /conf/nginx.conf 挂载到容器的 :/etc/nginx/nginx.conf。配置目录
  * **-v $PWD/logs:/var/log/nginx**：将主机当前目录下的 logs 目录挂载到容器的/var/log/nginx。日志目录

#### 4.redis 部署

```shell
#1.搜索镜像
docker search redis
#2.拉去镜像
docker pull redis
#3.创建redis容器
docker run -id --name=m_redis\
-p 6379:6379 \
redis
#4. 启动redis容器
docker start m_redis
# 5. 部署redis 并设置挂在点
docker run -p 6379:6379 --name redis -v /usr/local/docker/redis/data:/data -v /usr/local/docker/redis/conf/redis.conf:/etc/redis/redis.conf -d redis redis-server /etc/redis/redis.conf
```

### 六.docker容器制作

* **说明**

  > docker镜像是分为bootfile 和rootfile ; 是一个文件分成系统；

* **方式**

  * 1.容器转为镜像


#### 1.将容器转为镜像

* **说明**

> **将容器转为镜像后，挂在目录或文件不会被放到镜像内**

```shell
# 1.容器转为镜像
docker commit container_id your_image_name:your_tag
docker save -o your_tar_name your_image_name:your_tag
# 2. 还原镜像
docker load -i your_tar_name
```

+++

#### 2.dockerfile制作镜像

| 关键字      | 作用                     | 备注                                                         |
| ----------- | ------------------------ | ------------------------------------------------------------ |
| FROM        | 指定父镜像               | 指定dockerfile基于那个image构建                              |
| MAINTAINER  | 作者信息                 | 用来标明这个dockerfile谁写的                                 |
| LABEL       | 标签                     | 用来标明dockerfile的标签 可以使用Label代替Maintainer 最终都是在docker image基本信息中可以查看 |
| RUN         | 执行命令                 | 执行一段命令 默认是/bin/sh 格式: RUN command 或者 RUN ["command" , "param1","param2"] |
| CMD         | 容器启动命令             | 提供启动容器时候的默认命令 和ENTRYPOINT配合使用.格式 CMD command param1 param2 或者 CMD ["command" , "param1","param2"] |
| ENTRYPOINT  | 入口                     | 一般在制作一些执行就关闭的容器中会使用                       |
| COPY        | 复制文件                 | build的时候复制文件到image中                                 |
| ADD         | 添加文件                 | build的时候添加文件到image中 不仅仅局限于当前build上下文 可以来源于远程服务 |
| ENV         | 环境变量                 | 指定build时候的环境变量 可以在启动的容器的时候 通过-e覆盖 格式ENV name=value |
| ARG         | 构建参数                 | 构建参数 只在构建的时候使用的参数 如果有ENV 那么ENV的相同名字的值始终覆盖arg的参数 |
| VOLUME      | 定义外部可以挂载的数据卷 | 指定build的image那些目录可以启动的时候挂载到文件系统中 启动容器的时候使用 -v 绑定 格式 VOLUME ["目录"] |
| EXPOSE      | 暴露端口                 | 定义容器运行的时候监听的端口 启动容器的使用-p来绑定暴露端口 格式: EXPOSE 8080 或者 EXPOSE 8080/udp |
| WORKDIR     | 工作目录                 | 指定容器内部的工作目录 如果没有创建则自动创建 如果指定/ 使用的是绝对地址 如果不是/开头那么是在上一条workdir的路径的相对路径 |
| USER        | 指定执行用户             | 指定build或者启动的时候 用户 在RUN CMD ENTRYPONT执行的时候的用户 |
| HEALTHCHECK | 健康检查                 | 指定监测当前容器的健康监测的命令 基本上没用 因为很多时候 应用本身有健康监测机制 |
| ONBUILD     | 触发器                   | 当存在ONBUILD关键字的镜像作为基础镜像的时候 当执行FROM完成之后 会执行 ONBUILD的命令 但是不影响当前镜像 用处也不怎么大 |
| STOPSIGNAL  | 发送信号量到宿主机       | 该STOPSIGNAL指令设置将发送到容器的系统调用信号以退出。       |
| SHELL       | 指定执行脚本的shell      | 指定RUN CMD ENTRYPOINT 执行命令的时候 使用的shell            |

* 案例1：创建一个centos7

```shell
# 创建file文件存储路径
mkdir docker-file
cd docker-file
vim centos-file
# 编辑file文件
FROM centos:7
MAITAINER yangtk <yangtongkuan@qq.com>
RUN yum install -y vim
WORKDIR /usr
CMD /bin/bash
# end
# 创建镜像
docker build -f ./centos-file -t yangtk_centos:1 .
# 启动镜像
docker run yangtk_centos:1
```

> **参数说明**
>
> -t : 给镜像命名
>
> -f: 表示执行的文件
>
> FROM: 标识继承哪个镜像
>
> MAITAINER：作者信息
>
> WORKDIR：设置默认工作路径
>
> RUN:标识容器制作时执行的命令脚本,只会在容器启动的时候执行
>
> CMD: 容器启动时执行的命令

+++



* 案例2：将springboot项目通过docker发布