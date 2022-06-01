## react项目打包时遇到Cannot assign to read only property 'exports' of object 'Object'问题的解决方法。

---

### abbrlink: 17

国外有一个大神对此分析的很清楚，他把原因分成以下两类：

![1597040040150-1619330e-8c80-4d39-a73b-910cee560427.png](react%E9%A1%B9%E7%9B%AE%E6%89%93%E5%8C%85%E6%97%B6%E9%81%87%E5%88%B0Cannot%20assign%20to%20read%20only%20property%20'exports'%20of%20object%20'Object'%E9%97%AE%E9%A2%98%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%E3%80%82.assets/1597040040150-1619330e-8c80-4d39-a73b-910cee560427.png)

简单翻译下,第一种原因就是import和module.exports的混用，要知道commonJS和ES6的语法是不太一样的前者是require和module.exports后者则是import和exports,当你混用这两个语法的时候，webpack就会报错，也就是第一种情况。为了使编译好的程序能在大多数浏览器下运行。

webpack里面有一个编译器叫Babel，负责把ES6的语言转化为commonJS以兼容绝大多数浏览器。当你混用这两个语法的时候你可以使用babel的commonJS模式帮你把import编译成require。

然而第二种情况就是你要使用@babel/plugin-transform-runtime这个插件的时候，同时你又在某个commonJS写的文件里使用这个插件时，babel会默认你这个文件是ES6的文件，然后就使用import导入了这个插件，从而产生了和第一种情况一样的混用错误。解决方法是在.babelrc里配置unambiguous设置，让babel和webpack一样严格区分commonJS文件和ES6文件。

```json
{
	"presets": [
		"react-app"
	],
	"sourceType": "unambiguous"
}
```

