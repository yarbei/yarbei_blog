# electron下载文件

# electron下载文件

---

## abbrlink: 18

今天有个需求，我先来交代一下背景：

项目中由于之前的历史遗留问题，应用内的下载操作都是用node来实现的，目的是为了获取到文件的下载进度，从而达到一个展示出下载进度条的一个小功能。但是现在由于一些其他的原因，不能支持node双向认证。所以我们现在要把把原来用node实现的下载功能替换为浏览器的下载功能。

需求背景交代了，接下来我们来说一下实现方法。由于electron程序因为本身原理上是基于浏览器内核的，所以实现起来也比较简单。

这里主要使用到了主/渲染 进程间通信，渲染进程把下载地址、保存路径等文件信息发送给主进程，主进程通过downloadURL方法会发送一个will-download消息，主进程捕获到这个消息后将文件保存，并且可以操控下载进度等等，成功后主进程再发送消息给渲染进程进行相应的响应。

渲染进程中：

let options = {

type: "file",

fileUrl: "[http://www.xxx.xxx/download/文件1.txt](http://www.xxx.xxx/download/%E6%96%87%E4%BB%B61.txt)",

fileName: "文件1.txt",

};

window.electron.ipcRenderer.send("file-download",options);

主进程中：

//download监听

ipcMain.on('file-download', (event, options) => {

mainWindow.webContents.downloadURL(options.fileUrl);

});

在这里我们使主进程通过downloadURL方法来触发下载动作，所有的下载动作都会被will-download给捕获到，那么我们今天的重点就在will-download这个事件之中。

const path = require("path");

mainWindow.webContents.session.on('will-download', (event, item, webContents) => {

// 设置文件保存位置

let fileSavePath = path.join(app.getPath('downloads'), item.getFilename());

item.setSavePath(fileSavePath);

})

在上面的代码中，我们已经通过will-download事件回调中item这个参数实现了设置文件存放位置的功能，我们接下来的操作也是围绕着item来进行的。

const path = require("path");

mainWindow.webContents.session.on('will-download', (event, item, webContents) => {

// 设置文件保存位置

let fileSavePath = path.join(app.getPath('downloads'), item.getFilename());

item.setSavePath(fileSavePath);

// 监听文件下载进度

item.on('updated', (event, state) => {

if (state === 'interrupted') {

// 下载被打断

} else if (state === 'progressing') {

// 下载进行中

if (item.isPaused()) {

// 暂停下载

} else {

// 下载进度

console.log(`已经下载的字节数: ${item.getReceivedBytes()}/总字节数: ${item.getTotalBytes()}`)

}

}

console.log("update")

})

item.once('done', (event, state) => {

if (state === 'completed') {

// 下载成功

console.log("下载成功！")

} else {

// 下载出错

console.log(`Download failed: ${state}`)

}

})

})

上面主要写了item的两个事件updated和done。

item的updated事件，是时刻更新触发的，我们获取文件下载可以从这里获取。下面是文件下载时的控制台输出结果：

当然我们也可以利用这两个数据来进行一个运算，获取到文件下载的百分比，实现一个比较漂亮的进度条的功能。这个小功能的实现小伙伴们可以进行自由的发挥。

接下来是item的done事件，见文知义，我们可以知道它是一个下载完成的一个事件，这里的下载完成只是代表下载这个动作结束了，并不一定就是下载成功了，所以我们又进行了一个状态的判断，只有当state === 'completed'的时候，才代表我们的文件是下载成功了，其他情况均不能算作下载成功，可以在else里面做一些对于下载异常情况的处理。

至此，我们的应用下载功能算是实现了一个较为简单的版本。最主要的下载进度的问题已经解决，小伙伴们也可以针对文件下载过程中的一些其他的状态来进行一些个性化的处理。比如用户可暂停或者继续下载，在状态栏显示下载进度百分比，下载成功后进行弹窗通知等等，这里不作赘述。
