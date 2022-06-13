# docker搭建samba共享目录

## 首先在服务器中查找docker版的samba

```bash
sudo docker search samba
```

## 拉取 samba 镜像

```bash
sudo docker pull dperson/samba
```

## 在本地创建个目录，以便于容器挂载

```bash
mkdir /home/shared            //在/home目录下创建shared目录
chmod 777 /home/shared		 //修改shared权限，不修改的话连接进去会提示没有权限写入数据
```

## 启动镜像

```bash
docker run -it --name samba -p 139:139 -p 445:445 -v /home/shared:/mount -d dperson/samba -u "htl;485374" -s "shared;/mount;yes;no;no;all;none"
```

"shared;/mount;yes;no;no;all;none" 参数说明：
分别是：
shared：共享文件夹的名称（shared）；
/mount：共享在samba容器中的路径（/mount）；
yes：共享名称对所有工作组用户可见；
no：不是只读（也就是说可写）；
no：不允许guest用户；
all：指定共享的所有权用户；
none：指定共享的超级用户；
指定具有写权限的用户；