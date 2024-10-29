# **Docker**

## 一、 初识docker

当我们利用Docker安装应用时，Docker会自动搜索并下载应用**镜像(image)**。

镜像不仅包含应用本身，还包含应用运行所需要的环境、配置、系统函数库。

Docker:会在运行镜像时创建一个隔离环境，称为**容器**（container)。
**镜像仓库**：存储和管理镜像的平台，Docker官方维护了一个公共仓库：Docker Hub。

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



