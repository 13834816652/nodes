## 查询nginx镜像

```bash
sudo docker search nginx
```

## 拉取镜像

```bash
sudo docker pull nginx
```

## 运行一个nginx的镜像的实例

```bash
sudo docker run -d --name nginx -p 80:80 nginx:latest
```

-d 指定容器以守护进程方式在后台运行
–name 指定容器名称，此处我指定的是nginx
-p 指定主机与容器内部的端口号映射关系，格式 -p [宿主机端口号]：[容器内部端口]，此处我使用了主机80端口，映射容器80端口

## 交互形式进入创建的nginx容器

```bash
sudo docker exec -it nginx /bin/bash
```

在容器里，找到nginx的默认的配置文件，/etc/nginx/nginx.conf

容器的nginx的配置文件，都是默认在容器里的/etc/nginx文件夹下，查看下nginx.conf内容，容器里，不支持vim操作；

## 将nginx容器内部配置文件挂载到主机

将nginx容器内部配置文件挂载到主机，之后就可以在主机对应目录修改即可。适合频繁修改，复杂使用的情况

### 在主机/mnt目录下执行 mkdir -p ./nginx/{html,logs,conf}创建挂载目录

### 将容器中的相应文件copy到刚创建的管理目录中

```bash
sudo docker cp dbc:/etc/nginx/nginx.conf /home/ubuntu/nginx/
sudo docker cp dbc:/etc/nginx/conf.d /home/ubuntu/nginx/conf
sudo docker cp dbc:/usr/share/nginx/html/ /home/ubuntu/nginx/html
sudo docker cp dbc:/var/log/nginx/ /home/ubuntu/nginx/logs
```

### 停止并移除容器

```bash
sudo docker stop dbc 
sudo docker rm dbc 
```

### 再次启动容器并作目录挂载

```bash
sudo docker run  --name nginx -m 200m -p 80:80 \
-v /home/ubuntu/nginx/nginx.conf:/etc/nginx/nginx.conf \
-v /home/ubuntu/nginx/logs:/var/log/nginx \
-v /home/ubuntu/nginx/html:/usr/share/nginx/html \
-v /home/ubuntu/nginx/conf:/etc/nginx/conf.d \
-e TZ=Asia/Shanghai \
--privileged=true -d nginx:latest
```

参数说明
-name  给你启动的容器起个名字，以后可以使用这个名字启动或者停止容器
-p 　　  映射端口，将docker宿主机的80端口和容器的80端口进行绑定
-v 　　　挂载文件用的，
-m 200m 分配内存空间
-e TZ=Asia/Shanghai  设置时区
第一个-v 表示将你本地的nginx.conf覆盖你要起启动的容器的nginx.conf文件，
第二个-v 表示将日志文件进行挂载，就是把nginx服务器的日志写到你docker宿主机的/home/docker-nginx/log/下面
第三个-v 表示的和第一个-v意思一样的。
-d 表示启动的是哪个镜像
