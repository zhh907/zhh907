
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

# 安装工具，设计docker仓库源
 yum install -y yum-utils  device-mapper-persistent-data  lvm2
 yum-config-manager  --add-repo  https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

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

# redis 安装启动
```
#搜索镜像
docker search redis

# 拉取镜像
docker pull redis

# 创建挂载文件路径和配置文件
mkdir -p /usr/local/docker_data/redis/data
mkdir -p /usr/local/docker_data/redis/conf

touch /usr/local/docker_data/redis/conf/redis.conf

# 启动命令
docker run --name redis -p 6379:6379 -v /usr/local/docker_data/redis/data:/data -v /usr/local/docker_data/redis/conf/redis.conf:/etc/redis/redis.conf -d redis redis-server /etc/redis/redis.conf 
```

# nginx 安装启动
```
# 创建挂载文件路径和配置文件
mkdir -p /usr/local/docker_data/nginx/conf
mkdir -p /usr/local/docker_data/nginx/html
mkdir -p /usr/local/docker_data/nginx/log

# 生成容器
docker run --name nginx -p 9001:80 -d nginx
# 将容器nginx.conf文件复制到宿主机
docker cp nginx:/etc/nginx/nginx.conf /usr/local/docker_data/nginx/conf/nginx.conf
# 将容器conf.d文件夹下内容复制到宿主机
docker cp nginx:/etc/nginx/conf.d /usr/local/docker_data/nginx/conf/conf.d
# 将容器中的html文件夹复制到宿主机
docker cp nginx:/usr/share/nginx/html /usr/local/docker_data/nginx/html

# 停止容器、删除容器
docker stop nginx
docker rm nginx

# 删除正在运行的nginx容器
docker rm -f nginx

#创建Nginx容器
docker run -p 9002:80 --name nginx -v /usr/local/docker_data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /usr/local/docker_data/nginx/conf/conf.d:/etc/nginx/conf.d -v /usr/local/docker_data/nginx/log:/var/log/nginx -v /usr/local/docker_data/nginx/html:/usr/share/nginx/html -d nginx:latest

```