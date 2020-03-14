## Centos7常用命令

**防火墙相关命令**

- 防火墙的开启、关闭、重启、与状态

  - ```shell
    #开启
    systemctl start   firewalld.service
    #关闭
    systemctl stop    firewalld.service
    #重启
    systemctl restart firewalld.service
    #状态
    systemctl status  firewalld.service
    #设置开机自启
    systemctl enable  firewalld.service
    #关闭开机自启
    systemctl disable firewalld.service
    ```

- 查看所有公开项目

  - ```shell
    #查看所有公开项
    firewall-cmd --list-all
    #查看端口是否放行
    firewall-cmd --zone=public --query-port=80/tcp
    ```

- 防火墙放行端口或服务

  - ```shell
    #添加放行端口
    firewall-cmd --zone=public --add-port=80/tcp --permanent
    #重新加载所有的设置项
    firewall-cmd --reload
    ```

  - ```shell
    #添加放行端口
    firewall-cmd --zone=public --add-service=http --permanent
    firewall-cmd --reload
    ```

- 防火墙移除放行端口或服务

  - ```shell
    #移除已经放行的端口
    firewall-cmd --zone=public --remove-port=80/tcp
    firewall-cmd --reload
    ```

  - ```shell
    #移除已经放行服务
    firewall-cmd --zone=public --remove-service=http
    firewall-cmd --reload
    ```