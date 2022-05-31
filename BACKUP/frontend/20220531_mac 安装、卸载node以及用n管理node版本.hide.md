---
abbrlink: 12
---
一、安装 nodenode 中文官网下载最新的安装包。
​

如果要安装以往的 node 版本请戳这里。我安装的是 10.13.0 版本。
​

【注意】：下载时，建议选择 尾缀名是 .pkg 的文件 下载。
​

下载下来，直接安装，一路绿灯，OK。
​

然后，查看 node 版本号：
​

命令行输入：
​

```javascript
node -v
```
若成功会显示你安装的 node 版本号。
​

顺便查看一下 npm 的版本号：
​

命令行输入：
​

```javascript
npm -v
```
如何查看 node 安装目录呢？
​

```javascript
which node
```
 
​

二、用 n 管理 node 版本
1、清除 node 缓存
```javascript
sudo npm cache clean -f
```
2、使用 npm 安装 n
命令行执行：
​

```javascript
npm install -g n
```
如果报错：code EACCES errno -13，表示你没有权限，请以管理员身份安装：
​

```javascript
sudo npm i n -g
```
查看 n 是否安装成功
​

```javascript
n -V（大写的V）
```
若成功，就会显示 n 的版本号。
​

3、使用 n 管理 node
（1）、查看可用 node 列表
```javascript
n ls
```
（2）、安装指定版本的 node
查看官方所有的 node 版本：
​

```javascript
npm view node versions
```
 以安装 14.15.3 版本的 node 为例，命令行执行：
​

```javascript
n 14.15.3
```
跑了一堆代码，可最终显示的还是原来的版本：
​

installed : v10.13.0 (with npm 6.4.1)
说明安装失败了。 是不是权限的问题呢？所以，用管理员身份运行一下：
​

```javascript
sudo n 14.15.3
```
最终显示的版本为新的版本：
​

installed : v14.15.3 (with npm 6.14.9)
至此，安装成功了。
​

（3）、切换 node 版本
现在，我的 node 版本是14.15.3 的，我想使用 10.13.0 的版本。
​

命令行执行：
​

```javascript
n
```
通过上下键，选择要使用的 node 版本 
​

```javascript
dd@lff ~ % n
 
 
  ο node/10.13.0
    node/14.15.3
 
Use up/down arrow keys to select a version, return key to install, d to delete, q to quit（使用上/下箭头键选择版本，回车键安装，d删除，q退出）
```
回车安装，待代码运行完毕后，发现最终显示的还是原来的版本：
​

installed : v14.15.3 (with npm 6.14.9)
说明切换 node 失败了。 是不是权限的问题呢？所以，用管理员身份运行一下：
​

```javascript
sudo n
```
最终显示的版本号是：
​

installed : v10.13.0 (with npm 6.4.1)
至此，node 版本切换成功了。
​

（4）、删除指定版本 node
命令行运行：
​

```javascript
n rm v10.13.0
```
然后，查看 node 版本，发现还是 10.13.0。
​

接着，查看一下已安装的 node 版本：
​

```javascript
n ls
node/10.13.0
node/14.15.3
```
这就说明 10.13.0 版本的 node 并没有删除掉，是不是权限的问题呢？所以，用管理员身份运行一下：
​

```javascript
sudo n rm v10.13.0
```
然后，查看 node 版本，发现还是 10.13.0。
​

再查看一下已安装的 node 版本：
​

```javascript
n ls
node/14.15.3
```
发现，10.13.0 的 node 已经被删掉了。
​

最后，记得用 n 切换一下 node 版本（相见第 （3）步），以正常使用 node。
​

4、卸载 n
命令行执行：
​

```javascript
npm uninstall n -g
```
或
```javascript
sudo npm uninstall n -g
```
 
​

三、卸载 node
首先，命令行执行：
​

```javascript
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```
查看 node 安装目录：
​

```javascript
which node
```
/usr/local/bin/node
依次执行下面的命令，删除 node 安装目录下的相关配置文件：
​

```javascript
sudo npm uninstall npm -g
sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*
sudo rm -rf /usr/local/include/node /Users/$USER/.npm
sudo rm /usr/local/bin/node
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d
```
最后验证一下是否完全删除掉：
​

```javascript
node 
bash: node: command not found
npm
bash: /usr/local/bin/npm: No such file or directory
```
至此，node 已经完全卸载掉了。
