# **DockerMysql5.7**

### 拉取mysql5.7镜像

```bash
docker pull mysql:5.7
```

### 启动容器

```bash
docker run -d --name db -v/home/mysql/data:/var/lib/mysql  -v /home/mysql/conf:/etc/mysql/config.d -e MYSQL_ROOT_PASSWORD=485374 -p 3306:3306 mysql:5.7 --lower_case_table_names=1
```

##### -d：后台启动

–name：容器的名字
-v： 挂载本地的文件夹
-e：设置root的密码
-p：端口映射
–lower_case_table_names=1：设置不区分大小写

```bash
docker run -d -p 3306:3306 --privileged=true -v /docker/mysql/conf/my.cnf:/etc/my.cnf -v /docker/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=48 --name mysql mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_general_ci
```

参数说明:
run 运行一个容器

-d 后台运行

-p 3306:3306 容器内部端口和服务器端口映射关联

--privileged=true 设置mysql用户，否则外部不能使用root用户登录

-v /docker/mysql/conf/my.cnf:/etc/my.cnf 服务器的/docker/mysql/conf/my.cnf配置映射到docker的my.cnf

-v /docker/mysql/data:/var/lib/mysql 映射数据库的数据目录，避免docker删除重新运行mysql容器，导致数据丢失

-e MYSQL_ROOT_PASSWORD=123456 设置root账号的密码

--name mysql mysql:5.7 从docker镜像mysql:5.7启动一个容器，并设置容器的名称为mysql

--character-set-server=utf8mb4 --collation-server=utf8mb4_general_ci 设置数据库默认编码