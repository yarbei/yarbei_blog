---
abbrlink: 19
---
经过简单了解，觉得electron这个框架不错，我们看他的首页介绍：
使用JavaScript，HTML和CSS构建跨平台的桌面应用程序。


这不就是我们想要的嘛，业界著名的Visual Studio Code就是用electron来构建的，就它了。


由于项目是使用的react搭建的，并且已经部署完成，我们这里就不再对业务上进行过多的更改。这里仅仅介绍怎样在已有的基础上使之变成一个桌面应用。


首先引入electron，在项目所在文件夹运行以下命令


```javascript
npm install --save-dev electron
```


在安装了electron之后，在根目录下创建main.js文件。


Electron apps 使用JavaScript开发，其工作原理和方法与Node.js 开发相同。electron模块包含了Electron提供的所有API和功能，在react项目中引入electron，引入方法和普通Node.js模块一样：


```
const electron = require('electron')
```


electron 模块所提供的功能都是通过命名空间暴露出来的。比如说：electron.app负责管理Electron 应用程序的生命周期， electron.BrowserWindow类负责创建窗口。我们应当在main.js中创建窗口，并处理程序中可能遇到的所有系统事件。下面是electron官方提供的一个main.js

```javascript
const { app, BrowserWindow } = require('electron')
function createWindow () {     // 创建浏览器窗口  
const win = new BrowserWindow({    
	width: 800,    
	height: 600,    
	webPreferences: {      
	nodeIntegration: true,      
	preload: `${__dirname}/preLoad.js`    
}  
})
  // 并且为你的应用加载index.html  
  win.loadFile('index.html')
  // 打开开发者工具  

  win.webContents.openDevTools()}
// Electron会在初始化完成并且准备好创建浏览器窗口时调用这个方法// 部分 API 在 ready 事件触发后才能使用。
	app.whenReady().then(createWindow)
//当所有窗口都被关闭后退出
app.on('window-all-closed', () => {  
  // 在 macOS 上，除非用户用 Cmd + Q 确定地退出，  
  // 否则绝大部分应用及其菜单栏会保持激活。  
  if (process.platform !== 'darwin') {    
    app.quit()  
  }})
app.on('activate', () => {  
  // 在macOS上，当单击dock图标并且没有其他窗口打开时，  
  // 通常在应用程序中重新创建一个窗口。  
  if (BrowserWindow.getAllWindows().length === 0) {    
    createWindow()  
  }})
// 您可以把应用程序其他的流程写在在此文件中
// 代码 也可以拆分成几个文件，然后用 require 导入。
```


需要注意的是这里：


![](https://cdn.nlark.com/yuque/0/2020/png/275376/1597905360044-118ba170-c57d-4fc3-a8e5-a470b851e3bb.png#align=left&display=inline&height=868&margin=%5Bobject%20Object%5D&originHeight=868&originWidth=1070&size=0&status=done&style=none&width=1070)


第一处的配置在官网提供的main.js里是没有的，这里不要忘了加，要不然会提示找不到node的错误。
因为我们是加载的react生成的页面，并不是静态页面，所以要将第二处的loadFile换成loadURL(localhost:3000),括号里面的地址，是可以自己配置的，比如我们的项目现在已经上线了，我这里完全可以写线上环境的域名地址，在开发环境的时候我们还可以写react项目启动时的地址。
最后修改package.json


```javascript
{  
    "name": "My-App",  
    "version": "0.0.1",  
    "private": true,  
    "description": "",  
    "main": "main.js",//添加main.js  
    "scripts": {    
      "dev": 
      "node scripts/start.js",    
      "build": "rm -rf dist && node scripts/build.js",    
      "start": "electron .",//添加启动指令  
     },
}
```


添加main.js以及启动指令，启动时输入npm run start指令即可启动你的electron程序了。
就在我以为已经大功告成的时候，领导却说，使用体验很不好，在网络不好的情况下，应用存在白屏的情况。
这是由于我们的窗口在打开的时候链接的是线上的地址，所有的静态资源，DOM结构，CSS样式等都是从服务器拉取的，所以就会存在网络不好的情况下会出现白屏的问题。

在上一篇文章，我们成功的把一个web应用变成一个桌面应用之后，又遇到了资源加载的问题。


也就是说，我们虽然已经把它变成了一个桌面端的应用，但其实本质上仍然是一个浏览器，我们打开它相当于打开了一个网址，然后开始请求资源，加载页面。那么我们既然做成了一个应用，我们能不能把这些页面结构，样式，脚本，图片，等等这些静态资源全部都在本地加载呢？答案是可以的。这样即使网络情况不好，也不会影响我们应用打开的样子。


那么今天我们就来完成这一件事情，首先我们要清楚的是，react项目打包部署之后，在浏览器地址栏输入服务器地址，它此时加载出来的页面，就是我们打包好之后的静态资源，它的入口，就是我们打包之后生成的dist文件夹下的index.html。
在上一篇文章里说到，根目录下新建的main.js这个文件中，我们创建的窗口，有一个win.loadURL()，这个参数，已经被我们设定成了线上的地址，那我们其实就可以把它写成我们打包之后的dist文件夹里的index.html。




```
* 加载应用----- electron-quick-start中默认的加载入口 win.loadURL(url.format({   pathname: path.join(__dirname, 'dist/index.html'),   protocol: 'file:',   slashes: true }))
```


本以为大功告成，但是没想到我npm run start启动的时候,页面仍是一片空白，这是怎么回事呢？打开控制台看看有没有报错：


![](https://cdn.nlark.com/yuque/0/2020/png/275376/1597906021292-d179df1a-98a7-49c6-966b-64d11a9a15cc.png#align=left&display=inline&height=115&margin=%5Bobject%20Object%5D&originHeight=115&originWidth=1080&size=0&status=done&style=none&width=1080)


果然，是找不到资源所在位置所导致的。原来，webpack在打包的时候，默认情况下，homepage是http://localhost:3000，build后，所有资源文件路径都是/static，而Electron调用的入口是file:协议，/static就会定位到根目录去，所以找不到静态文件。在package.json文件中添加homepage字段并设置为"."后，静态文件的路径就变成了相对路径，就能正确地找到了。


```
{  "name": "My-App",  "version": "0.0.1",  "private": true,  "description": "",  "main": "main.js",  "homepage": ".",//添加homepage字段并设置为"."  "scripts": {    "dev": "node scripts/start.js",    "build": "rm -rf dist && node scripts/build.js",    "start": "electron .",  },}
```


至此，我们再重新运行npm run build打包之后，再运行npm run start，已经可以看到修改之后的加载本地资源的electron应用了。
但是现在又面临一个问题，虽然现在加载的资源已经变成了本地的，可如果我是开发环境，每次修改之后还得先打包，再启动electron才可以看到效果，有没有方法可以热更新，让我不用每次都打包就可以看到更改呢？
当然可以，在main.js文件中将win.loadURL为 http://localhost:3000


```
win.loadURL('http://localhost:3000')
```


这样在开发时，需要启动两个终端，一个终端启动npm  run dev， 另一个终端启动npm run start，这样在electron中就可以热调试了。
不过编译发布时需要改回从需要将win.loadURL改回dist/index.html，这样很麻烦，可以在package.json中新增一条命令，然后修改main.js，代码如下：


```
// package.json{  "name": "My-App",  "version": "0.0.1",  "private": true,  "description": "",  "main": "main.js",  "homepage": ".",//添加homepage字段并设置为"."  "scripts": {    "dev": "node scripts/start.js",    "build": "rm -rf dist && node scripts/build.js",    "start": "electron .",    "start-dev": "electron . dev",//新增一条start-dev命令  },}
// main.jsif(process.argv[2]==="dev") {   win.loadURL("http://localhost:3000")} else {   win.loadURL(url.format({    pathname:path.join(__dirname, '/dist/index.html'),     protocol:'file:',     slashes:true   }))}
```


这样可以在开发环境下运行npm run dev启动react，再运行npm run start-dev启动electron。在编译发布时就运行npm run build打包react项目代码，再运行npm run start启动electron，此时electron启动页加载的就是打包之后的本地静态资源。


然而事情似乎并没有这么简单，我发现打包之后的应用虽然静态资源加载的是本地的，但是所有的接口也不能用了，所有的接口指向的居然是本地路径！！！


因为是第一次遇到这种情况，我并没有找到确切的原因，在网上也没有找到很好的解释。所以我斗胆猜测是因为启动页的win.loadURL使用了url.format，并且protocol:'file:'设置了一个file协议。由于我们的接口设置了代理，所以接口地址都是默认没有带域名以及端口的。因此在请求接口的时候自然而然的在其之前拼接了file://，这个问题也很简单，因为electron已经有了跨域解决方案，所以我们使用完整的接口地址即可解决。
首先要确定main.js文件中的webSecurity的值为false，设置webPreferences中的webSecurity为false，就禁用掉了浏览器的跨域安全特性（同源策略）了。


![](https://cdn.nlark.com/yuque/0/2020/png/275376/1597906021331-a7b94bf4-00cf-417c-92c1-90b5a5ca569d.png#align=left&display=inline&height=954&margin=%5Bobject%20Object%5D&originHeight=954&originWidth=1070&size=0&status=done&style=none&width=1070)


接着我们在接口处理的函数里面统一处理接口请求,将完整域名拼接在通过调用fetchRequest传进来的url上面，这里是一个简单的封装过的fetch请求函数：


```
const fetchRequest = (url, params) => {  return fetch("http://www.aaa.com"+url, {    method: "GET",    credentials: "include",    ...params,    headers: {      "Content-Type": "application/json",    }  })  .then((response) => {    if(response.ok){      return response.json();    }   }) }
```


可是如果我仍然是开发环境呢，这里拼接完整的接口会不会不走代理从而引起开发时的跨域问题呢？这个问题我们解决思路和上面解决热更新的问题一样，用process.argv[2]==="dev"来判断此时的环境是生产环境还是开发环境，fetchRequest修改之后如下：
```
const http = process.argv[2]==="dev" ? "http://localhost:3000" : "http://www.aaa.com";const fetchRequest = (url, params) => {  return fetch(http + url, {    method: "GET",    credentials: "include",    ...params,    headers: {      "Content-Type": "application/json",    }  })  .then((response) => {    if(response.ok){      return response.json();    }   }) }
```


到这里，终于是完成了需求并解决了所遇到的问题。


然而，领导似乎对结果并不是很满意......


“ 小北，我们的弹窗......能不能脱离这个主窗口，你懂我的意思吧，就是，我希望弹出的窗口和其他APP一样，是自由的，可随处拖动的，并不是局限在APP内......”


“ 老板，这...... ”


溜了溜了，明天再说，明天再说。


欢迎关注前端小北，我是亚北！
前两篇文章我们介绍了react+electron构建桌面应用和如何加载本地的静态资源。然后现在有个需求，是要使应用里的弹窗独立于主窗口，今天来实现这个需求。


在之前我们知道electron通过main.js这个文件里new一个BrowserWindow来新建一个窗口，同样的，这个应用的弹窗，也可以通过new一个BrowserWindow来新建：


```
const { app, BrowserWindow } = require('electron')
function createWindow () {     // 创建浏览器窗口  const win = new BrowserWindow({    width: 800,    height: 600,    webPreferences: {      nodeIntegration: true,      webSecurity: false,      preload: `${__dirname}/preLoad.js`    }  })  // 并且为你的应用加载index.html  win.loadURL(url.format({   pathname: path.join(__dirname, 'dist/index.html'),   protocol: 'file:',   slashes: true }))  // 创建第二个窗口  const win2 = new BrowserWindow({    width: 400,    height: 300,    webPreferences: {      nodeIntegration: true,      webSecurity: false,      preload: `${__dirname}/preLoad.js`    }  })}
```


到这里新的弹窗已经有了，那里面的内容要如何填充呢？我们可以参照第一个窗口的做法用win.loadURL()来决定加载html文件。但是react项目打完包只有一个index.html啊，新的窗口应该从哪里加载html呢。接下来我们就来解决这一问题。
首先，在config/webpack.config.js里找到entry。如果没有config文件夹需要先运行命令把我们的config配置文件给暴露出来：


```
npm run eject
```


如果你运行了之后报如下错误：


![](https://cdn.nlark.com/yuque/0/2020/png/275376/1597906538091-78689216-fc7d-48e9-95d0-48ff316469b6.png#align=left&display=inline&height=392&margin=%5Bobject%20Object%5D&originHeight=392&originWidth=1080&size=0&status=done&style=none&width=1080)


这是因为我们使用脚手架创建一个项目的时候，自动给我们增加了一个.gitignore文件，但是我们本地却没有文件仓库。需要在终端输入如下命令：


```
git add .git commit -am "Save before ejecting"
```


就是用git将项目添加到我们的本地仓库,接下来再执行npm run eject就没有问题了。


然后项目下会多出两个文件夹，config和scripts，我们开发中一般只需关心config文件下的webpack.config.js，paths.js，webpackDevServer.config.js，其他多出来的文件不要管他。下面就是config/webpack.config.js里entry的代码：


![](https://cdn.nlark.com/yuque/0/2020/png/275376/1597906538123-7c8f64ef-87a6-4ed0-9ad0-f8cdebe2ba6a.png#align=left&display=inline&height=145&margin=%5Bobject%20Object%5D&originHeight=145&originWidth=1080&size=0&status=done&style=none&width=1080)


这里面就只是显示index的路径，我们需要在config/paths.js里添加我们几个模块的路径：


```
module.exports = {  appIndexJs: resolveModule(resolveApp, 'src/index/index'),  // 添加的模块路径  appModalJs: resolveModule(resolveApp, 'src/Modal/index'),}
```


修改config/webpack.config.js的entry为对象形式：


```
entry: {      index: [paths.appIndexJs, isEnvDevelopment && require.resolve('react-dev-utils/webpackHotDevClient')].filter(Boolean),      modal: [paths.appModalJs, isEnvDevelopment && require.resolve('react-dev-utils/webpackHotDevClient')].filter(Boolean)    },
```


添加html配置，同样在config/paths.js里面添加：


```
appHtml: resolveApp('public/index.html'),// 新增的htmlappModalHtml: resolveApp('public/modal.html'),
```


config/paths.js里完整的module.exports如下：


```
module.exports = {  dotenv: resolveApp('.env'),  appPath: resolveApp('.'),  appBuild: resolveApp('build'),  appPublic: resolveApp('public'),  appHtml: resolveApp('public/index.html'),  // 新增的html  appModalHtml: resolveApp('public/modal.html'),  appIndexJs: resolveModule(resolveApp, 'src/index'),  // 添加的模块路径  appModalJs: resolveModule(resolveApp, 'src/Modal/index'),  appPackageJson: resolveApp('package.json'),  appSrc: resolveApp('src'),  appTsConfig: resolveApp('tsconfig.json'),  appJsConfig: resolveApp('jsconfig.json'),  yarnLockFile: resolveApp('yarn.lock'),  testsSetup: resolveModule(resolveApp, 'src/setupTests'),  proxySetup: resolveApp('src/setupProxy.js'),  appNodeModules: resolveApp('node_modules'),  publicUrlOrPath,};
```


在config/webpack.config.js里处理html，在plugins中将原来的HtmlWebpackPlugin进行配置修改，添加一个filename和chunks：


```
Object.assign(          {},          {            filename: 'index.html',// 添加filename            chunks: ['index'] //添加chunks          },
```


对之前配置的每个页面都复制一个改一下它的template和filename以及chunks:


```
new HtmlWebpackPlugin(        Object.assign(          {},          {            inject: true,            template: paths.appHtml,            filename: 'index.html',            chunks: ['index']          },          isEnvProduction            ? {                minify: {                  removeComments: true,                  collapseWhitespace: true,                  removeRedundantAttributes: true,                  useShortDoctype: true,                  removeEmptyAttributes: true,                  removeStyleLinkTypeAttributes: true,                  keepClosingSlash: true,                  minifyJS: true,                  minifyCSS: true,                  minifyURLs: true,                },              }            : undefined        )      ),      new HtmlWebpackPlugin(        Object.assign(          {},          {            inject: true,            template: paths.appTicketHtml,            filename: 'ticket.html',            chunks: ['ticket']          },          isEnvProduction            ? {              minify: {                removeComments: true,                collapseWhitespace: true,                removeRedundantAttributes: true,                useShortDoctype: true,                removeEmptyAttributes: true,                removeStyleLinkTypeAttributes: true,                keepClosingSlash: true,                minifyJS: true,                minifyCSS: true,                minifyURLs: true,              },            }            : undefined        )      )
```


然后我们运行npm run build来对react项目进行打包。如果build后报Cannot read property 'filter' of undefined，那么就需要把new ManifestPlugin里的generate属性删掉，这个问题我不是很清楚，从网上也是只搜到了解决方案而没有搜到原因，如果有前端小伙伴明白其中的原因，希望可以指点一二，不胜感激！


至此，我们的react项目已经可以打包出两个html文件和其对应的资源了，我们就用win2.loadURL()使其拥有两个独立的窗口。如下：


```
// 创建第二个窗口  const win2 = new BrowserWindow({    width: 400,    height: 300,    webPreferences: {      nodeIntegration: true,      webSecurity: false,      preload: `${__dirname}/preLoad.js`    }  })  win2.loadURL(url.format({   pathname: path.join(__dirname, 'dist/index.html'),   protocol: 'file:',   slashes: true }))
```
窗口相对独立了我们可以通过electron进程间通讯来控制两个窗口何时打开关闭，达到一个交互效果。当然了，如果要做更多页面也是可以的，我们可以继续优化webpack的配置，做到可以自动打包多个页面，这些我们将来再做介绍。


经过几天的更新，目前算是较为完整的实现了将一个基于react的web应用利用electron变成了一个桌面应用。中间也包含了很多由此引出的问题以及解决思路，当然也存在还没有搞明白的问题，技术略菜，欢迎各位前端小伙伴一起探讨...




![](https://cdn.nlark.com/yuque/0/2020/gif/275376/1597906538014-2633faaa-a4b3-4aaa-b189-0b72df20b69b.gif#align=left&display=inline&height=170&margin=%5Bobject%20Object%5D&originHeight=170&originWidth=183&size=0&status=done&style=none&width=183)


