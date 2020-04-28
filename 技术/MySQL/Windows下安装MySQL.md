# Windows下MySQL的安装
首先下载MySQL的的压缩包（MySQL社区版）
![16583476-5e49ba12ab1c3a2a](http://www.ajiehome.cn/wp-content/uploads/2020/04/16583476-5e49ba12ab1c3a2a-2.png)
---
![16583476-5e49ba12ab1c3a2a](http://www.ajiehome.cn/wp-content/uploads/2020/04/16583476-5e49ba12ab1c3a2a-3.png)
---
![16583476-5e49ba12ab1c3a2a](http://www.ajiehome.cn/wp-content/uploads/2020/04/16583476-5e49ba12ab1c3a2a-4.png)
---
## 安装
如果以前安装过其它版本的MySQL那么需要先卸载mysql的服务在进行安装
卸载方式：以管理员身份进入CMD控制台，然后输入一下命令进行删除服务
如果未安装则可以忽略这一步骤：mysql是服务的名称
```
sc delete mysql
```
**SC的用法**

``` mysql

        SC 是用来与服务控制管理器和服务进行通信
        的命令行程序。
        用法:
        sc <server> [command] [service name] <option1> <option2>...


        <server> 选项的格式为 "\\ServerName"
        可通过键入以下命令获取有关命令的更多帮助: "sc [command]"
        命令:
          query-----------查询服务的状态，
                          或枚举服务类型的状态。
          queryex---------查询服务的扩展状态，
                          或枚举服务类型的状态。
          start-----------启动服务。
          pause-----------向服务发送 PAUSE 控制请求。
          interrogate-----向服务发送 INTERROGATE 控制请求。
          continue--------向服务发送 CONTINUE 控制请求。
          stop------------向服务发送 STOP 请求。
          config----------更改服务的配置(永久)。
          description-----更改服务的描述。
          failure---------更改失败时服务执行的操作。
          failureflag-----更改服务的失败操作标志。
          sidtype---------更改服务的服务 SID 类型。
          privs-----------更改服务的所需特权。
          managedaccount--更改服务以将服务帐户密码
                          标记为由 LSA 管理。
          qc--------------查询服务的配置信息。
          qdescription----查询服务的描述。
          qfailure--------查询失败时服务执行的操作。
          qfailureflag----查询服务的失败操作标志。
          qsidtype--------查询服务的服务 SID 类型。
          qprivs----------查询服务的所需特权。
          qtriggerinfo----查询服务的触发器参数。
          qpreferrednode--查询服务的首选 NUMA 节点。
          qmanagedaccount-查询服务是否将帐户
                          与 LSA 管理的密码结合使用。
          qprotection-----查询服务的进程保护级别。
          quserservice----查询用户服务模板的本地实例。
          delete ----------(从注册表中)删除服务。
          create----------创建服务(并将其添加到注册表中)。
          control---------向服务发送控制。
          sdshow----------显示服务的安全描述符。
          sdset-----------设置服务的安全描述符。
          showsid---------显示与任意名称对应的服务 SID 字符串。
          triggerinfo-----配置服务的触发器参数。
          preferrednode---设置服务的首选 NUMA 节点。
          GetDisplayName--获取服务的 DisplayName。
          GetKeyName------获取服务的 ServiceKeyName。
          EnumDepend------枚举服务依赖关系。

        以下命令不需要服务名称:
        sc <server> <command> <option>
          boot------------(ok | bad)指示是否应将上一次启动另存为
                          最近一次已知的正确启动配置
          Lock------------锁定服务数据库
          QueryLock-------查询 SCManager 数据库的 LockStatus
示例:
        sc start MyService


QUERY 和 QUERYEX 选项:
        如果查询命令带服务名称，将返回
        该服务的状态。其他选项不适合这种
        情况。如果查询命令不带参数或
        带下列选项之一，将枚举此服务。
type=    要枚举的服务的类型(driver, service, userservice, all)
             (默认 = service)
state=   要枚举的服务的状态 (inactive, all)
             (默认 = active)
bufsize= 枚举缓冲区的大小(以字节计)
             (默认 = 4096)
ri=      开始枚举的恢复索引号
             (默认 = 0)
group=   要枚举的服务组
             (默认 = all groups)
```
## 1、MySQL的解压
将下载好的mysql的压缩包解压，并且把解压之后的mysql-8.0.15-winx64文件夹放到安装文件夹
我的是放在：D:\Software\MySQL\mysql-8.0.15-winx64
## 2、配置环境变量
右键我的计算机->属性->高级系统设置->环境变量
新建系统变量
```
变量名称：MYSQL_HOME
变量路径：D:\Software\MySQL\mysql-8.0.15-winx64\bin
```
修改path变量，加入
```
%MYSQL_HOME%\bin
```
**注意：**如果失败，可以创建一个指定初始配置文件(我现在这个版本是没有配置的)
初始化mysql前，可以通过ini文件来指定部分初始配置，比如`basedir`和`datadir`等，当然，也可以不指定利用默认的，参考文档[官方文档](https://dev.mysql.com/doc/refman/8.0/en/windows-create-option-file.html)
在mysql的根目录下，创建mysql.ini文件，加入下面内容
```
[mysqld]
# set basedir to your installation path
basedir=D:\Software\MySQL\mysql-8.0.15-winx64
# set datadir to the location of your data directory
datadir=D:\Software\MySQL\mysql-8.0.15-winx64\data
```
## 3、初始化
打开cmd控制台，输入初始化命令（由于配置了系统的环境变量，所以不需要进入到D:\Software\MySQL\mysql-8.0.15-winx64\bin下）
```
//生成临时密码
mysqlid --initialize --console
```
或者
```
//空密码
mysqld --initialize-insecure --console
```
如果选择临时密码，控制台会有以下输出，可以看到里面有临时密码（temporary password is generated for root@localhost:后面的值，注意去掉空格），请一定要记住，当然你如果不想记住也可以，只是需要再折腾一下去重置密码
```
2018-08-15T02:55:43.924361Z 0 [System] [MY-013169] [Server] C:\MyPrograms\mysql-8.0.12-winx64\bin\mysqld.exe (mysqld 8.0.12) initializing of server in progress as process 12040
2018-08-15T02:55:55.962035Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: XCeQtsgMO7-F
2018-08-15T02:56:03.261174Z 0 [System] [MY-013170] [Server] C:\MyPrograms\mysql-8.0.12-winx64\bin\mysqld.exe (mysqld 8.0.12) initializing of server has completed
```
## 4、安装windows服务
用管理员身份打开CMD，在里面输入如下命令
```
//serviceName为服务名，不输入默认为mysql
mysqld --install [serviceName]
```
如果看到下面内容，则证明服务安装成功
```
Service successfully installed.
```
## 5、启动服务
还是用管理员身份打开CMD，输入如下命令
```
net start mysql

//以下输出证明启动成功
MySQL 服务正在启动 ..
MySQL 服务已经启动成功。

//如果你要关闭的话
net stop mysql
```
## 6、更改密码
连接到mysql
```
//下面-p后面的内容就是临时密码
mysql -uroot -pXCeQtsgMO7-F
```
修改密码
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'newpassword';
```
## 总结
* 1、环境变量是为了在命令行CMD中更加方便使用mysql命令；
* 2、如果有之前有安装过其他版本mysql，记得先卸载并删除服务；
* 3、安装完记得登录并修改密码，不论是采用空密码还是临时密码；
* 4、当然，命令行终归是不方便的，现在有很多可视化界面，如：Navicat等。