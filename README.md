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

# DockerFile
dockerfile是用来构建docker image的文件，是一种命令参数脚本
1. 编写一个dockerfile文件
2. docker build构建成为一个image
3. docker run运行image
4. docker push发布image到hub

dockerfile: 构建文件，定义了一切步骤，源代码
Docker image: 通过dockerfile构建生成的镜像，最终发布和运行的产品
Docker 容器: 就是镜像运行起来提供服务的

每个关键字(指令)都必须是大写字母，执行顺序从上到下，#表示注释，每个指令都被创建提交为一个镜像层。
dockerfile是面向开发的，以后要发布项目，做镜像就要编写docker file。

FROM: 指定基础镜像
MAINTAINER: 指定维护者信息，一般为姓名+邮箱
RUN: 镜像构建时需要运行的命令，注意运行是在build期间
ADD: copy文件/内容，会自动解压，例如tomcat的镜像要添加一个tomcat的压缩包
WORKDIR: 设置当前工作目录
VOLUME: 设置卷，挂载主机目录
EXPOSE: 指定对外的接口
CMD: 指定运行的命令，只有最后一个会生效，且可被替代，运行是在docker run时运行
ENTRYPOINT: 指定运行的命令，可以追加命令，运行是在docker run时运行
ONBUILD: 当构建一个被继承docker file时会运行ONBUILD指令
COPY: 类似ADD，将文件拷贝到镜像中
ENV: 构建时设置环境变量

CMD和ENTRYPOINT的区别: 如果是CMD，在docker run后面指定的命令会替换CMD指定的命令，而ENTRYPOINT的话会把docker run的指令追加到ENTRYPOINT后的命令
例如 CMD ["ls","-a"], 在docker run image -l会报错，因为相当于去执行-l命令，如果是ENTRYPOINT ["ls","-a"], 则相当于执行ls -a -l

# Docker网络
我们每启动一个docker容器，docker就会给docker容器分配一个ip, 只要安装了docker, 就会有一个网卡docker0
veth-pair就是一对虚拟设备接口，它们都是成对出现，一端连着协议，一端彼此相连。




