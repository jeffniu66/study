虚拟机安装过程中，配置地址

```bash
http://mirrors.163.com/ubuntu
```

安装完后，重启以后，选择设备，移除光驱，然后再重启





编译skynet前先安装如下工具

```bash
apt install gcc

apt install libreadline-dev autoconf

apt install make
apt install make-guile
```

编译skynet

```bash
进入skynet根目录
make linux
```

安装MySQL

```bash
apt-get install mysql-server
```

skynet连接mysql时，报错

```bash
socket: auth failed ./skynet/lualib/skynet/socketchannel.lua:465: errno:1251, msg:Client does not support authentication protocol requested by server; consider upgrading MySQL client,sqlstate:08004
```

如果 **plugin 不是 `mysql_native_password`**，比如 `caching_sha2_password`，则需要改回 **MySQL 5.7 支持的方式**：

```bash
ALTER USER '你的用户名'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';
FLUSH PRIVILEGES;
```





