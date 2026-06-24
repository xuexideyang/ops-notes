# Docker

## 容器和虚拟机的区别
- 容器：共享宿主机内核，进程级隔离，启动快（秒级），资源占用小（MB级）。
- 虚拟机：独立内核，系统级隔离，启动慢（分钟级），资源占用大（GB级）。

## 镜像和容器的关系
- 镜像：只读模板，类似“类”。
- 容器：镜像的运行实例，类似“对象”。

## 常用命令
- `docker run`：运行容器
- `docker ps`：查看运行中的容器（`-a` 查看所有）
- `docker exec -it`：进入容器内部
- `docker logs`：查看容器日志
- `docker stop / rm`：停止 / 删除容器
- `docker images`：查看镜像列表
- `docker rmi`：删除镜像
- `docker push / pull`：推送 / 拉取镜像

## Dockerfile 常用指令
- `FROM`：指定基础镜像
- `RUN`：构建时执行的命令
- `COPY`：复制文件到镜像
- `EXPOSE`：声明端口（仅文档作用）
- `CMD`：容器启动时的默认命令（可被覆盖）
- `ENTRYPOINT`：容器启动命令（不易被覆盖）
- `WORKDIR`：设置工作目录

## 数据持久化
- **Volume**（推荐）：Docker 管理的数据卷，存储在 `/var/lib/docker/volumes/`。
- **Bind mount**：将宿主机指定目录挂载到容器内，适合开发和配置注入。
- **tmpfs**：存于内存，重启消失，适合临时数据。

## 网络模式
- `bridge`（默认）：容器有独立网络命名空间，通过 NAT 与外网通信。
- `host`：共享宿主机网络栈，无隔离，性能好。
- `none`：无网络。
- `container`：与另一个容器共享网络命名空间。

## 容器间通信
- 创建自定义 bridge 网络，将容器加入同一网络，使用容器名互相访问。
- 不推荐用 IP 通信，因为容器重启后 IP 会变化。

## docker-compose
- 用一个 YAML 文件定义多个容器，一键启动所有容器，实现多容器编排。
- 常用命令：`docker-compose up -d`、`docker-compose down`。

## 限制容器资源
- 在 `docker run` 时加参数：
  ```bash
  docker run -d --memory="512m" --cpus="1" nginx

## 镜像分层
- 好处:相同层可重复使用,节省磁盘空间,加快构建速度。


## 容器内访问宿主机服务
- 通过默认网关 172.17.0.1。
- 或使用 host.docker.internal。

## COPY 和 ADD
- COPY：单纯复制本地文件。
- ADD：除复制外，还支持自动解压 tar 文件和 URL 下载。

#  容器起不来的常见原因
- 端口冲突、名称冲突、镜像不存在、启动命令错误、内存/CPU 不足、挂载卷权限问题。

## 停止并删除所有已退出的容器
- docker container prune

## 查看容器日志
- docker logs <容器名>

## 端口映射
- 将容器端口映射到宿主机。