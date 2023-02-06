# **dockerMongo**

### 拉取最新mongoDB镜像

```bash
docker pull mongo:latest
```

### 运行mongoDB

```bash
docker run -p 27017:27017 --name mongo mongo
```

- **-p 27017:27017** ：映射容器服务的 27017 端口到宿主机的 27017 端口。外部可以直接通过 宿主机 ip:27017 访问到 mongo 的服务。
- **--auth**：需要密码才能访问容器服务。1
