# react组件的 render 方法

一个组件类必须要实现一个 render 方法，这个 render 方法必须要返回一个 JSX 元素。必须要用一个外层的 JSX 元素把所有内容包裹起来,返回并列多个 JSX 元素是不合法的.

错误的写法：

```
render () {
return (

<div>第一个</div>
<div>第二个</div>

)
}
```

正确的写法：

```
render () {
return (

<div>
<div>第一个</div>
<div>第二个</div>
</div>

)
}
```



表达式插入

在 JSX 当中你可以插入 JavaScript 的表达式，表达式返回的结果会相应地渲染到页面上。
要求： 表达式用 {} 包裹。

```
render () {
const word = 'is good'
return (

<div>
<h1>React 小书 {word}</h1>
</div>

)
}
```

可以用函数表达式来返回：

```
render () {
return (

<div>
<h1>React 小书 {(function () { return 'is good'})()}</h1>
</div>

)
}
```

{} 内可以放任何JavaScript代码。render都会把这些代码返回的内容如实渲染在页面上。

{}还可以用在标签的属性上. eg：

```
render () {
 const className = 'header'
 return (
 <div className={className}>
 <h1>React 小书</h1>
 </div>
 )
 }
```

直接使用 class 在 React.js 的元素上添加类名如<div class=“xxx”> 这种方式是不合法的。
 还有一个特例就是 for 属性，例如<label for='male'>Male</label>，因为 for 也是 JavaScript 的关键字，所以在 JSX 用 htmlFor 替代，即 <label htmlFor='male'>Male</label>.

条件返回

```
render () {
 const isGoodWord = true
 return (
 <div>
 <h1>
 React 小书
 {isGoodWord
 ? <strong> is good</strong>
 : <span> is not good</span>
 }
 </h1>
 </div>
 )
 }
```

上面的代码中定义了一个 isGoodWord 变量为 true，下面有个用 {} 包含的表达式，根据 isGoodWord 的不同返回不同的 JSX 内容。

如果返回 null ，那么 React.js 会什么都不显示，相当于忽略了该表达式插入。

```
render () {
 const isGoodWord = true
 return (

 <div>
 <h1>
 React 小书
 {isGoodWord
 ? <strong> is good</strong>
 : null
 }
 </h1>
 </div>

 )
 }
 JSX 元素变量

render () {
 const isGoodWord = true
 const goodWord = <strong> is good</strong>
 const badWord = <span> is not good</span>
 return (
 <div>
 <h1>
 React 小书
 {isGoodWord ? goodWord : badWord}
 </h1>
 </div>
 )
 }
```

这里给把两个 JSX 元素赋值给了 goodWord 和 badWord 两个变量，然后把它们作为表达式插入的条件返回值。达到效果和上面的例子一样，随机返回不同的页面效果呈现。

```
renderGoodWord (goodWord, badWord) {
 const isGoodWord = true
 return isGoodWord ? goodWord : badWord
 }

render () {
 return (
 <div>
 <h1>
 React 小书
 {this.renderGoodWord(
 <strong> is good</strong>,
 <span> is not good</span>
 )}
 </h1>
 </div>
 )
 }
```













