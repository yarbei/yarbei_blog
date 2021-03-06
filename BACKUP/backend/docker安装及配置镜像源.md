# docker安装及配置镜像源

# docker安装及配置镜像源

---

## abbrlink: 1

step 1: 安装必要的一些系统工具

```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

Step 2: 添加软件源信息

```shell
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

Step 3: 更新并安装Docker-CE

```shell
sudo yum makecache fast
sudo yum -y install docker-ce
```

Step 4: 开启Docker服务

```shell
sudo service docker start
```

或者

```shell
sudo systemctl start docker
```

Step 5:安装校验

```shell
docker version  	  #显示docker版本信息

docker info #显示docker系统信息

docker 命令 --help  	#查看命令帮助

docker：重启docker服务

service docker restart
```

或者

```shell
systemctl restart docker

docker：关闭Docker服务

service docker stop
```

或者

```shell
systemctl stop docker

docker：守护进程重启

systemctl daemon-reload
```

1

docker：配置国内镜像源

国外网站访问非常慢，所以这里配置了中国官方镜像源

```shell
vim /etc/docker/daemon.json

{

regstry-mirrors": ["[https://registry.docker-cn.com](https://registry.docker-cn.com)"]

}
```

通过 docker info查看镜像是否配置成功，显示Registry Mirrors:

httpxxxxxx

表示成功

附：

Docker中国区官方镜像

[https://registry.docker-cn.com](https://registry.docker-cn.com)

​

网易163镜像

[http://hub-mirror.c.163.com](http://hub-mirror.c.163.com)

​

阿里云镜像

[https://cr.console.aliyun.com/](https://cr.console.aliyun.com/)

https://{your_id}.mirror.aliyuncs.com

​

中科大镜像加速

[https://docker.mirrors.ustc.edu.cn](https://docker.mirrors.ustc.edu.cn)
