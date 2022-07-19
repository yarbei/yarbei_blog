# 消息推送机制-轮询、长轮询、SSE(Server Sent Event)和WS(WebSocket)

# 消息推送机制-轮询、长轮询、SSE(Server Sent Event)和WS(WebSocket)

---


1. 轮询-数据拉取

轮询是在特定的的时间间隔（如每隔1秒），由浏览器（客户端）对服务器发出HTTP request，然后由服务器返回最新的数据给客户端的浏览器。不管服务端数据有没有变化，客户端都会发起请求，来获取数据。

特点：客户端主动向服务器请求数据，服务器被动推送消息。

缺点：浏览器需要不断的向服务器发出请求，然而HTTP request 的header是非常长的，里面包含的有用数据可能只是一个很小的值，这样会占用很多的带宽，造成资源浪费。同时也不能保证及时更新最新信息。

```javascript
// 客户端
// 1、原生js操作
var xhr = new XMLHttpRequest();
setInterval(() => {
    xhr.open('GET', '/user');
    xhr.onreadystatechange = () => {
        if (xhr.readyState == 4) {
            console.log(xhr.response);
        }
    };
    xhr.send();
}, 1000);

// 2、es6API fetch
setInterval(() => {
    fetch('/user')
        .then(response => response.json())
        .then(json => {
            console.log(json);
        })
        .catch(error => console.error('fetch ', error));
}, 1000);

// 3、jquery ajax
setInterval(() => {
    $.ajax('/get')
        .done((data) => {
            console.log(data);
        })
        .fail(() => {
            console.log('ajax error');
        })
}, 1000);
```

2. 长轮询

客户端发送请求后，服务器端不会立即返回数据，服务器端会阻塞请求，连接不会立即断开，直到服务器端有数据更新或者是连接超时才返回，客户端才再次发出请求，新建连接，如此反复，从而获取最新数据。

特点：客户端主动向服务器请求数据，服务器被动推送消息。一个连接断开后，就有新的连接产生，跟轮询（短轮询）相比，能减少资源浪费但依旧很浪费资源。

```javascript
// 客户端
// 1、原生js操作
function getXHRNewData() {
    let xhr = new XMLHttpRequest();
    xhr.open('GET', '/user');
    xhr.onreadystatechange = () => {
        if (xhr.readyState == 4) {
            console.log(xhr.response);
            getXHRNewData();
        }
    };
    xhr.send();
}
getXHRNewData();

// 2、es6API fetch
function getFetchNewData() {
    fetch('/user')
        .then(response => {
            console.log(response.json());
        })
        .catch(error => {
            console.error('fetch ', error);
        })
        .finally(() => {
            getFetchNewData();
        });
}
getFetchNewData();

// 3、jquery ajax
function getAjaxNewData() {
    $.ajax('/get')
        .done((data) => {
            console.log(data);
        })
        .fail(() => {
            console.log('ajax error');
        })
        .always(() => {
            getAjaxNewData();
        });
}
getAjaxNewData();
```

3. SSE(Server Sent Event)

Server-Sent是HTML5提出一个标准。由客户端发起与服务器之间创建TCP连接，然后并维持这个连接，直到客户端或服务器中的任何一方断开，ServerSent使用的是"问"+"答"的机制，连接创建后浏览器会周期性地发送消息至服务器询问，是否有自己的消息。

特点：客户端只需连接一次，Server就定时推送，除非其中一端断开连接。并且SSE会在连接意外断开时自动重连。

缺点：并没有达到最新消息服务器端的实时推送。

```javascript
// 客户端
// 显示聊天信息
let chat = new EventSource("/chat-room");
chat.onmessage = function (event) {
    let msg = event.data;
    $(".list-group").append("<li class='list-group-item'>" + msg + "</li>");
    // chat.close(); 关闭server-sent event
};

// 自定义事件
chat.addEventListener("myChatEvent", function (event) {
    let msg = event.data;
    $(".list-group").append("<li class='list-group-item'>" + msg + "</li>");
});
```

```javascript
// 服务端 nodejs
var express = require('express');
var router = express.Router();

router.get('/chat-room', function (req, res, next) {
    // 当res.white的数据data 以\n\n结束时 就默认该次消息发送完成，触发onmessage方法，以\r\n不会触发onmessage方法
    res.header({
        "Content-Type": "text/event-stream",
        "Cache-Control": "no-cache",
        "Connection": "keep-alive"
    });

    // res.white("event: myChatEvent\r\n"); 自定义事件
    res.write("retry: 10000\r\n"); // 指定通信的最大间隔时间
    res.write("data: start~~\n\n");
    res.end(); // 不加end不会认为本次数据传输结束 会导致不会有下一次请求
});
```

4. WS(WebSocket)-全双工通信

这也是一个HTML5标准中的一项内容，他要求浏览器可以通过JavaScript脚本手动创建一个TCP连接与服务端进行通讯。WebSocket使用了ws和wss协议，需要服务器有与之握手的算法才能将连接打开。即可以满足"问"+"答"的响应机制，也可以实现主动推送的功能。

特点:省流量，客户端什么时候想发就发，服务器端什么时候想回就回，两边都有监听者socket在负责。可以实现服务端消息的实时推送。

```javascript
// 客户端
// WebSocket对象一共支持四个消息事件 onopen, onmessage, onclose和onerror，
let socket = new WebSocket('ws://localhost:2000/');
socket.onopen = function () {
    console.log("connected");
    socket.send('Hello Server~~');
};
socket.onmessage = function (event) {
    $(".list-group").append(`<li class='list-group-item'>${event.data}</li>`);
};
socket.onerror = function () {
    console.log("connect error");
};
```

```javascript
// 服务器端 nodejs 需要提前安装ws模块：npm install ws
let Server = require("ws").Server;
let wss = new Server({
    port: 2000
});
wss.on("connection", function (ws) {
    ws.on("message", function (data) {
        ws.send("服务器时间：" + new Date() + " => " + data); // 一个send方法就会触发前端的message事件
        // ws.send(JSON.stringify({"name": "test"}));
    });
    ws.on("close", function () {
        console.log("close websocket。");
    });
});
```
