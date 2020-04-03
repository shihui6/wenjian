# Docker

## docker 安装 

[docker安装文档](https://docs.docker.com/)

[centos系统参考](https://docs.docker.com/install/linux/docker-ce/centos/) 

## 常用命令



```bash
# 设置docker服务开机启动 
systemctl enable docker

# 启动服务
service docker start

# 查看本地镜像
docker images

# 查看所有容器
docker ps -a

```

### 问题

错误:but none of the providers can be installed

解决办法
[参考](https://www.linuxidc.com/Linux/2019-10/160948.htm)

安装新版的containerd.io软件包

　　containerd.io软件包下载地址：https://download.docker.com/linux/centos/7/x86_64/edge/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm

```bash

//下载相关软件包
[root@localhost ~]# wget https://download.docker.com/linux/centos/7/x86_64/edge/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm

//升级containerd.io软件包
[root@localhost ~]# yum -y install containerd.io-1.2.6-3.3.el7.x86_64.rpm

 ```