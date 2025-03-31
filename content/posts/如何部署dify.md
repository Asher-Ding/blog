# Demo 版 Dify 安装指南

## 环境

### 服务器

- CPU: Intel(R) Core(TM) i5-4460  CPU @ 3.20GHz
- RAM: 7.6 GiB
- 硬盘: 1T
- 操作系统: Ubuntu 22.04.4 LTS

## 开始

### 安装git
安装git是为了下载Dify的源码，并便于后续的更新。
```bash
sudo apt install -y git
```

### 安装docker
安装docker是为了运行Dify，管理Dify运行。
```bash
sudo apt install -y docker.io
```

### 安装docker-compose
安装docker-compose是为了管理Dify的运行。
```bash
sudo apt install -y docker-compose
```
### 拉取Dify源码

```bash
cd /opt
# 初始化git仓库
git init dify-docker
cd dify-docker
git remote add origin https://github.com/langgenius/dify.git
# 启用 sparse-checkout
git sparse-checkout init --cone
# 设置只拉取docker内容
git sparse-checkout add docker
# 拉取docker内容，并输出日志
git pull origin main -v
```

### 安装并启动Dify

```bash
ls docker
# 进入docker目录
cd docker
# 复制配置文件
cp .env.example .env
# 修改配置文件（自行修改）（可以不配置）
vi .env
# 使用docker-compose启动dify（后台运行）
docker-compose up -d
```

### 查看dify日志

```bash
# 查看dify日志
docker-compose logs -f
# 停止dify
docker-compose down
# 查看docker容器
docker ps
# 查看docker容器日志
docker logs -f dify
```

### docker设置dify开机启动

```bash
sudo systemctl enable docker
``` 

###  使用

### git的基本使用方法
```bash
# 拉取最新代码
git pull
# 查看git状态
git status
# 查看git日志
git log
# 查看git版本
git --version
```

```bash
# 查看docker系统信息
docker system info
```

### docker-compose的基本使用方法
> 有些版本的docker-compose 可能需要使用 `docker compose` 命令来启动。
```bash
# 启动docker-compose, 并后台运行
docker-compose up -d
# 停止docker-compose
docker-compose down
# 重启docker-compose
docker-compose restart
# 查看docker-compose日志
docker-compose logs
```


## 解决方法

### 1. git拉取失败

如果在拉取过程中，出现以下错误：

```bash
error: RPC failed; curl 16 Error in the HTTP2 framing layer
fatal: expected flush after ref listing
```
可按照以下步骤操作
```bash
# 测试git连接
git ls-remote origin
# 禁用HTTP/2
git config --global http.version HTTP/1.1
# 设置git缓冲区大小
git config --global http.postBuffer 52428800
git config --global https.postBuffer 52428800
git pull origin main -v
```

### 2. docker-compose启动失败

如果在启动过程中，出现以下错误：

```bash
ERROR: Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
```
可按照以下步骤操作
```bash
ping registry-1.docker.io 
# 如果ping不通，可按照以下步骤操作
# 1. 获取可以ping通的镜像源
ping ustc.edu.cn 
# 配置docker镜像
sudo vi /etc/docker/daemon.json
# 添加以下内容（vi编辑器，默认会使用vi编辑器，按i键进入编辑模式，按esc键退出编辑模式，按:wq键保存并退出）
{
  "registry-mirrors": ["https://docker.imgdb.de","https://docker.hlmirror.com"]
}
# 特别提醒： 以前很多常用的镜像源，现在都失效了，需要更换。
# 查看docker镜像
docker info | grep "Registry Mirrors"

docker info | awk '/Registry Mirrors/{getline; print}'
# 重启docker
sudo systemctl daemon-reload
sudo systemctl restart docker
# 测试docker镜像
docker pull ustc.edu.cn/library/hello-world
# 开放443端口
sudo ufw allow 443
```

### 3. Dify 配置问题

出现以下警告，可以忽略。可以自行配置参数。

```bash
WARNING: The CERTBOT_EMAIL variable is not set. Defaulting to a blank string.
WARNING: The CERTBOT_DOMAIN variable is not set. Defaulting to a blank string.
```

### 4. 公网访问无法加载web页面

公司内部网络配置问题。

```bash
# 测试网络连接
nc -zv 36.154.50.213 443
nc -zv 36.154.50.213 80

sudo lsof -i :80  # HTTP
sudo lsof -i :443 # HTTPS

docker exec -it <dify容器id> curl -I baidu.com

# 容器内访问 localhost
docker exec -it 94245ab4b012 curl -I 127.0.0.1:80
# 宿主机访问
curl -I http://localhost:80

# 查看防火墙规则
sudo iptables -L -n -v --line-numbers | grep '80\|443'
sudo ufw status | grep -E '80|443'

# 外部测试
curl -v -k http://36.154.50.213

# 
docker network ls && docker network inspect bridge

# 服务器的网络接口配置
ip addr show
# 检查 iptables 的 FORWARD 链规则
sudo iptables -P FORWARD ACCEPT
# 检查公网ip
ip route show | grep 36.154.50
# default via 36.154.50.209 dev enp3s0 
# 36.154.50.208/29 dev enp3s0 proto kernel scope link src 36.154.50.213 

# 检查时间
date && timedatectl status
timedatectl list-timezones | grep Asia/Shanghai
sudo timedatectl set-timezone Asia/Shanghai && sudo systemctl restart systemd-timesyncd && timedatectl status

```



