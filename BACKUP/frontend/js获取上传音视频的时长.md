# js获取上传音视频的时长

# js获取上传音视频的时长

---

获取上传视频路径，将该路径放入video标签，获取视频时长

​

## 方式一：隐藏一个音频标签，播放获取。

```javascript
<video style="display:none;" controls="controls" id="videoData" oncanplaythrough="geTime(this)"></video>
<input type="file" onchange="changeFile(this)" />

<script type="text/JavaScript">  
function geTime(ele) {
	var hour = parseInt((ele.duration)/3600);
	var minute = parseInt((ele.duration%3600)/60);
	var second = Math.ceil(ele.duration%60);
	console.log("这段视频的时长为："+hour+"小时，"+minute+"分，"+second+"秒");
}  
 
function changeFile(ele){  
    var video = ele.files[0];  
    var url = URL.createObjectURL(video);  
    console.log(url);  
    document.getElementById("videoData").src = url;  
}  
</script>
```

​

## 方式二;通过new Audio的方式获取：

```javascript
<input type="file" onchange="changeFile(this)" >
<script>
    function changeFile(ele) {
        var content = ele.files[0]
        var url = URL.createObjectURL(content);//获取录音时长
        var audioElement = new Audio(url);//audio也可获取视频的时长
        var duration;
        audioElement.addEventListener("loadedmetadata", function (_event) {
            duration = audioElement.duration;
            console.log(duration/60);
        });
    }
</script>
```

说明：

1.URL.createObjectURL()方法会创建一个 DOMString，其中包含一个表示参数中给出的对象的URL。这个 URL 的生命周期和创建它的窗口中的 document 绑定。这个新的URL 对象表示指定的 File 对象或 Blob 对象。（个人感觉可以把对象转换成url使用，十分灵活方便，特别是对于文件对象）。

2.loadedmetadata 当指定的音频/视频的元数据已加载时，会发生 loadedmetadata 事件。音频/视频的元数据包括：时长、尺寸（仅视频）以及文本轨道。

​

## 上传之前限制一下视频的时长

由于用阿里云解析视频的时候，是按照视频时长收费的，为了节省测试费用，老板要我在上传之前限制一下视频的时长！这里通过video 的视频预览来实现的。（参考：[https://www.jianshu.com/p/dc60d8dc07de）](https://www.jianshu.com/p/dc60d8dc07de%EF%BC%89)

```javascript
const gettime = (video, size) => {
  const promise = new Promise(resolve => {
    video.addEventListener('canplaythrough', e => {
      console.log(58, e, e.target.duration, size)
      if (e.target.duration <= size) {
        resolve(true)
      } else {
        resolve(false)
      }
    })
  })
  return promise
}

export const checkSize = async (files, size) => {
  // console.log(56, Number.isNaN(Number(size)), size)
  if (!files || !files[0]) return false
  // 这一条是正式服务器不需要这段所以当size 为undefined 时默认 返回 true
  if (Number.isNaN(Number(size))) return true
  const checktimevideo = document.getElementById('checktimevideo')
  if (checktimevideo) {
    document.body.removeChild(checktimevideo)
  }
  const video = document.createElement('video')
  const url = URL.createObjectURL(files[0])
  video.src = url
  video.id = 'checktimevideo'
  video.style.display = 'none'

  document.body.appendChild(video)

  return await gettime(video, size)
}


// 这个函数调用的时候也非常简单
<input 
  type='file'
  onChange = { async e => {
    e.persist()  // 这个是用来处理 SyntheticEvent 问题的
    const isok = await checkSize(e.target.files[0], 200)  // 200 表示200秒
} }
/>
```
