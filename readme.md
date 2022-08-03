



doucker(by woldier,power by tencent cloud)

## 1. Docker命令

###  1.1 进程相关命令

- 启动docker服务: `systemctl start docker  `
- 停止docker服务:` systemctl stop docker  `
- 重启docker服务: `systemctl restart docker` 
- 查看docker服务状态: `systemctl status docker`  
- 设置开机启动docker服务: `systemctl enable docke`

### 1.2 镜像相关命令

- 查看镜像: 查看本地所有的镜像 docker images docker images –q # 查看所用镜像的id 

![image-20220802102633880](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802102633880.png)

- 搜索镜像:从网络中查找需要的镜像 docker search 镜像名称 

![image-20220802102729017](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802102729017.png)

- 拉取镜像:从Docker仓库下载镜像到本地，镜像名称格式为 名称:版本号，如果版本号不指定则是最新的版本。 如果不知道镜像版本，可以去docker hub 搜索对应镜像查看。 docker pull 镜像名称 

查看redis可用的tag

![image-20220802102758708](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802102758708.png)

拉取镜像

```shell
docker pull redis:5.0
```



- 删除镜像: 删除时指定的是image的id

```shell
 docker rmi 镜像id # 删除指定本地镜像 
 docker rmi 'docker images -q' # 删除所有本地镜像
 docker rmi 'docker images -q redis:5.0' # 删除redis:5.0
```

### 1.3 容器相关命令

- 查看容器

```shell
docker ps #查看正在运行的镜像
docker ps -a #查看所有镜像
```



![image-20220802103541840](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802103541840.png)

![image-20220802103553443](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802103553443.png)

-  创建并启动容器 

```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

参数说明： 

- -i：保持容器运行。通常与 -t 同时使用。加入it这两个参数后，容器创建后自动进入容器中，退出容器后，容器自动关闭。 
- -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用。 

-  -d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用`docker exec `进入容器。退出后，容器不会关闭。
- -it 创建的容器一般称为交互式容器，-id 创建的容器一般称为守护式容器 • --name：为创建的容器命名。



注: 

- 使用 -it 启动的容器 一旦退出控制台 就退出了 ,接下来进行实验

新建并启动centos:7镜像

```shell
 docker run -it \ #指定容器运行分配一个客户端并且进入,退出后容器关闭
 	--name docker-centos7  \ #指定容器名
 	centos:centos7 # 指定镜像
	
```

![image-20220802110452731](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802110452731.png)

执行结束后进入容器内部`root@e4aa2946fe52`后面则为本容器的id,通过`exit`推出容器.

![image-20220802110656215](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802110656215.png)

现在查看下docker正在运行的容器

![image-20220802110743412](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802110743412.png)

为空说明退出`e4aa2946fe52`后,容器自动关闭了

查看下所有容器

![image-20220802110854013](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802110854013.png)

可以找到该容器,并观察到其status为exited

- 使用 -id 启动容器 

```shell
 docker run -id \ # Keep STDIN open even if not attached, Run container in background and print container ID
 	--name docker-centos7-id  \ #指定容器名
 	centos:centos7 # 指定镜像
```

![image-20220802111659370](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802111659370.png)

查看正在运行的镜像

![image-20220802111724900](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802111724900.png)

对运行的镜像创建控制台

```shell
 docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
 Run a command in a running container
 Options:
  -d, --detach               Detached mode: run command in the background
      --detach-keys string   Override the key sequence for detaching a container
  -e, --env list             Set environment variables
      --env-file list        Read in a file of environment variables
  -i, --interactive          Keep STDIN open even if not attached
      --privileged           Give extended privileges to the command
  -t, --tty                  Allocate a pseudo-TTY
  -u, --user string          Username or UID (format: <name|uid>[:<group|gid>])
  -w, --workdir string       Working directory inside the container

```

```shell
docker exec -it docker-centos7-id /bin/bash
```

最后的`COMMAND` 参数`/bin/bash`来自于上图中对应的COMMAD(这个command代表什么)．

相关解释https://blog.csdn.net/lw001x/article/details/107077570

![image-20220802113501370](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802113501370.png)

- 停止容器

`docker stop <containername>`

![image-20220802114350378](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802114350378.png)

- 启动容器

`docker start <containername>`

- 重启容器

`docker restart 容器名称`

- 删除容器

启动中的容器不能删除

```shell
docker rm 容器名称/id

docker rm 'docker ps -aq' # 删除所有容器

```

- 查看容器信息

`docker inspect 容器名称`

## 2. Docker数据卷

### 2.1 数据卷的介绍

数据卷概念

思考：

- Docker 容器删除后，在容器中产生的数据还在吗？

![image-20220802115325143](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802115325143.png)

- Docker 容器和外部机器可以直接交换文件吗？

![image-20220802115334437](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802115334437.png)

- 容器之间想要进行数据交互?

  ![image-20220802115343942](C:\Users\wang1\AppData\Roaming\Typora\typora-user-images\image-20220802115343942.png)

数据卷:

- 数据卷是宿主机中的一个目录或文件
- 当容器目录和数据卷目录绑定后，对方的修改会立即同步
- 一个数据卷可以被多个容器同时挂载
- 一个容器也可以被挂载多个数据卷

数据卷的作用:

- 容器数据持久化
- 外部机器和容器间接通信
- 容器之间数据交换

### 2.2 配置数据卷

####  2.2.1 容器数据卷的挂载

- 创建启动容器时，使用 –v 参数 设置数据卷

```shell
docker run ... –v 宿主机目录(文件):容器内目录(文件) ... 
```

注意:

-  目录必须是绝对路径
- 如果目录不存在，会自动创建
- 可以挂载多个数据卷





- 接下来给出一个挂载的示例:

```shell
#创建一个容器 将本地的/root/data挂载到容器的/root/data_container
docker run -it --name c1 -v /root/data:/root/data_container centos:centos7 

#这里可以不指定 -v参数中 :及其左边的部分 这种方式下docker 会自动在宿主机中创建文件夹进行挂载
docker run -it --name c1 -v /root/data_container centos:centos7 

#可以通过如下命令查看宿主的挂载文件夹路径
docker inspect c1
```

![image-20220802155130126](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802155130126.png)

进入容器内部 并且在/root/data_container目录下创建readme.txt 文档 并且写入一段话

![image-20220802155358570](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802155358570.png)

保存并且退出容器,推出容器后查看容器状态,发现容器已经是停止运行的状态

![image-20220802155522321](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802155522321.png)

这时 我们在宿主机的/root/data目录下查看是否有相应的文件,并且查看该文件的内容

![image-20220802155625332](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802155625332.png)

可以观察到有相应的文件,并且其中的内容与我们在容器中设置的一摸一样.



当前宿主机中的我们刚刚创建的容器起床太为exit,现在我们在宿主机容器挂载的目录下, 新建一个文件并且写入内容,启动容器后查看容器中是否会进行同步.

![image-20220802160140668](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802160140668.png)

写入的数据为 `woldier in host`

现在我们重新进入容器中查看 

```shell
# 运行容器 这里不能用docker start c1因为之前设置c1的时候没有设置后台运行
docker exec -it c1 /bin/bash

#进入相应目录
cd /root/data_container

#查看相应文件
ll

#查看readme-host.txt里面的内容
cat readme-host.txt

```

![image-20220802162005684](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802162005684.png)

与前文对比完全吻合

#### 2.2.2 容器数据的持久化

- 假设本容器被删除后 ,本地文件是否还在呢?

- 删除后重新创建容器并挂载相同位置,是否有反应呢?

```shell
#删除容器
docker rm c1
```

![image-20220802163557726](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802163557726.png)

再宿主机种查看相应文件及内容都在

![image-20220802163650270](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802163650270.png)



之后重新创建容器并且挂载相同路径

```shell
docker run -it --name c2 -v /root/data:/root/data_container centos:centos7 
cat /root/data_container /readme.txt
cat /root/data_container /readme-host.txt
```



![image-20220802163916081](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802163916081.png)

注: 容器里面的路径本应该为/root/data_container 但是这里写代码的时候敲快了



####  2.2.3数据卷的共享

现在我们想要有两个容器共享一个数据卷

![image-20220802164353517](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802164353517.png)

在前文的基础上 我们配置另一个容器c3并且设置挂载卷

```
docker run -it --name c3 -v /root/data:/root/data_container centos:centos7 
cat /root/data_container /readme.txt
cat /root/data_container /readme-host.txt
```



我们重新连接一个宿主命令控制台执行上述命令

![image-20220802164655476](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802164655476.png)

可以观察到与宿主机及c2一致,现在我们在c3种修改readme.txt 看看是否会进行同步

![image-20220802164808799](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802164808799.png)

c3种添加了 我们现在在c2和宿主机种查看是否有变化

![image-20220802164855315](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802164855315.png)

可以看到c2有变化 ,但是由于之前代码设置挂载点的问题设置的内部挂载点为/root/data,所以这里的路径有所不同.

我们重启起可以cmd ,在宿主机种查看相应文件

![image-20220802165047381](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802165047381.png)

### 2.3 配置数据卷容器

多容器进行数据交换 

1. 多个容器挂载同一个数据卷 
2. 数据卷容器

这里c1,c2是传统容器,而c3充当了数据卷容器,c3与宿主机进行挂载,c1,c2挂载到c3间接实现了挂载到宿主机

![image-20220802165325761](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802165325761.png)

- 首先先创建c3(在这之前需要删除潜泳使用到的同名的容器`docker rm c2 c3`)

```shell
docker run -it --name c3 -v /volume centos:centos7 

#查看宿主机挂载点
docker inspect c3
```

查看宿主机挂载地址

![image-20220802171359446](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802171359446.png)

- 创建c1,c2容器,并且使用--volumes-from参数设置数据卷

```shell
docker run -it --name c1 --volumes-from c3 centos:centos7
docker run -it --name c2 --volumes-from c3 centos:centos7 
```

![image-20220802172007027](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802172007027.png)

可以查看到c1,c2都有创建与c3相同的/volume数据卷文件夹

另起一个cmd 查看c1,c2的详细信息,可以看到 ,c1.c2其实是直接挂载到宿主的,个人猜测这样做的目的是为了让代码更加简洁,不容易出现路径拼写出错,并且就算c3挂掉了,也是可以进行共享数据卷的.

![image-20220802172221469](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802172221469.png)

### 2.4 小节

#### 2.4.1 概念

宿主机的一个目录或文件

#### 2.4.2 数据卷作用

- 容器数据持久化
- 客户端和容器数据交换
- 容器间数据交换

#### 2.4.3数据卷容器

- 创建一个容器，挂载一个目录，让其他容器继承自该容器( --volume-from )。
- 通过简单方式实现数据卷配置

## 3. 应用的部署

### 3.1 mysql部署

- 容器内的网络服务和外部机器不能直接通信
- 外部机器和宿主机可以直接通信
- 宿主机和容器可以直接通信
- 当容器中的网络服务需要被外部机器访问时，可以将容器中提供服务的端口映射到宿主机的端口上。外部机 器访问宿主机的该端口，从而间接访问容器的服务。
- 这种操作称为：<u>**端口映射**</u>

![image-20220802173210794](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802173210794.png)

1. 搜索mysql镜像

```shell
docker search mysql
```

2. 拉取mysql镜像

```shell
docker pull mysql:5.6
```

3. 创建容器，设置端口映射、目录映射

```shell
# 在/root目录下创建mysql目录用于存储mysql数据信息
mkdir ~/mysql
cd ~/mysql
```

```shell
docker run -id \
-p 3307:3306 \
--name=c_mysql \
-v $PWD/conf:/etc/mysql/conf.d \
-v $PWD/logs:/logs \
-v $PWD/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
mysql:5.6
```

- 参数说明：
  - **-p 3307:3306**：将容器的 3306 端口映射到宿主机的 3307 端口。
  - **-v $PWD/conf:/etc/mysql/conf.d**：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf。配置目录
  - **-v $PWD/logs:/logs**：将主机当前目录下的 logs 目录挂载到容器的 /logs。日志目录
  - **-v $PWD/data:/var/lib/mysql** ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。数据目录
  - **-e MYSQL_ROOT_PASSWORD=123456：**初始化 root 用户的密码。



4. 进入容器，操作mysql

```shell
docker exec –it c_mysql /bin/bash
```

5. 使用外部机器连接容器中的mysql

   

![1573636765632](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/1573636765632.png)

### 3.2 tomcat部署

1. 搜索tomcat镜像

```shell
docker search tomcat
```

2. 拉取tomcat镜像

```shell
docker pull tomcat
```

3. 创建容器，设置端口映射、目录映射

```shell
# 在/root目录下创建tomcat目录用于存储tomcat数据信息
mkdir ~/tomcat
cd ~/tomcat
```

```shell
docker run -id --name=c_tomcat \
-p 8080:8080 \
-v $PWD:/usr/local/tomcat/webapps \
tomcat 
```

- 参数说明：

  - **-p 8080:8080：**将容器的8080端口映射到主机的8080端口

    **-v $PWD:/usr/local/tomcat/webapps：**将主机中当前目录挂载到容器的webapps



4. 使用外部机器访问tomcat

### 3.3 nginx部署

1. 搜索nginx镜像

```shell
docker search nginx
```

2. 拉取nginx镜像

```shell
docker pull nginx
```

3. 创建容器，设置端口映射、目录映射


```shell
# 在/root目录下创建nginx目录用于存储nginx数据信息
mkdir ~/nginx
cd ~/nginx
mkdir conf
cd conf
# 在~/nginx/conf/下创建nginx.conf文件,粘贴下面内容
vim nginx.conf
```

```shell
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




```shell
docker run -id --name=c_nginx \
-p 80:80 \
-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf \
-v $PWD/logs:/var/log/nginx \
-v $PWD/html:/usr/share/nginx/html \
nginx
```

- 参数说明：
  - **-p 80:80**：将容器的 80端口映射到宿主机的 80 端口。
  - **-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf**：将主机当前目录下的 /conf/nginx.conf 挂载到容器的 :/etc/nginx/nginx.conf。配置目录
  - **-v $PWD/logs:/var/log/nginx**：将主机当前目录下的 logs 目录挂载到容器的/var/log/nginx。日志目录

4. 使用外部机器访问nginx

###  3.4 redis部署

1. 搜索redis镜像

```shell
docker search redis
```

2. 拉取redis镜像

```shell
docker pull redis:5.0
```

3. 创建容器，设置端口映射

```shell
docker run -id --name=c_redis -p 6379:6379 redis:5.0
```

4. 使用外部机器连接redis

```shell
./redis-cli.exe -h 192.168.149.135 -p 6379
```

## 4. Dockerfile

- Docker 镜像本质是什么？ • 是一个分层文件系统 

- Docker 中一个centos镜像为什么只有200MB，而一个centos操作系统的iso文件要几个个G？ 

  Centos的iso镜像文件包含bootfs和rootfs，而docker的centos镜像复用操作系统的bootfs，只有rootfs和其他镜像层 

- Docker 中一个tomcat镜像为什么有500MB，而一个tomcat安装包只有70多MB？

   由于docker中镜像是分层的，tomcat虽然只有70多MB，但他需要依赖于父镜像和基础镜像，所有整个对外暴露的 tomcat镜像大小500多MB 思



###  4.1 Docker 镜像原理

- Docker镜像是由特殊的文件系统叠加而成 最底端是 bootfs，并使用宿主机的bootfs 
- 第二层是 root文件系统rootfs,称为base image • 然后再往上可以叠加其他的镜像文件 
- 统一文件系统（Union File System）技术能够将不同的 层整合成一个文件系统，为这些层提供了一个统一的视角 ，这样就隐藏了多层的存在，在用户的角度看来，只存在 一个文件系统。 
- 一个镜像可以放在另一个镜像的上面。位于下面的镜像称 为父镜像，最底部的镜像成为基础镜像。 
- 当从一个镜像启动容器时，Docker会在最顶层加载一个读 写文件系统作为容器

Docker 镜像如何制作？



### 4.2 Dockerfile概念及作用

1. 容器转为镜像

   这种方式下若容器内部有加载的数据卷,其中的数据是不会被制作保存到镜像的,但是其他容器内的文件会保存下来.

```shell
#把容器转换为镜像
docker commit 容器id 镜像名称:版本号

#把镜像进行保存以便传输
docker save -o 压缩文件名称 镜像名称:版本号

#在目标宿主机解压镜像
docker load –i 压缩文件名称
```



2. dockerfile

- Dockerfile 是一个文本文件
- 包含了一条条的指令
- 每一条指令构建一层，基于基础镜像，最终构建出一个新的镜像
- 对于开发人员：可以为开发团队提供一个完全一致的开发环境
- 对于测试人员：可以直接拿开发时所构建的镜像或者通过Dockerfile文件构建一个新的镜像开始工作了
- 对于运维人员：在部署时，可以实现应用的无缝移植

![image-20220802210105065](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802210105065.png)





### 4.3 Dockerfile关键字

## 

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

### 4.4 Dockerfile案例



- 案例1：需求

在使用官方的centos时发现没有`vim`命令,并且默认的公做目录为 `/`

自定义centos7镜像。要求：

1. 默认登录路径为 /usr
2. 可以使用vim

实现步骤:

1. 定义父镜像：`FROM centos:7` 
2. 定义作者信息：`MAINTAINER itheima` 
3. 执行安装vim命令： `RUN yum install -y vim`  #注意这里不加-y参数可能会起停下来
4. 定义默认的工作目录：`WORKDIR /usr` 
5. 定义容器启动执行的命令：`CMD /bin/bash` 
6. 通过dockerfile构建镜像：`docker bulid –f dockerfile文件路径 –t 镜像名称:版本 .`



---

- 案例2:需求

定义dockerfile，发布springboot项目

实现步骤:

1. 定义父镜像：FROM java:8
2. 定义作者信息：MAINTAINER woldier
3. 将jar包添加到容器： ADD springboot.jar app.jar (需要把springboot.jar放在dockerfile相同的文件夹下)
4. 定义容器启动执行的命令：CMD java–jar app.jar
5. 通过dockerfile构建镜像：docker bulid –f dockerfile文件路径 –t 镜像名称:版本 . 

```text
FROM java:8
MAINTAINER woldier
ADD springboot-hello-0.0.1-SNAPSHOT.jar app.jar
CMD java -jar app.jar
```

```shell
# 注意最后还有一个·号 ,.号带面当前的上下文环境,才能找到 springboot.jar 
docker build -f ./boot_dockerfile -t boot:1.0 .
```

![image-20220802214652514](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220802214652514.png)

现在生成一个容器

```shell
docker run -id --name c_boot -p 80:8080 boot:1.0 
```

## 5. Docker 服务编排

### 5.1 服务编排

微服务架构的应用系统中一般包含若干个微服务，每个微服务一般都会部署多个实例，如果每个微服务都要手动启停 ，维护的工作量会很大。

- 要从Dockerfile build image 或者去dockerhub拉取image
- 要创建多个container
- 要管理这些container（启动停止删除）

### 5.2 Docker Compose 概述

Docker Compose是一个编排多容器分布式部署的工具，提供命令集管理容器化应用的完整开发周期，包括服务构建 ，启动和停止。使用步骤：

1. 利用 Dockerfile 定义运行环境镜像
2. 使用 docker-compose.yml 定义组成应用的各服务
3. 运行 docker-compose up 启动应用

![image-20220803100119528](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803100119528.png)

### 5.3 docker安装及使用

####  5.3.1 安装Docker Compose

```shell
# Compose目前已经完全支持Linux、Mac OS和Windows，在我们安装Compose之前，需要先安装Docker。下面我 们以编译好的二进制包方式安装在Linux系统中。 
curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /export/server/docker-compose
# 设置文件可执行权限 
chmod +x /export/server/docker-compose
# 查看版本信息 
docker-compose -version

```

出现找不到命令,这是因为我们没有安装到/usr/bin/docker-compose二进制目录下

![image-20220803102643490](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803102643490.png)

因此我们需要/export/server/docker-compose 与/usr/bin/docker-compose建立软连接

ps: 如果第一步直接下载到/user/bin/docker-compose 则不会出现上述问题

```shell
#创建软连接 
 ln -s /export/server/docker-compose /usr/bin/docker-compose
```

在/usr/bin文件夹中查看

![image-20220803103006623](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803103006623.png)

![image-20220803102800620](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803102800620.png)



####  5.3.2 卸载Docker Compose

```shell
# 二进制包方式安装的，删除二进制文件即可
rm /usr/bin/docker-compose
rm /export/server/docker-compose
```

#### 5.3.3 使用docker compose编排nginx+springboot项目

1. 创建docker-compose目录

```shell
mkdir -p /export/config/docker-compose
cd /export/config/docker-compose
```

2. 在/export/config/docker-compose目录下编写 docker-compose.yml 文件


![image-20220803104115170](C:\Users\wang1\AppData\Roaming\Typora\typora-user-images\image-20220803104115170.png)

```shell
version: '3'
services:
  nginx:
   image: nginx
   ports:
    - 80:80
   links:
    - app #表示与其他容器建立连接,可以通过该容器名指代ip
   volumes:
    - ./nginx/conf.d:/etc/nginx/conf.d
  app:
    image: app
    expose:
      - "8080"
```

3. 创建./nginx/conf.d目录

```shell
mkdir -p ./nginx/conf.d
```



4. 在./nginx/conf.d目录下 编写woldier-nginx.conf文件

![image-20220803104442402](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803104442402.png)

```shell
server {
    listen 80;
    access_log off;

    location / {
        proxy_pass http://app:8080;
    }
   
}
```

5. 在/export/config/docker-compose 目录下 使用docker-compose 启动容器

```shell
docker-compose up
#后台运行
docker-compose up -d
```

6. 测试访问

```shell
http://192.168.59.130/hello
```

## 6. Docker 私有仓库

###  6.1 私有仓库搭建

```shell
# 1、拉取私有仓库镜像 
docker pull registry
# 2、启动私有仓库容器 
docker run -id --name=registry -p 5000:5000 registry
# 3、打开浏览器 输入地址http://私有仓库服务器ip:5000/v2/_catalog，看到{"repositories":[]} 表示私有仓库 搭建成功
# 4、修改daemon.json   
vim /etc/docker/daemon.json    
# 在上述文件中添加一个key，保存退出。此步用于让 docker 信任私有仓库地址；注意将私有仓库服务器ip修改为自己私有仓库服务器真实ip 
{"insecure-registries":["私有仓库服务器ip:5000"]} 
# 5、重启docker 服务 
systemctl restart docker
docker start registry

```

![image-20220803110732628](C:\Users\wang1\AppData\Roaming\Typora\typora-user-images\image-20220803110836857.png)



### 6.2 将镜像上传至私有仓库

```shell
# 1、标记镜像为私有仓库的镜像     
docker tag centos:7 私有仓库服务器IP:5000/centos:7
 
# 2、上传标记的镜像     
docker push 私有仓库服务器IP:5000/centos:7

```

![image-20220803111041188](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803111041188.png)

![image-20220803111150535](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803111150535.png)

![image-20220803111210543](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803111210543.png)

###  6.3 从私有仓库拉取镜像 

```shell
#拉取之前先删除本地的
docker rmi 私有仓库服务器ip:5000/centos:7
#拉取镜像 
docker pull 私有仓库服务器ip:5000/centos:7
```

![image-20220803111406135](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803111406135.png)

![image-20220803111430331](https://woldier-pic-repo-1309997478.cos.ap-chengdu.myqcloud.com/image-20220803111430331.png)