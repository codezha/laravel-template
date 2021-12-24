# sail
```shell script
sail artisan queue:work
sail node --version
sail npm run prod
```


# laradock
```shell script
# 进入容器
docker-compose exec workspace bash


```
# docker
```shell script
# 卸载旧版Docker
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

# 添加Docker安装源
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# 安装 Docker
sudo yum install docker-ce docker-ce-cli containerd.io
# 安装指定版本的 Docker
sudo yum list docker-ce --showduplicates | sort -r
# 选取想要的版本
sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
# 安装完成启动Docker
sudo systemctl start docker
# 国际惯例hello world
sudo docker run hello-world
# 允许普通用户执行 docker 命令
sudo groupadd docker && sudo gpasswd -a ${USER} docker && sudo systemctl restart docker
docker images  # 列出所有镜像
docker image ls xxxx  # 查询指定的镜像
docker images |grep busybox  # 过滤查询镜像
docker tag busybox:latest mybusybox:latest # 重命名镜像
docker rmi mybusybox  # 删除镜像
docker image rm mybusybox  # 删除镜像
# 推送镜像
docker login #先登录
docker tag hello-world codezha/firstcomm #重命名
docker push codezha/firstcomm # 推送

```


