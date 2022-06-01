## Could not resolve host: gitee.com

## 使用git报错：Could not resolve host: gitee.com解决办法

基本上是因为使用了代理导致的，把代理关了就好。

### 网上解决方案：

使用git拉去远程仓库的文件（代码） 出现了 Could not resolve host: [gitee.com](http://gitee.com)

解决措施：

```shell
**1. cmd -> 打开命令行 -> 输入： ipconfig -> 找到当前使用的ipv4 (也就是IP地址)

2. ping一下IP地址 (ping 192.168.1.110)
3. 如果正常，请修改git安装目录中 (xxx/git/etc/hosts) 的hosts主机文件
4. 添加信息如： 192.168.1.117 gitee.com
```

