## electron实现复制文字和图片到剪切板

---

### abbrlink: 10

这是一个使用electron开发的客户端项目，其中想要实现一个模拟右键菜单复制的功能。

### 文字的复制

首先需要创建一个selection对象，该对象即为你左键拖过选中的文本内容，将其转成字符串保存，然后可以移除选中的区域。

```javascript
const selection = window.getSelection()
	const text = selection.toString()
	selection.removeAllRanges()
```

创建一个input输入框，将之前获取的内容放入输入框，通过select()选中输入框，调用execCommand(‘copy’)即可将内容复制到剪贴板。

```javascript
const newInput = document.createElement('input')
	document.body.appendChild(newInput)
	newInput.value = text
	newInput.select()
	document.execCommand('copy')
  document.body.removeChild(newInput)
```

### 图片的复制

尝试过与复制文字相同的思路，即通过getSelection()获取选中部分的图片信息，最后通过execCommand(‘copy’)存入剪贴板，但是这样并不能实现复制图片到剪切板。

于是不妨转变思路，看看electron中是否有剪切板以及存取图片信息相关的模块以实现复制图片到剪切板的功能。

查看API文档后，发现electron中可以通过nativeImage模块和clipboard模块来实现图片复制。通过创建一个nativeImage实例保存图片，然后通过clipboard.writeImage(nativeImage)即可将图片存入剪切板。

```javascript
const clipboard = require('electron').clipboard
const nativeImage = require('electron').nativeImage
```

查看文档后发现只可以通过本地路径path，数据流buffer，或者图片的base64编码创建一个nativeImage实例。而目前只能拿到图片的url。所以我们首先可以通过图片url将图片先转化成base64编码。

```javascript
// 传入图片url将图片转化为base64编码
// callback回调函数中拿到base64编码并进行下一步操作
function convertImgToBase64(url, callback, outputFormat){
var canvas = document.createElement('CANVAS'),
ctx = canvas.getContext('2d'),
img = new Image;
img.crossOrigin = 'Anonymous';
img.onload = function(){
canvas.height = img.height;
canvas.width = img.width;
ctx.drawImage(img,0,0);
var dataURL = canvas.toDataURL(outputFormat || 'image/png');
callback.call(this, dataURL);
canvas = null; 
};
img.src = url;
}
```

传入回调函数，通过createFromDataURL方法根据传入的图片的base64编码创建出nativeImage实例，然后通过clipboard.writeImage()传入该实例，即可将图片复制到剪切板上。

```javascript
convertImgToBase64(item.file.url,function(base64Image) {
const image = nativeImage.createFromDataURL(base64Image)
clipboard.writeImage(image)
})
```

另外如果是想将本地图片复制到剪切板上，也可以通过这两个模块实现。

```javascript
const image = nativeImage.createFromPath(‘C:\\xxx\\xxx\\xxx.png')
clipboard.writeImage(image)
```

