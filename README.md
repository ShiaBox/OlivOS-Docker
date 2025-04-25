# OlivOS-Docker

OlivOS-Docker，用于在linux上使用docker-compose快速组装OlivOS

## 使用方法

### 准备工作

#### 确保已安装 Docker 和 Docker Compose
- Docker 安装​​：参考 [官方文档](https://docs.docker.com/engine/install/)

- ​​Docker Compose 安装​​：通常随 Docker Desktop 自动安装，独立安装可参考 [官方指南](https://docs.docker.com/compose/install/)

#### 创建数据目录

  创建存储 OlivOS 和 go-cqhttp 数据的本地目录：

  我们以默认位置为例：

  ```
  mkdir -p /opt/OlivOS-Docker
  ```

#### 配置环境变量

下载 `docker-compose.yml` ，把它放到服务器的一个位置上，在同一级创建 `.env` 文件。

在 `.env` 内，键入骰娘账号变量 `LOGIN_UIN` 和密码变量 `LOGIN_PASSWD`，密码变量可以省略，会跳到手表登录，但是手表协议不稳定，自行决定。

变量 `OLIVOS_DATA` 是刚才创建的数据目录。

.env 文件示例：
  ```
  # 示例 .env 文件内容
  LOGIN_UIN=123456789       # 骰娘 QQ 账号
  LOGIN_PASSWD=your_password # 骰娘 QQ 密码
  OLIVOS_DATA=/opt/OlivOS-Docker # 持久化数据目录路径
  ```
### 运行服务
1. 启动所有服务
```
docker-compose up -d
```
`-d` 表示后台运行
首次运行会自动拉取镜像并创建容器

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
