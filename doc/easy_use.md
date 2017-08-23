# 概念
镜像 -> 类  

容器 -> 对象  

运行中的镜像, 称作容器  



# 查找镜像模板
docker search centos
其中centos是查找的名字
也可以在 [Docker Hub](https://hub.docker.com/) 搜索


# 创建镜像
```
docker run -it --name cc_centos7 centos:7 /bin/bash
```
其中cc_centos7是自定义的名字, centos:7是docker hub上的镜像模板
run表示生成镜像
```
-i --interactive                 Keep STDIN open even if not attached
-t, --tty                         Allocate a pseudo-TTY
```


# 查看镜像
```
docker images
```


# 删除镜像
```
docker rmi [IMAGE ID]
```


# 查看容器
```
docker ps -a
```

# 删除容器
```
docker rm [CONTAINER ID]
```


# 查看容器详情
```
docker inspect [REPOSITORY]
docker inspect cc_dev_server
```


# 从镜像运行一个新容器
```
docker run -it [IMAGE ID] /bin/bash
```
例如:
```
docker run -it cc_dev_server bash
```
需要使用systemd(systemctl), 否则提示"Failed to get D-Bus connection: Operation not permitted"
```
docker run -itd --privileged [IMAGE ID] init
```
映射22端口到38422端口
```
yum -y install openssh-server openssh-clients
systemctl enable sshd.service
systemctl start sshd.service
docker run -d -p 38422:22 [IMAGE ID] /usr/sbin/sshd -D
```


# 挂载硬盘到容器
```
docker run -it -v /data/downloads:/usr/downloads --name cc_centos centos:7 /bin/bash
```
-v 后的参数, :前是物理机路径, :后是容器路径


# 物理机文件与容器内互相传输
```
docker cp [物理机目录] [CONTAINER ID]:[容器内目录]
```
反向则改变两个参数顺序, 例如:
```
docker cp /data/downloads/Python-3.5.3/ 1bc40b1aa622:/data
```


# 进入已存在的docker容器
attach一个容器的stdin, 如果退出, 则容器停止:
```
docker attach [NAMES]
```
新启动一个session:
```
docker exec -it [NAMES] /bin/bash
```


# 保存容器
# export命令用于持久化容器, 相当于vm的快照, 会打包所有的修改
```
docker export [CONTAINER ID] > /home/export.tar
```


# 保存镜像
# save命令用于持久化镜像, 相当于vm的虚拟机, 会打包镜像所有的层
```
docker save [IMAGE NAME] > /home/save.tar
```


# 提交镜像
修改容器不会影响镜像, 使用commit提交, 可以把正在运行的容器创建为新的镜像, 相当于把快照另存为一个虚拟机, 然后在此基础上可以进行虚拟机迁移
```
docker commit [CONTAINER ID] cc_django_server
```
其中 cc_django_server 是新镜像的名字, 例如:
```
docker commit -m='django and scrapy dev server' -a='cc' 1bc40b1aa622 newidfordota/cc_dev_server
```
更新镜像(加上了:v2标签来表示版本):
```
docker commit -m='update vim-plugins.' -a='cc' 1ac5ac962f96 docker.io/newidfordota/cc_dev_server:v2
```

# docker hub登录
```
docker login
```


# docker 上传镜像
```
docker push [IMAGE NAME][:TAG]
```
这里的IMAGE NAME是用户名/镜像名, 例如:
```
docker push docker.io/newidfordota/cc_dev_server:v2
```
否则会提交到dockerHub的根, 没有权限


# docker 下载镜像
```
docker pull docker.io/newidfordota/cc_dev_server:v2
```

# 我的第一个docker镜像
基于centos7, 安装了py3和vim  
[镜像](https://hub.docker.com/r/newidfordota/cc_dev_server/)

使用:  
> 任意安装一个docker宿主, 例如centos7, 参考https://get.daocloud.io/#install-docker  

安装docker:  
> curl -sSL https://get.daocloud.io/docker | sh  

加速器:  
> curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://0e9ecaa5.m.daocloud.io  



