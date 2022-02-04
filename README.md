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


