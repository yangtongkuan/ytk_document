# 			docker基础命令学习

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
systemctl enable dokcer
```

#### 2. docker 镜像（images）相关命令

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

#### 3. docker 容器（container）相关命令

> 镜像和容器的关系 就好比 实体类和实例的关系一样 镜像运行（创建）后就会变成容器

+++







