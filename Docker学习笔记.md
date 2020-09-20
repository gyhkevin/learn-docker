## Docker学习笔记

#### 获取镜像

```bash
docker pull [选项][Docker Registry 地址[:端口号]/]仓库名[:标签]

$ docker pull ubuntu:16.04
```

#### 运行

```bash
$ docker run -it --rm \
	ubuntu:16.04 \
	bash
```

#### 列出镜像

```bash
$ docker image ls
```

#### 占用空间

```bash
$ docker system df
```

#### 虚悬镜像

```bash
$ docker image ls -f dangling=true
# 删除虚悬镜像
$ docker image prune
```

#### 中间层镜像

```bash
$ docker image ls -a
```

#### 构建镜像
```bash
$ docker build -t gyhkevin/hello-world .
```

#### 列出容器并删除
```bash
docker container ls -a
docker ps -a
docker container rm 98b4a956cd8b
docker rm
docker images
docker rmi 
docker rm $(docker container ls -aq)
docker rm $(docker container ls -f "status=exited" -q)
```
#### 构建容器
```bash
docker container commit = docker commit
docker image build

docker commit condescending_fermi gyhkevin/centos-vim
```
#### Dockerfile语法关键字
```docker
FROM scratch	# 制作base image
FROM centos     # 使用base image
FROM ubuntu:14.04
```
```docker
LABEL maintainer = "gyhaiven@gmail.com"
LABEL version = "1.0"
LABEL description = "详细说明"
```
```docker
RUN yum update && yum install -y vim \
	python-dev  # 反斜杠换行
RUN apt-get update && apt-get install -y perl \
	pwgen --no-install-recommends && rm -rf \
	/var/lib/apt/lists/*	#注意清理cache
RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
```
```docker
WORKDIR /root
WORKDIR /test	# 如果没有会自动创建test目录
WORKDIR demo
RUN pwd		# 输出结果应该是 /test/demo
```
```docker
ADD hello /
ADD test.tar.gz / # 添加到根目录并解压
WORKDIR /root

```
```bash
docker run -d gyhkevin/flask-hello-world
docker run -d --name=demo gyhkevin/flask-hello-world
docker exec -it a9e573e99341 /bin/bash
docker container stop = docker stop
```

## docker network space
```bash
sudo ip netns list
sudo ip netns add test1
sudo ip netns exec test1 ip a
sudo ip netns exec test1 in link set dev lo up 
sudo ip link add veth-test1 type veth peer name veth-test2
sudo ip link set veth-test1 netns test1
sudo ip netns exec test1 ip addr add 192.168.1.1/24 dev veth-test1
sudo ip netns exec test2 ip addr add 192.168.1.2/24 dev veth-test2
sudo ip netns exec test1 ip link set dev veth-test1 up 
sudo ip netns exec test2 ip link set dev veth-test2 up 
sudo ip netns exec test1 ping 192.168.1.2
```
```bash
sudo docker run --name web -p 80:80 -d nginx
```