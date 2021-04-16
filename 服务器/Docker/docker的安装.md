# docker的安装

**Centos安装Docker**

官方一键式安装命令如下

```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

安装完成之后启动`docker`

```shell
service docker start 
systemctl start docker
```

查看`docker`的运行状态

```shell
service docker status
systemctl status docker
```