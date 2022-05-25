由于很长时间没有使用MySql，今天想启动一下发现启动不了，输入MySql启动命令报错。
 操作步骤如下：
 输入MySql启动命令：



```bash
sudo /usr/local/mysql/support-files/mysql.server start
```

报错信息如下：



```bash
Starting MySQL
.Logging to '/usr/local/mysql/data/mac.local.err'.
 ERROR! The server quit without updating PID file (/usr/local/mysql/data/mac.local.pid).
```

后面找到原因发现目录`/usr/local/mysql/data/`的mysql拥有者权限发生了变更，没有写的权限，所以需要对该目录赋予写的权限，操作命令如下：



```bash
sudo chown -R mysql /usr/local/mysql/data
```

然后再次输入启动命令：



```bash
sudo /usr/local/mysql/support-files/mysql.server start
Starting MySQL
.Logging to '/usr/local/mysql/data/oxy-mac.local.err'.
. SUCCESS! 
```

成功了！