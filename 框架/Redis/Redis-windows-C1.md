# Redis-windows

**命令**

- `config get requirepass`   查看当前redis是否由设置密码，显示`requirepass` `null`为没有密码
- `config set requirepass 123456`   设置密码
- `redis-cli --raw -h 'ip' -p 'port'`   连接方式 
- `auth 123456`   密码验证

**配置文件修改**

- 永久密码修改
  - 进入redis.conf配置文件，找到requirepass属性，修改成自己想要的密码，保存重启redis即可
- 