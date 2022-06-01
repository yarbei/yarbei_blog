## arg和argument的区别

---

### abbrlink: 25

…args剩余参数(展开运算符)

允许一个表达式在某处展开。展开运算符在多个参数（用于函数调用）或多个元素（用于数组字面量）或者多个变量（用于解构赋值）的地方可以使用。剩余参数语法允许我们将一个不定数量的参数表示为一个数组。

​

```javascript
function sum(...theArgs) {
  return theArgs.reduce((previous, current) => {
    return previous + current;
  });
}
 
console.log(sum(1, 2, 3));
// expected output: 6
 
console.log(sum(1, 2, 3, 4));
// expected output: 10
```

描述

如果函数的最后一个命名参数以…为前缀，则它将成为一个数组，其中从0（包括）到theArgs.length（排除）的元素由传递给函数的实际参数提供。

​

用法

```javascript
function(a, b, ...theArgs) {
  // ...
}
```

在上面的例子中，theArgs将收集该函数的第三个参数（因为第一个参数被映射到a，而第二个参数映射到b）和所有后续参数。

​

函数调用中使用展开运算符

在以前我们会使用apply方法来将一个数组展开成多个参数：

​

```javascript
function test(a, b, c) { }
var args = [0, 1, 2];
test.apply(null, args);
```

如上，我们把args数组当作实参传递给了a,b,c，这边正是利用了Function.prototype.apply的特性。

​

不过有了ES6，我们就可以更加简洁地来传递数组参数：

​

```javascript
function test(a,b,c) { }
var args = [0,1,2];
test(...args);
```

我们使用…展开运算符就可以把args直接传递给test()函数。

​

数组字面量中使用展开运算符

在ES6的世界中，我们可以直接加一个数组直接合并到另外一个数组当中：

​

```javascript
var arr1=['a','b','c'];
var arr2=[...arr1,'d','e']; //['a','b','c','d','e']
```

展开运算符也可以用在push函数中，可以不用再用apply()函数来合并两个数组：

​

```javascript
var arr1=['a','b','c'];
var arr2=['d','e'];
arr1.push(...arr2); //['a','b','c','d','e']
```

用于解构赋值

解构赋值也是ES6中的一个特性，而这个展开运算符可以用于部分情景：

​

```javascript
let [arg1,arg2,...arg3] = [1, 2, 3, 4];
arg1 //1
arg2 //2
arg3 //['3','4']
```

展开运算符在解构赋值中的作用跟之前的作用看上去是相反的，将多个数组项组合成了一个新数组。

不过要注意，解构赋值中展开运算符只能用在最后：

​

```javascript
let [arg1,...arg2,arg3] = [1, 2, 3, 4]; //报错
```

​

类数组对象变成数组

展开运算符可以将一个类数组对象变成一个真正的数组对象：

​

```javascript
let a=new Set([1,2,3,4,5,2,1])  // a : Set(5) {1, 2, 3, 4, 5}
let b=[...a]    //  (5) [1, 2, 3, 4, 5]
```

ES7草案中的对象展开运算符

ES7中的对象展开运算符符可以让我们更快捷地操作对象：

​

```javascript
let {x,y,...z}={x:1,y:2,a:3,b:4};
x; //1
y; //2
z; //{a:3,b:4}
```

如上，我们可以将一个对象当中的对象的一部分取出来成为一个新对象赋值给展开运算符的参数。

同时，我们也可以像数组插入那样将一个对象插入另外一个对象当中：

​

```javascript
let z={a:3,b:4};
let n={x:1,y:2,...z};
n; //{x:1,y:2,a:3,b:4}
```

另外还要很多用处，比如可以合并两个对象：

​

```javascript
let a={x:1,y:2};
let b={z:3};
let ab={...a,...b};
ab //{x:1,y:2,z:3}
```

arguments 对象

在函数代码中，使用特殊对象 arguments，开发者无需明确指出参数名，就能访问它们。

​

例如，在函数 sayHi() 中，第一个参数是 message。用 arguments[0] 也可以访问这个值，即第一个参数的值（第一个参数位于位置 0，第二个参数位于位置 1，依此类推）。

​

因此，无需明确命名参数，就可以重写函数：

​

```javascript
function sayHi(message) {
  alert(arguments[0]);   // 此处将打印message参数的值
}
```

​

检测参数个数

还可以用 arguments 对象检测函数的参数个数，引用属性 arguments.length 即可。

​

下面的代码将输出每次调用函数使用的参数个数：

​

```javascript
function howManyArgs() {
  alert(arguments.length);
}

howManyArgs("string", 45);
howManyArgs();
howManyArgs(12);   //  上面这段代码将依次显示 "2"、"0" 和 "1"。
```

模拟函数重载

用 arguments 对象判断传递给函数的参数个数，即可模拟函数重载：

​

```javascript
function doAdd() {
  if(arguments.length == 1) {
    alert(arguments[0] + 5);
  } else if(arguments.length == 2) {
    alert(arguments[0] + arguments[1]);
  }
}
doAdd(10);	//输出 "15"
doAdd(40, 20);	//输出 "60"
```

当只有一个参数时，doAdd() 函数给参数加 5。如果有两个参数，则会把两个参数相加，返回它们的和。所以，doAdd(10) 输出的是 “15”，而 doAdd(40, 20) 输出的是 “60”。

​

…args剩余参数和 arguments对象的区别

剩余参数和 arguments对象之间的区别主要有三个：

​

> 1.剩余参数只包含那些没有对应形参的实参，而 arguments 对象包含了传给函数的所有实参。

> 2.arguments对象不是一个真正的数组，而剩余参数是真正的 Array实例，也就是说你能够在它上面直接使用所有的数组方法，比如 sort，map，forEach或pop。

> 3.arguments对象还有一些附加的属性 （如callee属性）。

[

]([https://blog.csdn.net/YauCheun/article/details/100396280](https://blog.csdn.net/YauCheun/article/details/100396280))

