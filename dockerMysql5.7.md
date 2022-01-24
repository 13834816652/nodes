# **DockerMysql5.7**

### 拉取mysql5.7镜像

```bash
docker pull mysql:5.7
```

### 启动容器

```bash
docker run -d --name db -v/home/mysql/data:/var/lib/mysql  -v /home/mysql/conf:/etc/mysql/config.d -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql:5.7 --lower_case_table_names=1
```

##### -d：后台启动

–name：容器的名字
-v： 挂载本地的文件夹
-e：设置root的密码
-p：端口映射
–lower_case_table_names=1：设置不区分大小写