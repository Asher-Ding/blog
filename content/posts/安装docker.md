---
title: "如何安装docker"
date: 2025-03-31
draft: false
tags: ["dify"]
---

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
