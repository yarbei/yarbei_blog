
## 一、为什么如此难受🤔
在我们去写 JSX 的时候，有时候一直在感叹为什么在 JSX 里面写不了条件语句，举个栗子🌰（为什么都举我）：
```javascript
render () {     
  return (         
    <div>             
    {                    
      if(true) {                     
        <span>渲染我</span>                 
      } else {                     
        <span>不！要渲染我</span>                 
      }             
    }         
    </div>     
  ); 
} 
```
但是！这样写是不合法的，JSX 识别不了。在 JSX 中我们只能去写表达式，这种情况下我们就只能三元表达式去替代咯。
## 二、用什么去增强jsx呢？
JSX-Control-Statements 是一个Babel插件，它扩展了 JSX 以添加基本控制语句：条件和循环。
它是通过将类似组件的控制语句转换为它们的 JavaScript 对应语句来实现的：
```javascript
<If condition={condition()}>Hello World!</If>变为condition() ? 'Hello World!' :null。
```
唯一依赖 JSX-Control-Statements 依赖的是 Babel 。它与React和React Native兼容。
## 三、安装
PS：因为它是 Babel 的一个插件，所以我们在使用它的时候要先检查是否安装了babel

我们通过 npm 去安装 jsx-control-statements
```javascript
npm install --save-dev babel-plugin-jsx-control-statements
```

然后你只需要将 JSX-Control-Statements 指定为 Babel 插件，你通常会在你的插件中执行 .babelrc 。
```javascript
{   
  ...  
  "plugins": ["jsx-control-statements"] 
} 
```

如果你使用的 transform-react-inline-elements 插件，把它放在  jsx-control-statements 后边：
```javascript
{  
  ...   
  "plugins": ["jsx-control-statements", "transform-react-inline-elements"] 
}
```

## 四、语法
### IF标签
用于表示最简单的条件逻辑。
```javascript
// 如果condition的条件为真，则渲染if语句的主体 
// 在这里，这条语句会被转换成三元表达式， 
// { true ? <span>我显示咯</span> : null } 
<If condition={ true }>   
  <span>我显示咯</span> 
</If>
```

PS：不要使用 <Else /> ,因为它已经被废弃了。
### Choose标签
这是更高级的一种写法。
```javascript
// 这个会被转换成更复杂的三元表达式 
<Choose>   
  <When condition={ isShow1 }>     
    <span>显示我？</span>   
</When>   
<When condition={ isShow2 }>     
  <span>还是显示我？</span>   
</When>   
<Otherwise>     
  <span>都不满足那就显示我吧，我帅！</span>   
</Otherwise> 
</Choose>   
<Choose>   
  <When condition={true}>     
    <span>显示我！</span>   
</When> 
</Choose> 
```

#### <Choose>
作为一个简单的容器，只允许将 <when> 和 <otherwise> 作为子级，每个 <choose> 语句至少需要一个 <when> 块，<otherwise> 块是可选的。
#### <When>
类似于 <IF>
#### <Otherwise>
都不满足则渲染这个。
### For标签
```javascript
// 注意，这里面我们不要忘记key属性！！！ 
// 注意，<for>不能位于react中render函数的根节点！！！ 
<For each="item" of={ this.props.items }>     
  <span key={ item.id }>{ item.title }</span> 
</For> 
<For each="item" index="idx" of={ [1,2,3] }>     
  <span key={ idx }>{ item }</span>     
  <span key={ idx + '_2' }>Static Text</span> 
</For> 
```
### With标签
用于为局部变量赋值。
```javascript
// 可以用一条<With>语句分配多个变量。定义的变量仅在<With>块中可用。 
// 我们可以在<With>上定义多个属性 比如：foo={ 47 }  foo2={ 48 } foo3={ 49 }。 
// 我们也可以去嵌套使用。 
<With foo={ 47 }>   
  <span>{ foo }</span> 
</With> 
```

这块可能稍微的有点费解，大家看下转换后的代码就会清楚了：
```javascript
// 定义了局部变量，相当于一个单独的作用域 
{   
  (function(foo) {
    return <span>{ foo }</span>   
  }).call(this, 47) 
} 
```
## 五、主要版本

- 4.xx是一个纯Babel插件，支持Babel >= 7。
- 3.xx是一个支持Babel >= 6的纯Babel插件。
- 2.xx是一个支持Babel >= 6的Babel插件，以及JSTransform使用者。
- 1.xx是一个Babel插件，支持Babel <= 5，以及JSTransform使用者。

这曾经支持JSTransform和Babel，但由于不再维护JSTransform，所以不再支持它。你可以在 [github.com/alexgillera…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Falexgilleran%2Fjsx-control-statements-jsttransform) 上找到JSTransform版本的代码。
## 六、最后说一点
在我看来，这个插件会大大加大编译打包的时间以及代码量，如果没有特殊需求，可以不去使用这个🤗🤗🤗🤗。
