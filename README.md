# OlivOS-Docker

OlivOS-Docker，用于在linux上使用docker-compose快速组装OlivOS

[Docker Hub](https://hub.docker.com/r/shiaworkshop/olivos)

## 使用方法

### 准备工作

#### 确保已安装 Docker 和 Docker Compose
- Docker 安装​​：参考 [官方文档](https://docs.docker.com/engine/install/)

- ​​Docker Compose 安装​​：通常随 Docker Desktop 自动安装，独立安装可参考 [官方指南](https://docs.docker.com/compose/install/)

#### 创建数据目录

  创建存储 OlivOS 和 napcat 数据的本地目录，用于持久化数据。

  我们以默认位置为例：

  ```
  mkdir -p -m 755 /opt/OlivOS-Docker
  cd /opt/OlivOS-Docker
  ```

#### 配置环境变量

  下载 `docker-compose.yml` ，在同一级创建 `.env` 文件。
  
  ```
  wget https://raw.githubusercontent.com/ShiaBox/OlivOS-Docker/refs/heads/main/docker-compose.yml
  echo 'ACCOUNT=123456' > .env
  ```

  在 `.env` 内，变量 `LOGIN_UIN` 是键入骰娘账号，所以要替换`123456`为你实际的骰娘账号。

### 运行服务
1. 启动所有服务
```
docker-compose up -d
```
2. 查看容器状态
```
docker-compose ps
```
3. 停止服务
```
docker-compose down
```
4. 更新服务
```
# 拉取最新镜像
docker-compose pull
# 重新创建容器
docker-compose up -d --force-recreate
```

（可选）启动时指定项目名称中包含UIN

```
export $(grep -v '^#' .env | xargs) && docker-compose -p "olivos-${ACCOUNT}" up -d
```

这样启动出来的容器名称会变成下面这样，更方便管理

```
olivos-123456_olivos-main_1
olivos-123456_napcat_1
```


### 登录

容器日志能看到二维码，同时也推荐你使用NapCat的webUI去扫码登录，比如 `IP:6099` 或者你启动容器时映射的其他端口号。

