
# 其他操作
```
# 查看内核
uname -a
cat /proc/version
# 一键启动所有docker 容器：
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)

# 一键关闭所有docker 容器：
docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)

# 一键删除所有docker 容器：
docker rm $(docker ps -a | awk '{ print $1}' | tail -n +2)

# 一键删除所有docker 镜像: 
docker rmi $(docker images | awk '{print $3}' |tail -n +2)
```

# 基于Centos7安装docker
```
# 卸载
yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine  
#确保删除干净
sudo yum remove docker*
#删除目录
rm -rf /var/lib/docker/

# 安装
yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin


# 配置镜像源
echo '{
  "registry-mirrors":[
    "https://registry.docker-cn.com",
    "https://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
    ],
  "dns":[
    "8.8.8.8",
    "114.114.114.114"
    ]
}' > /etc/docker/daemon.json
#加载配置
systemctl daemon-reload
#重启配置生效
systemctl restart docker



# 更新
yum update
# 安装docker engine
yum install -y docker-engine

```