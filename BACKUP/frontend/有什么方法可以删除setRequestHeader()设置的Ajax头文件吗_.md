# 有什么方法可以删除setRequestHeader()设置的Ajax头文件吗_

# 有什么方法可以删除setRequestHeader()设置的Ajax头文件吗_

---

## abbrlink: 15

我正在编写一个JS脚本，该脚本位于一个系统中，该系统现在通过jqXHR.setRequestHeader()向所有Ajax请求添加一个自定义的XID头，该函数注册于jQuery.ajaxPrefilter()。

在我的脚本中，我需要向第三方站点发出Ajax请求，它的访问控制允许- header没有设置为允许这个自定义的XID头。

看来我有三个选择

1. 找到一种方法来删除$.ajax()的前一个函数中的XID头，我已经确认它在ajaxPrefilter()函数之后被调用。
2. 完全放弃使用$. Ajax()，只需要自己编写Ajax。
3. 有$.ajax()指向系统的后端，它接受定制的XID头，并通过cURL向第三方站点执行请求。

如果有一种简单的方法，选择#1为首选，因为它是最干净、最快的路径。只是不知道怎么做。

## 4 个解决方案

---

我知道这是一条老路，但它仍然是个问题!希望以下内容能够帮助我们解决这个问题。

设置完报头后，将其设置为null/undefined/""不会删除它，它仍然发送报头，但没有值或“未定义”或“null”。这可能从来都不是您想要的，在我们的例子中，当它是授权头时，完全是螺丝SSO。

我们提出的解决方案依赖于@marlar的建议(感谢)和jQuery的另一个缺陷:您只能有一个bepreend()钩子，任何新钩子取代了之前的钩子。如果您在一个特定的$$.ajax()请求中提供了be前述()，那么看起来也是如此——它将覆盖$$. ajaxsetup()中的任何默认值。通过这样做，可以防止默认的钩子设置标题。(它不是100%理想的，因为在前面的缺省值()中可能还有其他事情不会发生，但是嘿)。

​

因此，这个处理了静态和动态设置的头，用于特定的请求:

```javascript
if($.ajaxSettings && $.ajaxSettings.headers) {
    delete $.ajaxSettings.headers.Authorization;
}
$.ajax({
    beforeSend: function() {}, // no op
    // ...
});
```

您不能对prefilter()执行相同的操作，但是我认为无论如何，前面()是执行此操作的更常见的方法。

---

```javascript
$.ajaxSetup({     beforeSend: function(jqXHR, settings) {         jqXHR.setRequestHeader('custom-xid-header', null);     } })
```

see [http://api.jquery.com/jQuery.ajaxSetup/](https://www.itdaan.com/link/aHR0cDovL2FwaS5qcXVlcnkuY29tL2pRdWVyeS5hamF4U2V0dXAv)

参见http://api.jquery.com/jQuery.ajaxSetup/

---

重叠看起来并不管用，但有一个简单的解决办法。

```javascript
$.ajaxSetup({
        beforeSend: function (jqXHR) {
            if (sendToken == true)
                jqXHR.setRequestHeader("Authorization", cookie);
            return true;
        }
    });
```

检查sendToken变量。这是全球性的。在我的应用开始的时候，我总是把它设为true因为我几乎在任何地方都需要标题集。

但是，当我不想发送“授权”头时，我只是将全局sendToken传递给false，然后才可以进行任何请求。当所有请求都完成时，我只是简单地将其设置为true…

​

诀窍在于，jqXHR变量是本地的，不会每次都保持指定的值。在ajaxSetup中覆盖当前值集不起作用，因为它总是添加新值，而不是替换它。我认为最简单、最有效的方法就是有条件。

​

它为我工作。

---

这将删除$. ajaxsetup()的头集。我想这也适用于前面的()。

```javascript
delete $.ajaxSettings.headers["custom-xid-header"]
```
