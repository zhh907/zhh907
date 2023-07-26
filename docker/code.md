
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

# 强制删除镜像
docker rm -f image_name
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



# mysql：5.7 安装启动
```
# 拉取镜像
docker pull mysql:5.7
 
# 创建挂载文件路径
# 宿主机创建数据存放目录映射到容器
mkdir -p /usr/local/docker_data/mysql/data
# 宿主机创建配置文件目录映射到容器 
mkdir -p /usr/local/docker_data/mysql/conf #(需要在此目录下创建"conf.d"、"mysql.conf.d"两个目录)
mkdir -p /usr/local/docker_data/mysql/conf/conf.d # (建议在此目录创建my.cnf文件并进行相关MySQL配置)
mkdir -p /usr/local/docker_data/mysql/conf/mysql.conf.d
# 宿主机创建日志目录映射到容器
mkdir -p /usr/local/docker_data/mysql/logs

# 启动镜像
# --privileged=true参数，让容器拥有真正的root权限
docker run --privileged=true --name mysql5.7 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d  -v /usr/local/docker_data/mysql/data:/var/lib/mysql -v /usr/local/docker_data/mysql/conf:/etc/mysql/ -v /usr/local/docker_data/mysql/logs:/var/log/mysql mysql:5.7

# 进入容器
docker exec -it mysql5.7 bash

```