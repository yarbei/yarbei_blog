## 查漏补缺！JS中数据类型的判断

---

### abbrlink: 11

面试季马上就要过去了，很多小伙伴也是在努力的抓住金九银十的小尾巴，今天来说一说面试中常见的一个问题：js中如何判断数据类型？

众所周知，JavaScript是一门弱类型的语言，它允许不兼容的类型进行运算，但有些场景使得我们不得不对数据类型做一个规范，如此就需要对数据类型进行判断。

可能你会回答，我一般用typeof来进行判断，可是面试官会问你，那如果遇到值为null的情况下typeof会返回什么？对于初入江湖的新手来说可能在这个时候就败下阵来，没错，我当初也是这样，判断数据为null或者undefined不是用if-else来判断的吗？

带着疑问，接下来我们就来看一下如何判断数据类型：

**typeof**

在此引用MDN的说明：

> typeof 操作符返回一个字符串，表示未经计算的操作数的类型。

下表总结了 `typeof` 可能的返回值：

![1603940156778-82be9fbe-9891-47d4-ac3f-853a3243df22.png](assets/1603940156778-82be9fbe-9891-47d4-ac3f-853a3243df22.png)

示例：

```javascript
console.log(typeof 333);             // number
console.log(typeof true);            // boolean
console.log(typeof 'aaa');           // string
console.log(typeof []);              // object     []数组的数据类型在 typeof 中被解释为 object
console.log(typeof function(){});    // function
console.log(typeof {});              // object
console.log(typeof undefined);       // undefined
console.log(typeof null);            // object     null 的数据类型被 typeof 解释为 object
```

```javascript
null// JavaScript 诞生以来便如此
typeof null === 'object';
```

在 JavaScript 最初的实现中，JavaScript 中的值是由一个表示类型的标签和实际数据值表示的。对象的类型标签是 0。由于 `null` 代表的是空指针（大多数平台下值为 0x00），因此，null 的类型标签是 0，`typeof null` 也因此返回 `"object"`。

曾有一个 ECMAScript 的修复提案（通过选择性加入的方式），但被拒绝了。该提案会导致 `typeof null === 'null'`。

错误：

在 ECMAScript 2015 之前，`typeof` 总能保证对任何所给的操作数返回一个字符串。即便是没有声明的标识符，`typeof` 也能返回 `'undefined'`。使用 `typeof` 永远不会抛出错误。

但在加入了块级作用域的 `let` 和 const 之后，在其被声明之前对块中的 `let` 和 `const` 变量使用 `typeof` 会抛出一个 ReferenceError。块作用域变量在块的头部处于“暂存死区”，直至其被初始化，在这期间，访问变量将会引发错误。

```other
typeof undeclaredVariable === 'undefined';
typeof newLetVariable; // ReferenceError
typeof newConstVariable; // ReferenceError
typeof newClass; // ReferenceError
let newLetVariable;
const newConstVariable = 'hello';
class newClass{};
```

**instanceof**

依然引用MDN的说明：

> instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。

示例：

```javascript
console.log(333 instanceof Number);                  // false
console.log(true instanceof Boolean);                // false 
console.log('aaa' instanceof String);                // false  
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```

以上结果显示，直接的字面量值判断数据类型，只有引用数据类型（Array，Function，Object）被精准判断，其他（Number，Boolean，String）字面值不能被instanceof精准判断。

需要注意的是，如果表达式 `obj instanceof Foo` 返回 `true`，则并不意味着该表达式会永远返回 `true`，因为 `Foo.prototype` 属性的值有可能会改变，改变之后的值很有可能不存在于 `obj` 的原型链上，这时原表达式的值就会成为 `false`。另外一种情况下，原表达式的值也会改变，就是改变对象 `obj` 的原型链的情况，虽然在目前的ES规范中，我们只能读取对象的原型而不能改变它，但借助于非标准的 `__proto__` 伪属性，是可以实现的。比如执行 `obj.__proto__ = {}` 之后，`obj instanceof Foo` 就会返回 `false` 了。

到这里大家可能已经观察到了，为什么我在上面的示例中没有用instanceof来判断null和undefined，我们在浏览器中输出一下看是什么结果：

![1603940156896-0d0014a2-0b0f-4287-9c3b-613459a53545.png](assets/1603940156896-0d0014a2-0b0f-4287-9c3b-613459a53545.png)

浏览器在这里报错了，它认为null，undefined不是构造器。

**Object.prototype.toString.call()**

最后，留到最后的才是最好的，我们来看一下终极大招：

Object.prototype.toString.call()

MDN中说明：

> toString() 方法返回一个表示该对象的字符串。

每个对象都有一个 `toString()` 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。默认情况下，`toString()` 方法被每个 `Object` 对象继承。如果此方法在自定义对象中未被覆盖，`toString()` 返回 "[object *type*]"，其中 `type` 是对象的类型。以下代码说明了这一点：

```javascript
var o = new Object();
o.toString(); // returns [object Object]
```

可以通过 `toString()` 来获取每个对象的类型。为了每个对象都能通过 `Object.prototype.toString()` 来检测，需要以 `Function.prototype.call()` 或者 `Function.prototype.apply()` 的形式来调用，传递要检查的对象作为第一个参数，称为 `thisArg`。

```javascript
var toString = Object.prototype.toString;toString.call(333);          // [object Number]
toString.call("aaa");        // [object String]
toString.call(true);         // [object Boolean]
toString.call([]);           // [object Array]
toString.call(function(){}); // [object Function]
toString.call({});           // [object Object]
toString.call(undefined);    // [object Undefined]
toString.call(null);         // [object Null]
// 甚至于js的内置对象也能被精确判断
toString.call(new Date);     // [object Date]
toString.call(new String);   // [object String]
toString.call(Math);         // [object Math]
```

相信到这里你已经知道该如何回答面试官的问题了吧

文末彩蛋

大家好，我是亚北，也就是博主各大粉丝群中的小助理，今天心血来潮想写一篇文章给老大来投稿，好吧，其实是她嫌我不思进取？可能吧。于是我翻了几道常见的面试题，最后选择了这道，可能很常见，但是也容易被忽略，比如要不是今天查资料，我就不知道Object.prototype.toString.call()还可以判断Date，Math等js内置对象类型。最后希望大家都可以在面试季中拿到自己心仪公司的offer，人均大厂，年薪百万！

跟着前端Q学前端，大厂始终近你一步！

![1603940156762-679c73b8-9449-438e-a670-30a8c6cd9ac2.png](assets/1603940156762-679c73b8-9449-438e-a670-30a8c6cd9ac2.png)

![1603940156921-c82759aa-9ffa-4bd1-9c63-ff91dcd4a3fa.png](assets/1603940156921-c82759aa-9ffa-4bd1-9c63-ff91dcd4a3fa.png)

![1603940156911-1782b310-942b-4e6d-bef6-1da674546504.png](assets/1603940156911-1782b310-942b-4e6d-bef6-1da674546504.png)

