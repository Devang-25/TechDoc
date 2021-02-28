# Docker Command

1. __Image相关指令__

| 命令 | 说明 |
| ---- | ---- |
| docker image ls | 列出所有image |
| username/repository:tag | image的一般命名法，username用于远程仓库登录，repository为远程仓库名 |
| docker image rm username/repository:tag | 删除指定tag，如果image只有一个tag，则删除image |
| docker rmi username/repository:tag | 删除指定tag，如果image只有一个tag，则删除image |
| docker run -d -p 4000:80 [image name] | 后台运行指定image，把container内部80端口映射到宿主机4000端口上，外部通过4000访问服务。如果本地不存在该image，则从远程仓库拉取 |
| docker tag [image name] username/repository:tag | 为指定image命名，并打tag |
| docker push username/repository:tag | 把image push到远程仓库 |
| -v [host path]:[container path]:[model] | 挂载数据卷volume，model默认为rw，此时主机目录和容器目录任意一方修改都会反映到对方。ro只读模式时只能从宿主机反映到容器，而无法修改容器中的关联内容 |

2. __Container相关指令__

| 命令 | 说明 |
| ---- | ---- |
| docker ps ; docker container ls | 显示所有在运行的container |
| docker ps -a ; docker container ls -a | 显示所有container |
| docker container rm -f [container id/container name] ; docker rm -f [container id/container name] | 强制删除指定container |
| docker container rm $(docker container ps -aq) | 删除所有容器 |
| docker container start [container id/container name] | 启动指定container |
| docker container stop [container id/container name] | 停止指定container |

3. __Service相关指令__

| 命令 | 说明 |
| ---- | ---- |
| docker service ls | 列出所有service |
| docker service ps [service name] | 列出指定service，包括所有副本信息 |

4. __Swarm/Stack相关指令__

| 命令 | 说明 |
| ---- | ---- |
| docker swarm init | 把当前机器初始化为swarm manager |
| docker stack deploy -c [yml config file(ex. docker-compose.yml)] [stack name(ex. getstartedlab)] | 部署一个新的stack或者更新一个旧的stack |
| docker stack rm [stack name(ex. getstartedlab)] | 删除指定stack |
| docker swarm leave --force | 离开swarm |
| docker-machine create --driver virtualbox myvm1 | 创建一个名称为myvm1的virtualbox |
| docker-machine ssh myvm1 "docker swarm init --advertise-addr [myvm1 ip:port]" | 向myvm1发送命令，让其成为swarm manager，advertise-addr为指定ip和port，port一般用默认端口 |
| docker-machine ssh myvm2 "docker swarm join --token SWMTKN-1-1gfjzov1d6sa8tvqqjenrhau5p45749dxk0rngnen8ya2azjek-8zir0ugrcow6rr2dg7weg59rr [myvm1 ip:port]" | 向myvm1发送命令，让其成为swarm worker，token是myvm1成为swarm manager是自动生成的 |
| docker node ls | 在swarm manager上执行，显示当前swarm所有node |
| docker-machine env myvm1 | 打印跟myvm1进行通讯的所需环境变量 |
| eval $(docker-machine env myvm1) | 倒入跟myvm1进行通讯的所需环境变量，这样在执行docker stack deploy等命令是就可以直接执行，而不需要ssh |
| eval $(docker-machine env -u) | 移除跟myvm1进行通讯的所需环境变量 |

5. __其他指令__

| 命令 | 说明 |
| ---- | ---- |
| docker inspect [image id/image name] | 获取镜像源信息 |
| docker inspect [container id/container name] | 获取容器源信息 |
| docker exec -it [container id/container name] /bin/bash | 进入容器 |
| docker logs [container id/container name] | 查看容器日志 |
| docker cp [host path] [container id/container name]:[path] | 复制宿主机文件到容器。反过来写就可以复制容器文件到宿主机 |
| docker commit [container id/container name] [new image name:tag] | 基于指定容器生成新镜像 |
| docker build -f [Dockerfile path] -t [new image name:tag] [command context(always use .)] | 用Dockerfile生成新镜像 |
| docker history [image id/image name] | 查看镜像构建历史 |