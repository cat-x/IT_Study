1. #### 在 windows 下挂载磁盘

> win10 docker 使用 run -v 时，虚拟机无法显示宿主机挂载的目录

![](.\res\common1.png)

1. 在设置界面进行共享
2. 如果**修改了 PC 密码** 需要点**重新认证**

```Shell
 -v /e/app:/webapp #挂载E:/app 文件夹 到 /webapp
 -v /D/docker-data/exchange/:/mnt #挂载D:/docker-data/exchange/ 文件夹 到 /mnt
 #特别注意：挂载路径和挂载目标之间只能有一个":"符号
```

2. #### 查看容器 Linux 系统版本

```bash
cat /etc/issue
```

docker search ubuntu 查看有哪些版本的 ubuntu 镜像

docker search ubuntu

3. #### 如何进入容器？
   首先使用下面的命令，查看容器 ID（CONTAINER ID）：

```bash
docker ps
```

然后用下面的命令进入容器，就可以使用 bash 命令浏览容器里的文件：

```bash
docker exec -it [CONTAINER ID] bash
```

4. #### 如何在容器与本地之间 copy 文件？
   从容器到本地：

```bash
docker cp <containerId>:/file/path/within/container /host/path/target
```

从本地到容器：

```bash
docker cp filename <containerId>:/file/path/within/container
```

以上的操作如果是文件夹，就拷贝的是整个文件夹内容。
