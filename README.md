# Docker-Learning

# Docker命令
# 帮助命令
docker version: 显示docker版本信息
docker info: 显示docker的系统信息
docker 命令 --help: 帮助命令

# 镜像命令
docker images: 查看本地已有的镜像 -a -q
docker search 镜像名: 在hub上搜索镜像
docker pull 镜像名[:tag]: 从hub上拉取镜像
docker rmi <镜像名/镜像id> <镜像名/镜像id>: 删除镜像
docker rmi -f $(docker images -qa): 删除所有image

# 容器命令
docker run [可选参数] image: 新建容器并启动 
docker run -it  centos /bin/bash
    --name="Name": 给容器起名字
    -d: 后台运行
    -it: 使用交互方式运行，可进入容器查看内容
    -p: 指定端口
        -p ip:主机端口:映射容器端口
        -p 主机端口:映射容器端口(长影)
        [-p ]容器端口
    -P: 随机指定端口

exit: 停止容器并退出

Ctrl + P + Q: 容器不停止退出

docker ps: 查看正在运行的容器
    -a: 正在运行和历史运行记录
    -q: 只显示容器编号

docker rm: 删除容器
    容器id: 删除指定容器
    -f $(docker ps -aq): 删除所有容器 同 docker ps -aq | xargs docker rm -f

docker start 容器id: 启动一个容器
docker restart 容器id: 重启一个容器
docker stop 容器id: stop
docker kill 容器id: kill

# 其它命令
docker run -d 镜像名: 以后台方式运行容器
    坑: 使用后台运行时必须要有前台进程，否则docker发现自己没有提供服务就会自动停止

docker logs: 显示日志

docker top dockerId: 显示进程信息

docker inspect dockerid: 查看docker的元数据

docker exec -it dockerId shell: 以交互的方式进入正在运行的容器，进入容器后开启新的终端
    通常我们以后台方式运行容器，有时我们需要进入容器修改配置
    
docker attach dockerId: 进入正在运行的容器，和上一个方法不同的地方是容器内不开启新的终端

docker cp dockerId:容器路径 主机目的路径 -> 从容器内拷贝文件到主机

docker stats: 查看docker资源使用率

docker commit -m "comment" -a "author" dockerId 目标镜像名[:tag]: 提交容器作为一个新的镜像 

# 容器数据卷
数据不能放到容器中，因为如果容器被删除，数据也会丢失，数据持久化是必要的，因此容器之间可以有一个数据共享的技术，Docker容器中的数据同步到本地，即将容器内的目录挂载到主机，也可以实现容器间的数据共享。

方式一: docker run -v 主机目录:docker目录 ...
    停止容器后，主机的修改在容器start后仍然同步

# 数据卷容器
容器间同步数据
子容器 --volumes-from 父容器
一旦确定了多个容器间的共享，关闭其中几个不影响共享，也就是数据卷容器的生命周期一直持续到没有容器使用为止。


