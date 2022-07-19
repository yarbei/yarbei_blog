# Object.getOwnPropertyNames()

# Object.getOwnPropertyNames()

---


**`Object.getOwnPropertyNames()`**方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的数组。

## [语法](https://developer.mozilla.org/zh-cn/docs/web/javascript/reference/global_objects/object/getownpropertynames#Syntax)

Object.getOwnPropertyNames(*obj*)

## [参数](https://developer.mozilla.org/zh-cn/docs/web/javascript/reference/global_objects/object/getownpropertynames#%E5%8F%82%E6%95%B0)

`obj`一个对象，其自身的可枚举和不可枚举属性的名称被返回。

## [返回值](https://developer.mozilla.org/zh-cn/docs/web/javascript/reference/global_objects/object/getownpropertynames#%E8%BF%94%E5%9B%9E%E5%80%BC)

在给定对象上找到的自身属性对应的字符串数组。

## [描述](https://developer.mozilla.org/zh-cn/docs/web/javascript/reference/global_objects/object/getownpropertynames#Description)

`Object.getOwnPropertyNames()` 返回一个数组，该数组对元素是 `obj`自身拥有的枚举或不可枚举属性名称字符串。 数组中枚举属性的顺序与通过 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 循环（或 [`Object.keys`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)）迭代该对象属性时一致。数组中不可枚举属性的顺序未定义。

## [示例](https://developer.mozilla.org/zh-cn/docs/web/javascript/reference/global_objects/object/getownpropertynames#%E7%A4%BA%E4%BE%8B)

### [使用 Object.getOwnPropertyNames()](https://developer.mozilla.org/zh-cn/docs/web/javascript/reference/global_objects/object/getownpropertynames#%E4%BD%BF%E7%94%A8_Object.getOwnPropertyNames)

```other
var arr = ["a", "b", "c"];
console.log(Object.getOwnPropertyNames(arr).sort()); // ["0", "1", "2", "length"]
// 类数组对象
var obj = { 0: "a", 1: "b", 2: "c"};
console.log(Object.getOwnPropertyNames(obj).sort()); // ["0", "1", "2"]
// 使用Array.forEach输出属性名和属性值
Object.getOwnPropertyNames(obj).forEach(function(val, idx, array) {
  console.log(val + " -> " + obj[val]);
});
// 输出
// 0 -> a
// 1 -> b
// 2 -> c
//不可枚举属性
var my_obj = Object.create({}, {
  getFoo: {
    value: function() { return this.foo; },
    enumerable: false
  }
});
my_obj.foo = 1;
console.log(Object.getOwnPropertyNames(my_obj).sort()); // ["foo", "getFoo"]
```

如果你只要获取到可枚举属性，查看[`Object.keys`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)或用[`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)循环（还会获取到原型链上的可枚举属性，不过可以使用[`hasOwnProperty()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)方法过滤掉）。

下面的例子演示了该方法不会获取到原型链上的属性：

```other
function ParentClass() {}
ParentClass.prototype.inheritedMethod = function() {};
function ChildClass() {
  this.prop = 5;
  this.method = function() {};
}
ChildClass.prototype = new ParentClass;
ChildClass.prototype.prototypeMethod = function() {};
console.log(
  Object.getOwnPropertyNames(
    new ChildClass()  // ["prop", "method"]
  )
);
```

### [只获取不可枚举的属性](https://developer.mozilla.org/zh-cn/docs/web/javascript/reference/global_objects/object/getownpropertynames#%E5%8F%AA%E8%8E%B7%E5%8F%96%E4%B8%8D%E5%8F%AF%E6%9E%9A%E4%B8%BE%E7%9A%84%E5%B1%9E%E6%80%A7)

下面的例子使用了 [`Array.prototype.filter()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 方法，从所有的属性名数组（使用`Object.getOwnPropertyNames()`方法获得）中去除可枚举的属性（使用[`Object.keys()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)方法获得），剩余的属性便是不可枚举的属性了：

```other
var target = myObject;
var enum_and_nonenum = Object.getOwnPropertyNames(target);
var enum_only = Object.keys(target);
var nonenum_only = enum_and_nonenum.filter(function(key) {
    var indexInEnum = enum_only.indexOf(key);
    if (indexInEnum == -1) {
        // 没有发现在enum_only健集中意味着这个健是不可枚举的,
        // 因此返回true 以便让它保持在过滤结果中
        return true;
    } else {
        return false;
    }
});
console.log(nonenum_only);
```

注：[Array.filter(filt_func)方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。

## [提示](https://developer.mozilla.org/zh-cn/docs/web/javascript/reference/global_objects/object/getownpropertynames#Notes)

在 ES5 中，如果参数不是一个原始对象类型，将抛出一个 [`TypeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError)  异常。在 ES2015 中，非对象参数被强制转换为对象 **。**

```other
Object.getOwnPropertyNames('foo');
// TypeError: "foo" is not an object (ES5 code)
Object.getOwnPropertyNames('foo');
// ['length', '0', '1', '2']  (ES2015 code)
```
