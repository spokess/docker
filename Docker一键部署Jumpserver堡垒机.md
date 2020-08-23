# Docker 一键部署Jumpserver堡垒机

Docker 快速部署

- 使用 root 身份输入
- 环境迁移和更新升级请检查 SECRET_KEY 是否与之前设置一致, 不能随机生成, 否则数据库所有加密的字段均无法解密

## 一、生成随机加密秘钥

Linux 生成随机加密秘钥, 可以用下面的命令：

```
if [ ! "$SECRET_KEY" ]; then
  SECRET_KEY=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 50`;
  echo "SECRET_KEY=$SECRET_KEY" >> ~/.bashrc;
  echo $SECRET_KEY;
else
  echo $SECRET_KEY;
fi  
if [ ! "$BOOTSTRAP_TOKEN" ]; then
  BOOTSTRAP_TOKEN=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 16`;
  echo "BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN" >> ~/.bashrc;
  echo $BOOTSTRAP_TOKEN;
else
  echo $BOOTSTRAP_TOKEN;
fi
```

## 二、安装并启动docker

## 三、一键部署

```
docker run --name jms_all -d \
  -p 80:80 -p 2222:2222 \
  -e SECRET_KEY=$SECRET_KEY \
  -e BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN \
  -v /opt/jumpserver/data:/opt/jumpserver/data \
  -v /opt/jumpserver/mysql:/var/lib/mysql \
  jumpserver/jms_all:v2.1.2
```

## 四、访问

浏览器访问: http://<容器所在服务器IP>

- SSH 访问: ssh -p 2222 <容器所在服务器IP>
- XShell 等工具请添加 connection 连接, 默认 ssh 端口 2222
- 默认管理员账户 admin 密码 admin

官方文档：
https://docs.jumpserver.org/zh/master/install/docker_install/



<hr>

相关命令：

查看运行的容器：docker ps -a

停止运行的容器：docker stop ee8555134f83

删除停止容器(删除前必须先停止)：docker rm 82ac8a923fa7

