## electron 如何加载 chrome 的 crx 扩展插件？

发布于2018年12月06日    作者：[苏南大叔](https://newsn.net/)    来源：[程序如此灵动~](https://newsn.net/)

---

electron是基于Chromium的，而谷歌浏览器chrome也是基于Chromium的。那么，理论上来说，chrome的扩展在electron中，也是存在着可以使用的可能性。对吧？本文中，苏南大叔将要探讨在electron中使用chrome扩展的可能性。

![1652753732066-56fa3772-258b-4f18-8a4b-5ad05bc53398.png](assets/1652753732066-56fa3772-258b-4f18-8a4b-5ad05bc53398.png)

electron 如何加载 chrome 的 crx 扩展插件？（图5-1）

本文的实验的基础环境是：mac 10.14.1，electron 3.0.5，jsonview 0.0.32.3。核心函数就是：BrowserWindow.addExtension()。

### 龙套演员jsonview

本文的实验对象，是谷歌插件jsonview，id号是：chklaanhfefbnpoihckbnefhakgolnmc，版本号是0.0.32.3，作用就是：可以格式化输出.json文件。详细的介绍信息，见下面的这个连接：

- [https://newsn.net/say/chrome-extension-json.html](https://newsn.net/say/chrome-extension-json.html)

本步骤中，主要任务就是：下载到目标插件，然后在本地的chrome浏览器（对，没看错，就是谷歌浏览器）里面安装。安装好之后才会有接下来的步骤。您可能需要的经验文字是：

- 《chrome浏览器扩展如何下载和安装》

[https://newsn.net/say/chrome-extension-download.html](https://newsn.net/say/chrome-extension-download.html)

- 《最新版chrome浏览器如何离线安装crx插件？》

[https://newsn.net/say/chrome-crx-offline.html](https://newsn.net/say/chrome-crx-offline.html)

由于crx的下载和安装，是个超级大的工程。这里就假设，读者朋友已经下载并安装好了对应的谷歌扩展crx了，搞不定的就请仔细阅读上面的两篇经验文字。

### 寻找crx插件文件位置

crx文件在谷歌浏览器中安装好之后，都是存在于本地的一个文件夹里面的。谷歌浏览器的扩展文件存放目录的路径，还是有套路可循的：

windows:

```shell
%LOCALAPPDATA%\Google\Chrome\User Data\Default\Extensions
```

Linux:

```shell
~/.config/google-chrome/Default/Extensions/ ~/.config/google-chrome-beta/Default/Extensions/ ~/.config/google-chrome-canary/Default/Extensions/ ~/.config/chromium/Default/Extensions/
```

macos:

```shell
~/Library/Application Support/Google/Chrome/Default/Extensions
```

因为苏南大叔的实验环境是mac，所以对于cd命令中的空格，注意需要转义一下。

```shell
cd ~/Library/Application\ Support/Google/Chrome/Default/Extensions
```

通过上述命令，就可以得到扩展文件的真正目录了。注意：截图是mac下的截图，其他操作系统，请注意更换路径！

![1652753721681-bdcaec58-c17f-4e48-b283-ad1a039053bb.png](assets/1652753721681-bdcaec58-c17f-4e48-b283-ad1a039053bb.png)

electron 如何加载 chrome 的 crx 扩展插件？（图5-2）

当然对比一下谷歌浏览器的扩展管理器chrome://extensions/，就可以很明确的知道每个文件夹所代表的扩展是什么了。在这里，需要得到目标扩展的路径，留给下一步备用。这里需要特别注意的是：扩展的路径需要具体到扩展插件的版本号目录：

```shell
/Users/sunan/Library/Application Support/Google/Chrome/Default/Extensions/chklaanhfefbnpoihckbnefhakgolnmc/0.0.32.3_0
```

再次特别提示：注意路径到版本号！

大家不要照抄上面的路径哦，每个人的路径都不一样的。

![1652753722326-64b9ce1a-f325-4e83-a579-40e3973ace0a.png](assets/1652753722326-64b9ce1a-f325-4e83-a579-40e3973ace0a.png)

electron 如何加载 chrome 的 crx 扩展插件？（图5-3）

### 加载谷歌插件的代码

main.js:

```javascript
let mainWindow=null
function createWindow () {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600
})   
  mainWindow.loadURL("http://test/test.json");   
  //...   
  BrowserWindow.addExtension("/Users/sunan/Library/Application Support/Google/Chrome/Default/Extensions/chklaanhfefbnpoihckbnefhakgolnmc/0.0.32.3_0");   
  //最新的API，BrowserWindow.addExtension在新版本的electron9.0以后已废弃
  session.defaultSession.loadExtension("/Users/sunan/Library/Application Support/Google/Chrome/Default/Extensions/chklaanhfefbnpoihckbnefhakgolnmc/0.0.32.3_0"); 
  //...
}
```

下面的是效果图对比，哪个是加载了插件的，哪个不是加载了插件的呢？您看出来了么？

![1652753723889-dfd9ce3d-c3a7-4533-abf4-22c12725041f.png](assets/1652753723889-dfd9ce3d-c3a7-4533-abf4-22c12725041f.png)

electron 如何加载 chrome 的 crx 扩展插件？（图5-4）

![1652753723741-f049b94b-7d94-4eec-88bf-2b0a4dc684b3.png](assets/1652753723741-f049b94b-7d94-4eec-88bf-2b0a4dc684b3.png)

electron 如何加载 chrome 的 crx 扩展插件？（图5-5）

### 总结

特别提示一下，并不是所有的chrome插件都适用于electron，如果碰到不能用的谷歌扩展，可以自行修改crx的相关源码，或者寻找其他替换方案，没有百分百的完美兼容。请冷静对待这个谷歌插件加载方案，谢谢。

在实际的代码中，这个谷歌插件肯定不能放置在这个目录上面，并且还会涉及到相对路径，以及打包asar的处理相关问题。不过，这些都不是苏南大叔目前要考虑的问题了。这些问题，都请参考苏南大叔的electron相关经验文章：

- [https://newsn.net/tag/electron](https://newsn.net/tag/electron)

