# Centos安装RabbitMQ

**一、安装Erlang环境->yum方式安装**

1、安装依赖项

```shell
yum install -y epel-release
```

2、添加存储库条目

```shell
wget https://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
rpm -Uvh erlang-solutions-1.0-1.noarch.rpm
```

![](E:\Notes\框架\RabbitMQ\image\下载erlang语言.PNG)

![](E:\Notes\框架\RabbitMQ\image\2.PNG)

3、安装

```shell
yum install -y erlang
```

![](E:\Notes\框架\RabbitMQ\image\3.PNG)

4、验证是否安装成功

```shell
erl -version
```

![](E:\Notes\框架\RabbitMQ\image\4.PNG)

**二、安装Erlang环境->rpm方式安装**

1、安装依赖项

```shell
yun install -y epel-release
```

2、下载rpm包

```shell
wget url
```

3、安装

```shell
yum install 'package'
```

4、验证

```shell
erl -version
```

