# 查找镜像
    docker search tomcat
    
# 拉取镜像
    docker pull tomcat:8
    
# 删除镜像
    docker rmi ID
    
# 查看容器
    docker ps -a
    
# 启动删除容器
    docker start ID
    dokcer stop ID
    
# 删除容器
    docker rm 容器ID
    
# 创建容器
    docker run 参数 镜像名称:镜像标签 /bin/bash
    
    -i:创建之后立马启动,不加不启动容器
    
    -t:交互式容器,进去容器的命令行(进去一个伪终端)
    
    -d:守护式容器,容器在后台运行
    
    --name:创建容器名称
    
    -v:目录映射(前者是宿主机目录,后者是映射到宿主机上的目录)
    
    -p:端口映射
    
### 运行例子
    docker run -it --name=容器名称 镜像名称:标签 /bin/bash
    
### 进去容器例子
    docker exec -it 容器Id   /bin/bash
    
### 挂载目录例子
    docker run -di -v 宿主机目录:容器的目录 --name=容器名称 镜像名称:标签
    
    -v可以有多个,挂在多个目录
    
### 定义数据卷容器例子
    docker run -di --name c4 -v /home/java5678/data/c4 centos:7
### 挂载数据卷容器例子
    docker run -di --name c5 --volumes-from c4 centos:7
    c4\c5中的文件就可以共享了
    
    
    
# docker拷贝文件
    docker cp aa.html   容器id:/user/tomcat/webapps
    
    docker cp 容器id:/user/tomcat/webapps   aa.html
    
# 查看容器IP
    docker inspect 容器Id
    
    Mounts:查看挂在的目录
    
    Networks:查看ip
    
# docker迁移和备份
### 容器保存为镜像例子
    docker commit 容器Id 镜像名:版本
    docker commit id redis:v1
    
### 容器保存为镜像例子
    docker save -o 压缩包保存路径  镜像id
    docker save -o redis.tar redis:v1
    
### 镜像恢复例子
    docker load -i redis.tar
