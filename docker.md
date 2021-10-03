

# docker底层原理

docker快的原因：
1）docker比虚拟机的抽象层更少。

2）docker利用的是宿主机的内核，VM需要的是Guest OS系统（例如centos等）

![image-20210129175451709](C:\Users\18270\AppData\Roaming\Typora\typora-user-images\image-20210129175451709.png)

# docker命令

安装docker

```shell
#卸载旧版本
yum remove docker docker-client docker-client-lastest docker-common docker-lastest docker-lastest-logrotate docker-logrotate docker-engine
#需要的安装包
yum install -y yum-utils
#设置镜像的仓库，第一个默认是使用国外的，但可以使用第二个设置阿里云，速度快
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

#安装docker相关
yum install docker-ce docker-ce-cli containerd.io
```

卸载docker

```shell
#卸载依赖
yum remove dcoker-ce docker-ce-cli containerd.io
#删除资源
rm -rf /var/lib/docker
```

## Docker常用命令

```shell
#镜像显示
docker images
docker images -a	#列出所哟肚饿镜像 
docker images -q	#只显示出镜像的id
#镜像搜寻
docker search mysql #搜索所有的镜像
docker search mysql --filter=STARS=3000
#镜像下载
docker pull mysql:lastest	#下载镜像，如果不添加tag，则下载最新的，分层下载
#镜像删除
docker rmi -f imageid	#通过imageid来删除镜像
docker rmi -f $(docker images -aq) 	#通过imageid来删除所有的镜像

#Demo
docker pull centos
#执行
docker run [可选参数] images
#参数说明
--name="Name" 容器名字
-d 			  后台方式运行
-it			  使用交互方式运行
docker run [可选参数] images #运行容器####
#参数说明
--name 容器名字，用来区分容器
-d 后台方式运行
-it 使用交互方式运行，进入容器查看内容
-p 指定容器端口
    -p ip:主机端口:容器端口
    -p 主机端口:容器端口 （常用）
    -p 容器端口
-P 随机指定端口

#交互式方式运行centos
docker run -it centos /
#退出centos
exit	#容器停止并退出
Ctrl+p+q	#容器不停止退出
#列出所有运行的容器
docker ps 命令
-a 	#当前运行的容器+已经运行过的容器
-q	#只显示容器的编号

#删除容器
docker rm 容器id				#删除指定容器，若需要强制删除正在运行的容器，则需要加-f
docker rm -f $(docker ps -aq)	#强制删除所有容器
docker ps -a -q|xargs docker rm #删除所有容器

#启动停止容器操作
docker start 容器id		#启动容器
docker restart 容器id		#重启容器
docker stop	容器id		#停止当前正在运行的容器
docker kill 容器id		#强制停止当前容器

#暂停容器
docker pause 容器id
#继续容器
docker unpause 容器id
#运行中容器的信息
docker top 容器id
```

## Docker其他常用命令

```shell
#后台启动centos
docker run -d centos	#后台运行需要一个前台进程，如果没有前台进行后台应用会自动停止
#查看日志命令
docker logs -tf --tail 10 容器id
#查看容器进程信息ps
docker top 容器id
#查看容器的信息
docker inspect 容器id

#进入当前运行的容器，可以在最后面加/bin/bash
docker exec -it 容器id 
#进入当前运行的容器，进入的是上一次退出时所执行的位置
docker attach 容器id

#从容器内拷贝文件到主机
docker cp 容器id:容器内路径 目的主机路径
```

# 作业nginx部署

```shell
#搜索nginx版本信息
docker search nginx
#默认下载最新的nginx
docker pull nginx
#-d后台运行   --name命名为nginxdemo1  -p将主机端口3344对接为容器80
docker run -d --name nginxdemo1 -p 3344:80 nginx
#运行测试,会获取网页所有信息
curl localhost:3344
```

## 作业tomcat

```shell
#搜索tomcat版本，默认lastest版本
docker pull tomcat
#运行tomcat,-d 后台运行   --name命名为tomcatdemo1
docker run -d -p 3355:8080 --name tomcatdemo1 tomcat
#上面下载的镜像全是最小可运行的环境，没有webapps，需要将webapps.dist中的文件复制到一个新的webapps
```

## Docker镜像讲解

镜像的复制

```shell
#保存镜像，提交镜像，docker images
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[Tag]
docker commit -m="demo" -a="hdt" d4093f4addab centos:1.0.1
```

## 容器数据卷

```shell
#目录的挂载，容器的持久化和同步操作，将容器目录地址挂载到主机的目录地址
docker run -it -v 主机目录地址:容器目录地址 镜像名
#具名挂载，不指定主机目录，
docker run -it -v 容器目录地址 镜像名 
#查看镜像卷匿名名称
docker volume ls

#具名
docker run -it -v 自定义卷名:容器内路径

没有指定主机挂载目录的情况，数据都是挂载在主机的/var/lib/docker/volumes/xxxx/_data

-v 容器内路径		#匿名挂载
-v 卷名:容器内路径	   #具名挂载
-v 主机挂载路径:容器内路径		#指定路径挂载


```



## DockerFile

生成镜像脚本

```shell
FROM centos				#基础镜像
MAINTAINER 				#镜像作者，姓名+邮箱
RUN						#镜像构建的时候需要运行的命令
ADD						#步骤：tomcat镜像，
WORKDIR					#镜像工作的目录
VOLUME					#挂载目录
EXPOST					#暴露端口
CMD						#指定容器启动的时候要执行的命令，只有最后一个会生效，可被替代
ENTRYPOINT				#指定容器启动的时候要执行的命令，可追加命令
ONBUILD					#当构建一个被继承dockerfile这个时候会运行ONBUILD的指令，触发指令
ENV						#够贱的时候设置环境变量

#查看镜像变更历史，可看到镜像怎样生成的
docker history 容器id
#将镜像保存成tar归档文件
docker save -o mycentos.tar hdt/centos:1.0.1
#导入使用save导出的镜像文件
docker load --input mycentos.tar

VOLUME ["volume01","volume02"] #匿名挂载
CMD echo "-----end------"
CMD /bin/bash
```

Demo

```shell
FROM centos
MAINTAINER hdt<1443652986@qq.com>
ENV MYPATH /usr/local
WORKDIR $MYPATH
RUN yum -y install vim
RUN yum -y install net-tools
EXPOSE 80
CMD echo $MYPATH
CMD echo "----end----"
CMD /bin/bash

```

dockerhub

```shell
docker login -u 用户名 -p 密码
docker push 容器名
```



```shell
#执行dockerfile文件脚本,dockerfile 为文件名   hdt/centos:1.0.1为镜像名，最后的. 必须要
docker build -f dockerfile -t hdt/centos:1.0.1 .

#使用
```







## 数据卷容器

```shell
#容器之间的数据共享
#创建docker01容器，
docker run -it --name docker01 hdt/centos:1.0.1
#创建docker02容器，并将docker01挂载到docker02中,则docker01中的所有创建文件都在docker02中
docker run -it --name docker02 --volumes-from docker01 hdt/centos:1.0.1
```



## Docker网络

同一个主机下的容器和容器，容器和主机之间ping ip地址  网络是互通的



通过容器名来连接容器

``` shell
#通过--link 连接tomcat02容器，--link本质是在hosts配置中添加tomcat02的ip，现在不建议这个操作
docker run -d -P --name tomcat03 --link tomcat02 tomcat
#完成上面的后，便可直接ping tomcat02连通，但在tomcat02中不能ping tomcat03

#查看所有的docker网络
docker network ls


docker run -d -P --name centos01 --bridge centos
#创建自定义网络,--driver bridge 桥接，--subnet 192.168.0.0/16 子网，--gateway 192.168.0.1网关
docker net work create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
#将自己容器的网络放置到上面的自定义网络中,这中情况下，不使用ip地址，通过容器名也能ping通
docker run -d -P --name tomcatnet01 --net mynet tomcat
#将tomcat01和mynet连通，
docker network connect mynet tomcat01 
docker exec -it tomcat01 ping tomcatnet01
```



![image-20210202092122816](C:\Users\18270\AppData\Roaming\Typora\typora-user-images\image-20210202092122816.png)









``` 	shell
# 查看docker进程
docker ps 
# 停止进程
docker stop 进程id
# 启动centos，将容器内的地址映射挂载到主机内，之后目录共享
docker run -it -v 主机目录:容器目录 
docker run -it -v /home/ceshi:/home centos /bin/bash
# 查看容器信息
docker inspect 容器id
#启动nginx镜像,设置端口为3355:8080：
docker run -it -p 3355:8080 nginx
#匿名挂载,未指定容器外的挂载地址，本地生成匿名卷
docker run -d -p --name nginx01 -v /ect/nginx nginx
#查看本地卷的匿名卷，是在挂载的时候只写了容器内的路径
docker volume ls
#查看本地名称为juming-nginx的匿名卷存储位置
docker volume inspect juming-nginx
-v 卷名:容器内路径   #具名挂载
-v 容器内路径		#匿名挂载


  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```

## 发布自己的镜像

Dockerhub







