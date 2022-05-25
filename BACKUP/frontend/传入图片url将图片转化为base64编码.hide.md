---
abbrlink: 8
---
_https://blog.csdn.net/myf8520/article/details/107340712/_
_传入图片url将图片转化为base64编码_
_callback回调函数中拿到base64编码并进行下一步操作_
   
```javascript
convertImgToBase64=(url, callback, outputFormat)=>{
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
