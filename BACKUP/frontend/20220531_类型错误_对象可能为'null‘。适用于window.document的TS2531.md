
第一次将Typescript添加到我的项目中。
在一个地方，我曾经使用window.document.getElementById访问过一些东西。它给出了这个错误。
```javascript
Type error: Object is possibly 'null'.  TS2531
```

TS正在做它的工作，并告诉你window.document.getElementById("foobar")可以返回一些null类型的东西。
如果您完全确定DOM中确实存在#foobar元素，则可以使用!操作符向TS显示您的信心。

```javascript
// Notice the "!" at the end of line 
const myAbsolutelyNotNullElement = window.document.getElementById("foobar")!
```
或者，您可以添加一个运行时可空检查，以使TS满意
```javascript
const myMaybeNullElement = window.document.getElementById("foobar") myMaybeNullElement.nodeName // <- error! 
if (myMaybeNullElement === null) {   alert('oops'); } else { 
  // since you've done the nullable check   
  // TS won't complain from this point on   
  myMaybeNullElement.nodeName // <- no error 
}
```
