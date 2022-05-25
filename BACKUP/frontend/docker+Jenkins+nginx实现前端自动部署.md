---
title: docker+Jenkins+nginx实现前端自动部署
author: 亚北
avatar: /images/avatar.png
categories: 技术
comments: true
keywords: docker、Jenkins、Nginx、前端、自动部署
photos: 'https://gitee.com/yarbei/imgcloud/raw/main/202111292239974.png'
abbrlink: 2224127129
date: 2021-11-29 00:00:00
authorLink:
authorAbout:
authorDesc:
tags:
description:
---


# docker+Jenkins+nginx实现前端自动部署

阿里云双11大促买了个服务器玩一玩

买之前很激动，买了之后，emmmm我用它能干嘛

找我干运维的好兄弟问一问，好兄弟也对我想做的事情很疑惑

![](https://cdn.yarbei.com/img/202205191107130.png)

所以我稀里糊涂买了服务器但是并没有想好用它来做什么

先介绍一下配置吧，我是用的女朋友的账号买的，阿里云双11对新人优惠力度更大

最终选了2核4G，5M带宽，40G硬盘这样的一个配置，一次性买了3年，800多

有想法的小伙伴也可以尝试一下，错过了双11还有双12可以看看

如果像我一样买了之后不知道干嘛，实在不行还可以部署一下自己的网站嘛

说起来我的博客自从部署在gitee之后就没有然后了。。。

博客是用hexo写的，hexo这个东西适合想拥有自己的博客但是又没什么想法的同学

它提供了非常多的精美的模板，可以选择自己喜欢的模板，一键生成自己的博客

hexo生成的静态的html页面，所以我只需要在服务器配置一个Nginx服务，然后再把我的资源文件扔上去就行

但是，但是，如果仅仅是这样来部署，那也太简单了，不如让它更加的自动化

所以我想要每次我对文件更改完成后让服务器自动帮我完成部署的事情，

而不是每次手动打包，上传，解压，等等......

这一系列的操作时间长了那可真是受不了

所以我的需求也渐渐的清晰明了起来，要有Nginx，要能自动部署，公司用的Jenkins我比较熟悉，这一次仍然选择它作为我自动部署的工具

上帝说要有光，于是便有了光，我想做上帝，需要有个沙箱

>Docker是一个开源的应用容器引擎，基于 [Go 语言](https://www.runoob.com/go/go-tutorial.html) 并遵从 Apache2.0 协议开源。
Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

有了沙箱之后就可以愉快的在里面进行创造了。

## 环境准备

- 服务器： CentOS 8
- docker
- docker-compose
- nginx 镜像
- jenkins 镜像
- gitee

## 安装docker环境

1. 链接服务器

```shell
ssh root@000.000.000.000 #@你自己的公网IP
```

2. 安装必要的一些系统工具

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```

3. 添加Docker CE的软件源信息。

```shell

sudo yum-config-manager --add-repo \ https://download.docker.com/linux/centos/docker-ce.repo #容易失败，可以尝试下面这个
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
4. 更新并安装Docker CE

```shell
sudo yum makecache fast
sudo yum -y install docker-ce
复制代码
```

5. 启动Docker服务

```shell
sudo systemctl enable docker #设置开机自启
sudo systemctl start docker  #启动docker
```
6. 安装校验

```shell
docker version  	  	 #显示docker版本信息
docker info 					 #显示docker系统信息
docker 命令 --help  	  #查看命令帮助
```

![](https://cdn.yarbei.com/img/202205191107131.png)

## 安装docker-compose

> docker-compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

1. 下载docker-compose：

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. 提升权限：

```shell
sudo chmod +x /usr/local/bin/docker-compose
```

3. 查看是否安装成功

```shell
docker-compose -v
```

![](https://cdn.yarbei.com/img/202205191107131.png)

## 安装Nginx和Jenkins镜像

docker 拉取Nginx和Jenkins镜像命令如下：

```shell
docker pull nginx  #安装Nginx
docker pull jenkins/jenkins:lts   #安装Jenkins
```

安装完成后执行docker images可以查看docker下存在的镜像：

![](https://cdn.yarbei.com/img/202205191107140.png)

## 编写配置目录

接下来创建配置目录，结构如下：

```shell
├── compose
│   └── docker-compose.yml  #docker-compose配置文件
├── jenkins
│   └── jenkins_home  #Jenkins挂载卷
├── nginxcfg
    └── default.conf  #Nginx配置
```

以上目录我是放在根目录之下的，同学们也可以放在自己喜欢的位置

## 文件配置

docker-compose.yml配置文件内容：

```shell
version: '3'
services:                                      
  docker_jenkins:
    user: root                                
    restart: always                            
    image: jenkins/jenkins:lts                 # 指定服务所使用的镜像 
    container_name: jenkins                    # 容器名称
    ports:                                     # 对外暴露的端口定义
      - 8080:8080
      - 50000:50000
    volumes:                                   # 卷挂载路径
      - /root/jenkins/jenkins_home/:/var/jenkins_home  #冒号前为刚刚创建的路径，这里要写绝对路径
      - /var/run/docker.sock:/var/run/docker.sock
      - /usr/bin/docker:/usr/bin/docker                
      - /usr/local/bin/docker-compose:/usr/local/bin/docker-compose
  docker_nginx:
    restart: always
    image: nginx
    container_name: nginx
    ports:
      - 8090:80
      - 80:80
      - 433:433
    volumes:
      - /root/nginxcfg:/etc/nginx/conf.d  #用我们创建的Nginx配置去替换容器中的默认配置，冒号前为我们创建的目录的路径
      - /root/nginxcfg/logs:/var/log/nginx  #nginx日志位置
      - /root/xxx/xxx/xxx:/usr/share/nginx/html
```

default.conf文件配置

```shell
error_log  /var/log/nginx/error.log notice;
server{ # 简单的监听80端口，指定index位置
  listen  80;
  root /usr/share/nginx/html;
  index index.html index.htm;
}
```

在这里我踩了一个坑，就是不懂docker-compose.yml这个配置文件该去怎么写，里面的每个参数都是什么意思，也是查了很多资料才弄明白，关键的是哪个挂载卷，volumes这个参数，我的理解是用我们的一个配置去替换掉容器的默认配置，这里我请教过一些同学，他们说要在启动docker容器的时候，用docker命令决定以哪一个配置来启动，就使我很不理解，这个docker-compose.yml文件不就是用来做这个事情的吗?

最终尝试了很多次之后，终于弄明白了，问题在volumes之下，冒号前面写的是我的配置路径，冒号之后的路径是docker容器内各个镜像默认的配置路径

这个volumes就是用自己的配置去替换掉默认配置

上述两个文件配置完成之后，需要进入compose目录下面输入以下命令启动：

```shell
docker-compose up -d
```

停止：
```
docker-compose stop
```

##   Jenkins配置

输入docker ps -a查看容器的运行情况:

![](https://cdn.yarbei.com/img/202205191107141.png)
状态显示为up,此时在浏览器地址栏输入服务器公网IP:8080端口可显示Jenkins管理页面：

![](https://cdn.yarbei.com/img/202205191107142.png)

如果是第一次进入，需要做一些初始化工作，因为我已经安装过了，就不重新展示初始化的过程了，大概需要以下步骤：

1. 根据提示找到密码复制粘贴

   通过这个命令可以获取，/root/jenkins/jenkins_home为挂载目录

   cat /root/jenkins/jenkins_home/secrets/initialAdminPassword

2. 安装推荐的插件

3. 设置管理员账号

然后就可以愉快的使用Jenkins了



接着点击系统管理->插件管理，需要在Jenkins安装两个插件：

1. 安装 [Publish Over SSH](https://plugins.jenkins.io/publish-over-ssh/) 作用: 将构建后的编译产出发布到服务器
2. 安装[Generic Webhook Trigger Plugin](https://plugins.jenkins.io/generic-webhook-trigger/)作用：通用 Webhook 触发器构建

点击系统管理->系统配置，全局配置ssh：

![](https://cdn.yarbei.com/img/202205191107145.png)

然后到系统管理->全局工具配置，安装nodejs：

![](https://cdn.yarbei.com/img/202205191107146.png)

- 注意node版本，有的项目对node版本有要求，可能会出现构建不成功的情况

## 新建一个Jenkins构建任务

1. 选择新建任务,输入任务名称，构建一个自由风格的软件项目：

![](https://cdn.yarbei.com/img/202205191107147.png)

2. 配置源码管理：

![](https://cdn.yarbei.com/img/202205191107148.png)

3. 构建触发器配置：

![](https://cdn.yarbei.com/img/202205191107149.png)

4. gitee配置webhook：

![](https://cdn.yarbei.com/img/202205191107150.png)

- URL为：http://服务器公网IP:8080/generic-webhook-trigger/invoke?token=你的token
- 选择push触发构建任务
- 这里选择了gitee，毕竟国内速度快一些，GitHub的配置也是一样的，源码在GitHub的同学可以自己研究一下

5. 构建环境选择node：

![](https://cdn.yarbei.com/img/202205191107151.png)

6. 构建：

![](https://cdn.yarbei.com/img/202205191107152.png)

- 增加构建步骤选择执行shell

- 编写shell脚本

```shell
node -v  #查看node，npm 版本
npm -v 
npm i  #npm安装项目所需依赖
npm install hexo-cli -g  #npm安装hexo
hexo clean  #hexo清除缓存文件和静态文件
hexo g  #hexo生成静态文件
tar -zcvf public.tar ./public  #压缩生成的静态文件目录
```

- 以上shell脚本就是关联的git仓库有了推送事件之后触发的构建脚本，也是我的hexo博客项目所需的构建过程，同学们可以根据需要，编写自己项目的构建脚本

7. 构建后操作：

![](https://cdn.yarbei.com/img/202205191107153.png)

- 构建后操作选择send build artifacts over SSH
- 填写要上传到服务器的文件名称（在构建脚本最后一句：tar -zcvf public.tar ./public）
- 填写上传到服务器的路径（这里的路径是以Jenkins配置 [Publish Over SSH](https://plugins.jenkins.io/publish-over-ssh/) 插件时的那个路径为根目录的，最终的路径为docker-compose.yml中配置的Nginx下root目录，比如这里我写的是/yarbei/apps,上传文件的实际的实际路径为/root/yarbei/apps/public.tar，root目录为/root/yarbei/apps/yarbeiweb，之后public.tar解压替换yarbeiweb)
- 编写文件上传后的脚本

```shell
cd /root/yarbei/apps  #进入文件所在目录
mv yarbeiweb yarbeiweb-$(date +%Y%m%d-%H%M) #将旧的文件夹更名备份
tar zxvf public.tar  #解压public.tar
mv public yarbeiweb  #将解压后的文件夹改名
rm -rf public.tar  #删除压缩包
```

至此，基于docker+Jenkins+Nginx实现的前端自动部署功能就实现了

![](https://cdn.yarbei.com/img/202205191107154.png)

## 小结

其实这次的折腾是属于意料之外的，源于双十一的一次冲动消费，不过经过一番折腾也算有了一些收获，作为一个前端工程师，我对服务器、运维方面的知识储备是比较少的，期间走了不少弯路，比如路径问题，docker-compose的配置问题，在文中都有体现。至于为什么是docker，可能也是对Linux命令行的操作方式了解较少，不想因为自己操作失误最终让整个环境乱遭糟，到最终不可控。使用docker不仅能快速实施，而且能隔离环境，避免环境依赖。接下来就可以通过我自己的服务器访问博客了，域名正在备案中😂😂

本次部署大概步骤如下：

1. 准备环境
2. 安装docker
3. 安装docker-compose
4. 安装Jenkins和Nginx
5. 编写配置文件
6. 配置Jenkins
7. 配置Jenkins构建任务

## 参考资料

- [小白都能看懂的前端部署（docker+nginx+jenkins） - 掘金 (juejin.cn)](https://juejin.cn/post/6844904111826010125#heading-7)
- [配置项目构建完成后文件移动---- Jenkins自动化部署学习笔记（三） - 简书 (jianshu.com)](https://www.jianshu.com/p/ca07a19d036a)
- [Jenkins设置指定路径的source file进行SSH传输管理_fan_haha的博客-CSDN博客](https://blog.csdn.net/fan_haha/article/details/110224311)


