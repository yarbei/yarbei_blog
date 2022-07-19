# iframe

# iframe

---


该元素包含[全局属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes)。

[allow](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-allow)

用于为指定其[特征策略](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Feature_Policy).

[allowfullscreen](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-allowfullscreen)

设置为true时，可以通过调用 的 [requestFullscreen()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/requestFullScreen) 方法激活全屏模式。

这是一个历史遗留属性，已经被重新定义为 allow="fullscreen"。

[allowpaymentrequest](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-allowpaymentrequest)

设置为true时，跨域的 就可以调用 [Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)。

这是一个历史遗留属性，已经被重新定义为 allow="payment".

[csp](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-csp)** **

对嵌入的资源配置[内容安全策略](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)。 查看 [HTMLIFrameElement.csp(en-US)](https://developer.mozilla.org/en-US/docs/Web/API/HTMLIFrameElement/csp) 获取详情。

[height](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-height)

以CSS像素格式[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)，或像素格式HTML 4.01，或百分比格式指定frame的高度。默认值为150。

[importance](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-importance)** **

表示 的 src 属性指定的资源的加载优先级。允许的值有：

**auto (default)**

不指定优先级。浏览器根据自身情况决定资源的加载顺序

**high**

资源的加载优先级较高

**low**

资源的加载优先级较低

[name](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-name)

用于定位嵌入的浏览上下文的名称。该名称可以用作  标签与 [

]([https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form)) 标签的 target 属性值，也可以用作  标签和  标签的 formtarget 属性值，还可以用作 [window.open()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/open) 方法的 windowName 参数值。

[referrerpolicy](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-referrerpolicy)

表示在获取 iframe 资源时如何发送 [referrer](https://developer.mozilla.org/en-US/docs/Web/API/Document/referrer) 首部：

- no-referrer: 不发送 [Referer](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Referer) 首部。
- no-referrer-when-downgrade (default): 向不受 [TLS](https://developer.mozilla.org/zh-CN/docs/Glossary/TLS) ([HTTPS](https://developer.mozilla.org/zh-CN/docs/Glossary/https)) 保护的 [origin](https://developer.mozilla.org/zh-CN/docs/Glossary/Origin) 发送请求时，不发送 [Referer](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Referer) 首部。
- origin: referrer 首部中仅包含来源页面的源。换言之，仅包含来源页面的 [scheme](https://developer.mozilla.org/en-US/docs/Archive/Mozilla/URIScheme), [host](https://developer.mozilla.org/zh-CN/docs/Glossary/Host), 以及 [port(en-US)](https://developer.mozilla.org/en-US/docs/Glossary/Port)。
- origin-when-cross-origin: 发起跨域请求时，仅在 referrer 中包含来源页面的源。发起同源请求时，仍然会在 referrer 中包含来源页面在服务器上的路径信息。
- same-origin: 对于 [same origin](https://developer.mozilla.org/zh-CN/docs/Glossary/Same-origin_policy) （同源）请求，发送 referrer 首部，否则不发送。
- strict-origin: 仅当被请求页面和来源页面具有相同的协议安全等级时才发送 referrer 首部（比如从采用 HTTPS 协议的页面请求另一个采用 HTTPS 协议的页面）。如果被请求页面的协议安全等级较低，则不会发送 referrer 首部（比如从采用 HTTPS 协议的页面请求采用 HTTP 协议的页面）。
- strict-origin-when-cross-origin: 当发起同源请求时，在 referrer 首部中包含完整的 URL。当被请求页面与来源页面不同源但是有相同协议安全等级时（比如 HTTPS→HTTPS），在 referrer 首部中仅包含来源页面的源。当被请求页面的协议安全等级较低时（比如 HTTPS→HTTP），不发送 referrer 首部。
- unsafe-url: 始终在 referrer 首部中包含源以及路径 （但不包括 [fragment](https://developer.mozilla.org/en-US/docs/Web/API/HTMLHyperlinkElementUtils/hash)，[密码](https://developer.mozilla.org/en-US/docs/Web/API/HTMLHyperlinkElementUtils/password)，或[用户名](https://developer.mozilla.org/en-US/docs/Web/API/HTMLHyperlinkElementUtils/username)）。**这个值是不安全的**, 因为这样做会暴露受 TLS 保护的资源的源和路径信息。

[sandbox](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-sandbox)

该属性对呈现在 iframe 框架中的内容启用一些额外的限制条件。属性值可以为空字符串（这种情况下会启用所有限制），也可以是用空格分隔的一系列指定的字符串。有效的值有：

- allow-downloads-without-user-activation : 允许在没有征求用户同意的情况下下载文件.
- allow-forms: 允许嵌入的浏览上下文提交表单。如果没有使用该关键字，则无法提交表单。
- allow-modals: 允许嵌入的浏览上下文打开模态窗口。
- allow-orientation-lock: 允许嵌入的浏览上下文锁定屏幕方向（译者注：比如智能手机、平板电脑的水平朝向或垂直朝向）。
- allow-pointer-lock: 允许嵌入的浏览上下文使用 [Pointer Lock API](https://developer.mozilla.org/zh-CN/docs/Web/API/Pointer_Lock_API).
- allow-popups: 允许弹窗 (例如 window.open, target="_blank", showModalDialog)。如果没有使用该关键字，相应的功能将自动被禁用。
- allow-popups-to-escape-sandbox:  允许沙箱化的文档打开新窗口，并且新窗口不会继承沙箱标记。例如，安全地沙箱化一个广告页面，而不会在广告链接到的新页面中启用相同的限制条件。
- allow-presentation: 允许嵌入的浏览上下文开始一个[presentation session](https://developer.mozilla.org/en-US/docs/Web/API/PresentationRequest)。
- allow-same-origin: 如果没有使用该关键字，嵌入的浏览上下文将被视为来自一个独立的源，这将使 [same-origin policy](https://developer.mozilla.org/zh-CN/docs/Glossary/Same-origin_policy) 同源检查失败。
- allow-scripts: 允许嵌入的浏览上下文运行脚本（但不能创建弹窗）。如果没有使用该关键字，就无法运行脚本。
- allow-storage-access-by-user-activation : 允许嵌入的浏览上下文通过 [Storage Access API](https://developer.mozilla.org/en-US/docs/Web/API/Storage_Access_API) 使用父级浏览上下文的存储功能。
- allow-top-navigation: 允许嵌入的浏览上下文导航（加载）内容到顶级的浏览上下文。
- allow-top-navigation-by-user-activation: 允许嵌入的浏览上下文**在经过用户允许后**导航（加载）内容到顶级的浏览上下文。

**注意:**

- 当被嵌入的文档与主页面同源时，强烈建议不要同时使用 allow-scripts 和 allow-same-origin。如果同时使用，嵌入的文档就可以通过代码删除 sandbox 属性，如此，就安全性而言还不如不用sandbox。
- 如果攻击者可以在沙箱化的 iframe 之外展示内容，例如用户在新标签页中打开内联框架，那么沙箱化也就没有意义了。建议把这种内容放置到独立的专用域中，以减小可能的损失。
- 沙箱属性(sandbox)在Internet Explorer 9及更早的版本上不被支持。

[src](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-src)

被嵌套的页面的 URL 地址。使用 about:blank 值可以嵌入一个遵从[同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)的空白页。在 Firefox （version 65及更高版本）、基于 Chromium 的浏览器、Safari/iOS 中使用代码移除 iframe 的 src 属性（例如通过 [Element.removeAttribute()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/removeAttribute) ）会导致 about:blank 被载入 frame。

[srcdoc](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-srcdoc)** [**HTML5**](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5) only**

该属性是一段HTML代码，这些代码会被渲染到 iframe 中。如果浏览器不支持 srcdoc 属性，则会渲染 src 属性表示的内容。

[width](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-width)

以CSS像素格式[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)，或以像素格式HTML 4.01，或以百分比格式指定的 frame 的宽度。默认值是300。

### [不赞成使用的属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#%E4%B8%8D%E8%B5%9E%E6%88%90%E4%BD%BF%E7%94%A8%E7%9A%84%E5%B1%9E%E6%80%A7)

下面这些属性已不赞成使用，并且可能不再被所有的浏览器支持。你应避免在新项目里面使用它们，也应尽量从旧项目中移除它们。

[align](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-align)** 已废弃 [**HTML4.01**](https://developer.mozilla.org/zh-CN/docs/Web/HTML), 已废弃 **[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

此元素相对于周围元素的对齐方式。

[frameborder](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-frameborder)** 已废弃 **[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

值为1（默认值）时，显示此框架的边框。值为0时移除边框。此属性已不赞成使用，请使用 CSS 属性 [border](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border) 代替。

[longdesc](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-longdesc)** 已废弃 **[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

表示框架内容的长描述的 URL。由于广泛的误用，该属性对于无图形界面的浏览器不起作用。

[marginheight](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-marginheight)** 已废弃 **[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

这个属性定义了框架的内容距其上边框与下边框的距离，单位是像素。

[marginwidth](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-marginwidth)** 已废弃 **[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

这个属性定义了框架的内容距其左边框和右边框的距离，单位是像素。

[scrolling](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-scrolling)** 已废弃 **[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

这个属性控制是否要在框架内显示滚动条，允许的值包括：

- auto: 仅当框架的内容超出框架的范围时显示滚动条。
- yes: 始终显示滚动条。
- no: 从不显示滚动条。

### [非标准属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#%E9%9D%9E%E6%A0%87%E5%87%86%E5%B1%9E%E6%80%A7_non-standard_inline)

[mozbrowser](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-mozbrowser)** **

查看 [bug 1318532](https://bugzilla.mozilla.org/show_bug.cgi?id=1318532) 了解如何在 Firefox 的 WebExtensions 中使用此属性。

这个属性可以让 变得像顶级浏览器窗口，详情请参看 [Browser API](https://developer.mozilla.org/zh-CN/docs/Web/API/Using_the_Browser_API)。这个属性**只能**在 [WebExtensions](https://developer.mozilla.org/zh-CN/docs/Mozilla/Add-ons/WebExtensions) 中使用。

## [脚本](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#%E8%84%9A%E6%9C%AC)

内联的框架，就像  元素一样，会被包含在 [window.frames](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/frames) 伪数组（类数组的对象）中。

有了 DOM [HTMLIFrameElement](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLIFrameElement) 对象，脚本可以通过 [contentWindow](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLIFrameElement/contentWindow) 访问内联框架的 [window](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 对象。 [contentDocument(en-US)](https://developer.mozilla.org/en-US/docs/Web/API/HTMLIFrameElement/contentDocument) 属性则引用了 内部的 document 元素，(等同于使用contentWindow.document），但IE8-不支持。

在框架内部，脚本可以通过 [window.parent](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/parent) 引用父窗口对象。

脚本访问框架内容必须遵守[同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)，并且无法访问非同源的 window 对象的几乎所有属性。同源策略同样适用于子窗体访问父窗体的 window 对象。跨域通信可以通过 [window.postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage) 来实现。

## [定位和缩放](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#%E5%AE%9A%E4%BD%8D%E5%92%8C%E7%BC%A9%E6%94%BE)

作为一个[可替换元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)， 可以使用 [object-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-position) 和 [object-fit](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit) 来定位、对齐、缩放 元素内的文档。

## [示例](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#%E7%A4%BA%E4%BE%8B)

### [一个简单的](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#example1)

下面的例子中，我们创建了一个框架。当用户点击框架中的按钮时，浏览器会弹出一个提示框。

#### HTML

```
<p>Your browser does not support iframes.</p> \#### Result ### \[在新标签页中打开里面的链接\](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#example2) 下面的例子演示了在框架中展示谷歌地图。 #### HTML <base target="\_blank"> <iframe id="Example2" title="Example2" width="400" height="300" frameborder="0" scrolling="no" marginheight="0" marginwidth="0" src="https://maps.google.com/maps?f=q&amp;source=s\_q&amp;hl=es-419&amp;geocode=&amp;q=buenos+aires&amp;sll=37.0625,-95.677068&amp;sspn=38.638819,80.859375&amp;t=h&amp;ie=UTF8&amp;hq=&amp;hnear=Buenos+Aires,+Argentina&amp;z=11&amp;ll=-34.603723,-58.381593&amp;output=embed">  
[See bigger map](https://maps.google.com/maps?f=q&source=embed&hl=es-419&geocode=&q=buenos+aires&sll=37.0625,-95.677068&sspn=38.638819,80.859375&t=h&ie=UTF8&hq=&hnear=Buenos+Aires,+Argentina&z=11&ll=-34.603723,-58.381593) \#### Result ## \[无障碍环境\](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#%E6%97%A0%E9%9A%9C%E7%A2%8D%E7%8E%AF%E5%A2%83) 使用 iframe 的 \[title\](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global\_attributes/title) 属性来标识框架的主要内容，这样可以极大方便使用辅助技术（例如屏幕阅读器）浏览网页的人。框架的标题应该清楚地描述框架的内容，例如： Copy to Clipboard 如果没有标题，他们就只能浏览每一个框架来获取需要的内容。这非常耗时间，也很容易让人迷惑，尤其是当页面中包含很多框架或者互动内容如音视频等的时候。
```
