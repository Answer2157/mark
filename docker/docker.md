# **Docker**

## 一、 初识docker

当我们利用Docker安装应用时，Docker会自动搜索并下载应用**镜像(image)**。

镜像不仅包含应用本身，还包含应用运行所需要的环境、配置、系统函数库。

Docker:会在运行镜像时创建一个隔离环境，称为**容器**（container)。
**镜像仓库**：存储和管理镜像的平台，Docker官方维护了一个公共仓库：Docker Hub。

![image-20241029183550970](./docker/image-20241029183550970.png)

配置镜像加速器的命令 

目前测试到的只有下面这一个镜像源可用：https://docker.m.daocloud.io

~~~bash
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://docker.m.daocloud.io"] 
}
EOF

sudo systemctl daemon-reload

sudo restart docker
~~~

安装MySQL示例：

安装root用户，密码123，任意源3306端口访问本机的3306端口的MySQL服务

~~~bash
docker run -d --name mysql -p 3306:3306 -e TZ=Asia/Shanghai -e MYSQL_ROOT_PASSWORD=123 mysql
~~~

查看安装的镜像

~~~bash
docker images
~~~

查看启动的镜像服务

~~~bash
docker ps
~~~

## 二、 安装docker

在 Ubuntu 系统上安装 Docker 的步骤如下：

### 1. 更新现有的软件包
```bash
sudo apt-get update
```

### 2. 安装必要的软件包
```bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

### 3. 添加 Docker 的 GPG 密钥
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

### 4. 设置 Docker 仓库
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 5. 更新软件包列表
```bash
sudo apt-get update
```

### 6. 安装 Docker CE（Community Edition）
```bash
sudo apt-get install docker-ce
```

### 7. 检查 Docker 服务状态
确保 Docker 服务已经启动并运行：
```bash
sudo systemctl status docker
```

### 8. 运行一个测试容器
```bash
sudo docker run hello-world
```

### 9. （可选）将当前用户加入 `docker` 组
为了在不使用 `sudo` 的情况下运行 Docker 命令，可以将当前用户添加到 `docker` 组：
```bash
sudo usermod -aG docker $USER
```
然后，退出并重新登录，或使用以下命令应用组更改：
```bash
newgrp docker
```

完成以上步骤后，则能够在 Ubuntu 上成功安装并使用 Docker。

### 10. 版本检查

~~~bash
docker --version
~~~

## 三、命令解读

对于刚才得安装MySQL的指令可能不太理解，所以进行解释一下。

~~~bash
docker run -d \
--name mysql \
-p 3306:3306 \
-e TZ=Asia/Shanghai \
-e MYSQL_ROOT_PASSWORD=123 \
mysql
~~~

- **docker  run** ：创建并运行一个容器，**-d**是让容器在后台运行
- **--name mysql** ：给容器起个名字，必须唯一
- **-p 3306:3306** ：设置端口映射 （0.0.0.0的3306端口可以让问本机的3306端口服务，第二个端口取决于进程的端口，mysql的端口为3306）
- **-e KEY= VALUE**：是设置环境变量
- **mysql**：指定运行的镜像的名字

### 镜像命名规范

- 镜像名称一般分为两部分组成：[repository]:[tag]
  - 其中repository就是镜像名
  - tag是镜像的版本
- 没有指定tag时，默认是latest，代表最新版本的镜像

![image-20241101202130809](./docker/image-20241101202130809.png)

~~~
简单小结一下:										nagin为例
-d: 让容器在后台运行							   docker run -d \
--name: 给容器起名字								--name nginx \
-e: 环境变量
-p: 宿主机端口映射到容器内端口					   -p 80:80
镜像名称结构:						
Repository:TAG ------> 镜像名:版本号				nginx
~~~

## 四、Docker基础

### （一）常见命令

Docker:最常见的命令就是操作镜像、容器的命令，详见官方文档：https://hub.docker.com/

![image-20241101204138292](./docker/image-20241101204138292.png)

下述均已容器名为nginx的进程为例

~~~bash
查看容器更简洁一点：
docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"
~~~

docker ps ：仅查看运行中的进程，若需查看所以需要加上 -a

~~~bash
查看日志：
docker logs nginx ---------->查看nginx的日志
docker logs -f nginx ------->实时跟踪nginx日志
~~~

~~~bash
删除容器：
docker rm nginx ------>删除前需要先停止容器
docker rm nginx -f --->强制删除容器
~~~

#### 命令别名

进入编辑面板

~~~bash
# 修改/root/.bashrc文件
vi ~/.bashrc   
~~~

加入内容

~~~bash
alias dps='docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"'

alias dis='docker images'
~~~

执行命令使别名生效

~~~bash
source ~/.bashrc
~~~

### （二）数据卷挂载

#### 数据卷

**数据卷(volume)**是一个虚拟目录，是**容器内目录**与**宿主机目录**之间映射的桥梁。

![image-20241102124246833](./docker/image-20241102124246833.png)

| 命令                  | 说明                 | 文档地址 |
| --------------------- | -------------------- | -------- |
| docker volume create  | 创建数据卷           |          |
| docker volume ls      | 查看所有数据卷       |          |
| docker volume rm      | 删除指定数据卷       |          |
| docker volume inspect | 查看某个数据卷的详情 |          |
| docker volume prune   | 清楚数据卷           |          |

#### 1)案例1-利用Nginx容器部署静态资源

需求：

- 创建Nginx容器，修改nginx容器内的html目录下的index.html文件，查看变化
- 将静态资源部署到nginxl的html目录

提示：

- 在执行docker run命令时，使用-**v数据卷：容器内目录**可以完成数据卷挂载
- 当创建容器时，如果挂载了数据卷且数据卷不存在，会自动创建数据卷

以nginx为例子

~~~bash
# 挂载操作
docker run -d --name nginx -p 80:80 -v html:/usr/share/nginx/html nginx
# 查看挂载的所有数据卷
docker volume ls
# 查看创建的数据卷的位置
docker volume inspect html # html 为查看的数据卷的名称
# 如果要修改其中的内容只需要移动到宿主机的数据卷的位置找到对应文件锦绣修改就可以了

# 查看容器内的文件情况
# 进入容器
docker exec -it nginx bash
# 切换目录 
cd /usr/share/nginx/html/
# 查看
ls
~~~

#### 2)案例2-mysql容器的数据挂载

需求：

- 查看mysql容器，判断是否有数据卷挂载
- 基于宿主机目录实现MySQL数据目录、配置文件、初始化脚本的挂载（查阅官方镜像文档)
