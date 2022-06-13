# python实时监控股票，并且持续扫描大盘

## 安装vim

```bash
sudo apt-get install vim
```

查看vim版本

```bash
vim --version
```

## 安装python

```bash
docker pull python
```

## 运行镜像

```bash
docker run -i -t python:latest /bin/bash
```

-t：进入终端
-i：获得一个交互式的连接，通过获取[container](https://so.csdn.net/so/search?q=container&spm=1001.2101.3001.7020)的输入
/bin/bash：在container中启动一个bash shell
退出方式：Ctrl-D

查看python版本

```bash
sudo docker container run --rm -it python:latest python -V
```

查看pip版本

```bash
sudo docker container run --rm -it python:latest pip -V
```



## 使用[docker](https://so.csdn.net/so/search?q=docker&spm=1001.2101.3001.7020)里的python执行本地脚本

```bash
# 需要关闭setenforce：
setenforce 0
# 通过将本地脚本挂载到镜像里，进行执行
sudo docker run  -v /home/yeqinfang/myWorkSpace/python:/yeqinfang  -w /yeqinfang python:latest  python helloworld.py
```

（1）docker run … python:latest 运行python镜像
（2）-v /home/yeqinfang/myWorkSpace/python:/yeqinfang 将服务器目录挂载到docker指定目录（不存在就先创建）
（3）-w /yeqinfang 挂载后，镜像里，脚本所在的目录
（4） python helloworld.py 执行脚本的指令



## 设置存储库

1. ### 更新`apt`包索引并安装包以允许`apt`通过 HTTPS 使用存储库：

   ```bash
   $ sudo apt-get update
   
   $ sudo apt-get install \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
   ```

2. ### 添加 Docker 的官方 GPG 密钥：

   ```
   $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

3. ### 使用以下命令设置**稳定**存储库。要添加 **nightly**或**test**存储库，请在以下命令中的单词之后添加单词`nightly`或`test`（或两者） 。[了解](https://docs.docker.com/engine/install/)[**夜间**](https://docs.docker.com/engine/install/)[和](https://docs.docker.com/engine/install/)[**测试**](https://docs.docker.com/engine/install/)[频道](https://docs.docker.com/engine/install/)。`stable`

   ```bash
   $ echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

## 安装 Docker 引擎

1. 更新`apt`包索引，安装*最新版本*的 Docker Engine、containerd 和 Docker Compose，或者进入下一步安装特定版本：

```bash
 $ sudo apt-get update
 $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

## 启动Docker

### 1. 开机自启Docker 

```bash
sudo systemctl enable docker
```

### 2. 启动Docker

```bash
sudo systemctl start docker.service
```

### 创建/etc/docker/daemon.json文件 , 配置镜像源

```bash
cat > /etc/docker/daemon.json <<-EOF
{
	"registry-mirrors":[
		"https://registry.docker-cn.com",
		"https://docker.mirrors.ustc.edu.cn"
	],
	"exec-opts":["native.cgroupdriver=systemd"],
	"log-driver":"json-file",
	"log-opts":{
		"max-size":"100m"
	},
	"storage-driver":"overlay2"
}
EOF
```

## 安装node-exporter

```bash
docker pull prom/node-exporter
```

### 拉取成功之后首先启动node-exporter

安装Prometheus和Grafana首先要安装node-exporter，该镜像相当于一个收集器！

因此搭建Prometheus、Grafana需要安装的镜像为：

```bash
sudo docker run -d -p 9100:9100 -v "/proc:/host/proc:ro" -v "/sys:/host/sys:ro" -v "/:/rootfs:ro" prom/node-exporter
```

然后通过sudo docker ps，查看是否启动成功！

启动成功访问：http://ip:9100/metrics


## 安装prometheus

```bash
docker pull prom/prometheus
```

### 1. 首先创建Prometheus的配置文件

```bash
sudo mkdir /opt/prometheus
cd /opt/prometheus/
sudo vim prometheus.yml
```

### 2. 创建之后文件中写入Prometheus的相关配置

```bash
global:
  scrape_interval:     60s
  evaluation_interval: 60s

scrape_configs:

  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']
        labels:
          instance: prometheus

  - job_name: linux
    static_configs:
      - targets: ['本机ip:9100']
        labels:
          instance: localhost

```

启动Prometheus

```bash
sudo docker run -d -p 9090:9090 -v /opt/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml --name prometheus prom/prometheus
```

启动成功之后通过sudo docker ps查看状态

然后浏览器访问：http://ip:9090/graph和http://ip:9090/targets

## 安装Grafana

```bash
sudo docker pull grafana/grafana
```

Prometheus安装就绪之后，需要安装Grafana展示监控数据UI，通过Grafana来实现。

### 创建文件夹

```bash
sudo mkdir /opt/grafana-storage
```

### 修改文件夹权限

```bash
sudo chmod 777 -R /opt/grafana-storage
```

### 启动grafana

```bash
sudo docker run -d -p 3000:3000 --name=grafana -v /opt/grafana-storage/:/var/lib/grafana grafana/grafana
```

启动成功之后访问：http://ip:3000

Grafana的默认帐号密码都是admin，登录之后需要设置新密码！