### 安装[postgresSQL](https://hub.docker.com/_/postgres)

安装最新版本：

> docker pull postgres

安装指定版本：

> docker pull postgres:9.6

### 启动镜像

```shell
docker run --name postgres -e POSTGRES_PASSWORD=123456 -p 5432:5432 -d postgres:9.6
```

> 解释：  
> run，创建并运行一个容器；
> --name，指定创建的容器的名字；  
> -e POSTGRES_PASSWORD=password，设置环境变量，指定数据库的登录口令为 password；  
> -p 54321:5432，端口映射将容器的 5432 端口映射到外部机器的 54321 端口；  
> -d postgres:9.4，指定使用 postgres:9.4 作为镜像。

> 注意：  
> postgres 镜像默认的用户名为 postgres，  
> 登录口令为创建容器是指定的值。

查看端口使用情况：

```shell
netstat -ano
netstat -aon|findstr "5432"
```

我们将使用“docker run”命令，用“postgres”官方镜像启动一个新容器，名称为“postgres1”，并公开端口 5432（PostgreSQL 的默认值）。我们正在运行分离（-d）模式（在后台）。

我们同样用“-v”挂载一个 volume，它将用于存储我们创建的数据库。这个 volume 的名称是“postgres-data”，如果不存在具有此名称的 volume，Docker 将自动创建它（仅使用 Docker 主机上本地磁盘的存储）。

PostgreSQL 将其数据存储在“/var/lib/postgresql/data”中，因此我们将 volume 挂载到该路径。

docker run -d -p 5432:5432 -v postgres-data:/var/lib/postgresql/data
--name postgres1 postgres

一旦我们完成了这一步，我们就可以开始检查它的运行情况了：

```Shell
docker ps
```

查看日志输出：

```Shell
docker logs postgres
```

> My:

```Shell
docker run --name postgres -e POSTGRES_PASSWORD=123456 -p 5432:5432 -v /c/ShareData/Docker/postgreSQL:/var/lib/postgresql/data -d postgres
```

### 安装 Vim(如果无需修改 PostgresSQl 配置 可跳过)

> 进入容器:  
> docker exec -it [CONTAINER ID] bash

1. **更新软件包**

```bash
apt-get update
```

2. **安装 vim**

```bash
apt-get install vim
```

### 修改 PostgresSQl 配置

默认情况下，PostgresSQl 只允许本地访问，需要需要修改配置文件

如果采用默认 postgres 镜像创建，那么配置文件目录在 **/var/lib/postgresql/data** 中
如果不在则需要搜索，或者在传统/etc/postgresql/
find . -name "\*.xxx"

1. **修改 PostgreSQL 数据库配置**
   > [参考：vim 怎么保存退出](https://jingyan.baidu.com/article/af9f5a2d5bc2b843150a456a.html)

修改 postgresql.conf 文件

```bash
vim /etc/postgresql/9.5/main/postgresql.conf
```

> 将#listen_address='localhost'改为 listen_address='\*'，监听任何地址访问；  
> 将#password_encryption=on 改为 password_encryption=on 启用密码验证。

修改 pg_hba.conf 文件

```bash
vim /etc/postgresql/9.5/main/pg_hba.conf
```

> 添加 host all all 0.0.0.0/0 md5。

like:
||||||
|------|--------|------------------|------------------------|-----------|
|host | all | all | 127.0.0.1/32 | md5 |
|host | all | all | 0.0.0.0/0 | md5 |

2. **重启数据库服务**

```bash
etc/init.d/postgresql restart
```

2. **修改防火墙(Windows)**

在修改配置文件后，还需要修改防火墙设置。有 2 中方式：

a)关闭防火墙（不建议）

b)设置防火墙规则

> 在控制面板中搜索防火墙，打开 windows defender 防火墙，找到“高级设置”打开。  
> 在“入站规则”中新建一条规则，规则类型选择“端口”，特定本地端口端口填写安装时设置的端口号，如默认的“5432”，其他默认即可。先新建一条 TCP 的规则，再新建一条 UDP 的规则。

设置好防火墙后，局域网内即可访问 postgresql 数据库了。
