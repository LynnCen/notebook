[TOC]



## 第2章 let and const 

#### 2.1 let命令

不存在变量提升，必须在声明后使用。

暂时性死区：在代码块内，使用let命令声明变量之前，该变量都是不可使用的。

不允许重复声明。

#### 2.2 块级作用域

取代了立即执行函数(IIFE);

######  块级作用域和函数声明

允许在块级作用域中声明函数

函数声明类似var，即会提升到全局作用域或函数作用域的头部

函数声明还会提升到所在块级作用域的头部。

尽量在写成函数表达式的形式，而不是函数声明语句。

```javascript
//函数声明语句
{
let a = 'cat';
function f(){
return a;
}
}

//函数表达式
{
let a = 'dog';
let f = fucntion (){
alert(a);
}
}


```



#### 2.3 const命令

基本用法：const声明一个只读的常量。一旦声明，常量的值就不能改变。

const命令声明的常量也不会提升，同样存在暂时性死区，只能在声明后使用且不可重复声明。

本质：const实际上保证的不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于简单类型的数据（数值，字符串，布尔值），值就保存在变量指向的内存地址中，因此等同于常量。但是对于复杂类型的数据（主要是对象和数组），变量就指向的内存地址保存的只是一个指针，const只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，这完全不受控制。

Object.freeze ——冻结对象

es6声明变量的6中方法——var function let const import class



## 第3章 变量的解构与赋值

#### 3.1数组的解构赋值

基本使用：

```javascript
let [head,...tail] = [1,2,3,4];
head;//1
tail;//[2,3,4]
```

指定默认值：

```javascript
let [foo = true] = [];
foo;//true
```

#### 3.2 对象的解构赋值

对象解构赋值和数组解构赋值有一个不同：数组的元素是按次序排列的，变量的取值是由它的位置决定的；而对象的属性没有次序，变量必须与属性同名才能取到正确的值。

```javascript
let {bar,foo}= {foo:'aaa',bar:'bbb'};
foo//'aaa'
bar//'bbb'

let {baz} = {foo:'aaa',bar:'bbb'};
baz//undefined
```

指定默认值：

```javascript
var {x = 3} = {};
x//3
//默认值生效的条件是：对象属性值严格等于undefined
```

#### 3.3 字符串的解构赋值

字符串也可以解构赋值，因此字符串被转换成一个类似数组的对象

```javascript
const [a,b,c,d,e] = 'hello';
a//'h'
b//'e'
c//'l'
d//'l'
e//'o'
```

#### 3.4 数值和布尔值的解构赋值

解构赋值时，如果等号的右边是数值和布尔值。则会先转为对象

#### 3.5 函数参数的解构赋值



#### 3.6 用途

**交换变量的值**

```javascript
let x= 1;
let y=2;
[x,y] = [y,x];
```

**从函数中返回多个值**

```javascript
function example(){
return([1,2,3])
}
let [a,b,c]=example();

//返回一个对象
function example(){
return{
foo:1,
bar:2
}
}
let {foo,bar} = example();
```

**函数参数的定义**

```javascript
//参数是一组有次序的值
function f([x,y,z]){...}
f([1,2,3]);
//参数是一组无次序的值
function f({x,y,z}){...}
f({z:3,y:2,x:1})
```

**提取JSON数据**

```javascript
let jsonData = {
id:22,
status:'ok',
data:[213,323]
}
let {id,status,data:number} = jsonData
```

**函数参数的默认值**

```javascript
jquery.ajax = function(
url,{
async = true,
beforeSend = function(){},
cache = true,
complete = function(){},
crossDomain = false,
global = true,
//...more
}){
//...do
}
```

**遍历Map结构**

任何部署了iterator接口的对象都可以使用for of遍历循环。

```javascript
var map = new Map();
map.set('first','hello');
map.set('second','world');

for(let [key,value] of map){
console.log(key + 'is' + value)
}
//first is hello
//second is world
```

**输入模块的指定方法**

```javascript
const {SoourceMapconsumer,SourceNode} = require('source-map');
```



## 第4章 字符串的拓展

#### 4.1 字符的Unicode表示法



#### 4.2 codePointAt()

能够正确处理4个字符存储的字符，返回一个字符的码点。这个方法定义在字符串的实例对象上。

参数：字符在字符串中的位置

#### 4.3 String.fromCodePoint()

用于从码点返回对应字符，但是这个方法不能识别32位的UTF-16字符。这个方法定义在String对象上。

#### 4.4 字符串的遍历器接口

字符串可由 for...of循环遍历

```javascript
for (let codePoint of 'foo'){
console.log(codePoint)
}
//'f'
//'o'
//'o'
```

#### 4.5 at()

es5对字符串对象提供了charAt方法，返回字符串给定位置的字符。该方法不能识别码点大于oxFFFF的字符.而at方法则可以。

```javascript
'abc'.charAt(0);//'a'
'吉'.charAt(0);//'\uD842'
'吉'.charAt(0);//'吉'
```

#### 4.6 normalize()





#### 4.7 includes()、statsWith()、endsWith()

includes()：返回布尔值，表示是否找到了这个参数字符串。

statsWith()：返回布尔值，表示参数字符串是否在源字符串的头部。

endsWith()：返回布尔值，表示参数字符串是否在源字符串的尾部。

```javascript
var s = 'hello world!';

s.startsWith('hello');//true
s.endsWith('!');//true
s.includes('o');//true
```

第二个参数表示开始搜索的位置

#### 4.8 repeat()

repeat()：返回一个新的字符串，表示将源字符串重复n次

#### 4.9 padStart() 、padEnd()

如果某字符串不够指定长度，会在头部或尾部不全。padStatr()用于头部补齐，padEnd()用于尾部补齐。

```
'x'.padStart(5,'ab') // 'ababx'
'x'.padEnd(5,'ab')//'xbaba'
```

第一个参数指定字符串的最小长度，第二个参数用来补全的字符串

#### 4.10 模板字符串

模板字符串是增强版的字符串，用反引号`标识，它也可以做普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

```javascript
`hello ${name},how are you${time}?`
```

如果在模板字符串中需要使用反引号，则在其前面要用反斜杠来转义

```javascript
var greeting = `\`yo\`world!`
```

模板字符串中还能调用函数

```javascript
function fn(){}
`foo ${fn()} bar`
```

#### 4.11 实例：模板编译 



#### 4.12 标签模板



#### 4.13 String.raw()

用来充当模板字符串的处理函数，返回一个反斜线都被转移(即反斜线前面再加一个反斜线)的字符串，对应于替换变量后的模板字符串。



#### 4.14 模板字符串的限制



## 第5章 正则的拓展







## 第6章 数值的拓展

#### 6.1 二进制和八进制的表示法



#### 6.2 Number.isFinite() 、Number.isNaN()

**Number.isFinite()**:用来检查一个数值是否为有限的(finite).

**NUmber.isNaN()**：用来检查一个值是否为NaN.



#### 6.3 Number.parselnt()、Number.parseFloat()

```javascript
Number.parselnt('12.34');//12
Number.parseFloat('123.34#');//123.34
```

#### 6.4 Number.islntteger()

**Number.islntteger()** —— 用来判断一个值是否为整数。

#### 6.5 Number.EPSILON()

**Number.EPSILON()** —— 新增一个极小的常量

#### 6.6 安全整数和Number.isSafeinteger()

新引入了 Number.MAX_SAFE_INTEGER 和 Number.MIN_INTEGER 两个常量来表示整数的范围。

**Number.isSafeinteger()**  —— 用来判断一个整数是否落在这个范围之内。

#### 6.7 Math对象的拓展

###### 6.7.1 Math.trunc()

**Math.trunc()**  —— 用于去除一个数的小数部分，返回整数部分，对于非数值，其内部使用Number方法将其先转为数值，对于空值和无法截取整数的值，返回NAN

###### 6.7.2 Math.sign()

**Math.sign()** —— 用来判断一个数到底是正数，负数，还是零。对于非数值会先将其转为数值。

返回值的5种情况：

参数为正数，返回+1；

参数为负数，返回-1；

参数为0，返回0；

参数为-0，返回-0；

其他值，返回NaN；

###### 6.7.3 Math.cbrt()

**Math.cbrt()** —— 用于计算一个数的立方根

###### 6.7.4 Math.clz32()



###### 6.7.5 Math.imul()

**Math.imul()**  —— 返回两个数以32位带符号整数形式相乘的结果，返回也是一个32位的带符号整数。









## 第7章 函数的拓展

#### 7.1 函数参数的默认值

###### 7.1.1 基本用法

```javascript
function Person(x = 0,y = 0){
this.x = x;
this.y = y;
}
var p = new Person();
p//{x:0,y:0}
```

**note：**

① 参数变量是默认生命，所以不能用let或const再次声明

② 使用参数默认值时，函数不能有同名参数。

###### 7.1.2 与解构赋值默认值结合使用

```javascript
function foo ({x,y = 5}){
console.log(x,y)
}
foo({}) //undefiend,5
foo({x:1}) // 1,5
foo()// TypeError:cannot read property 'x' of undefiend
```

不能够省略第一个参数，改进：

```javascript
function fetch(url,{method = 'GET'} = {}){
console.log(method)
}
fetch('http://example.com');//'GET'
```

###### 7.1.3 参数默认值的位置

通常定义了默认值的参数应该是函数的尾参数，如果非尾部的参数设置默认值，实际上这个参数就是无法省略的。

```javascript
function f(x = 1,y){
return [x,y]
}
f()//[1,undefiend]
```

###### 7.1.4 函数的length属性

指定默认值以后，函数的length属性将返回没有指定默认值的参数个数。也就是说指定默认值之后，length属性将失真。

```javascript
(function(a){}).length //1
(function(a = 5){}).length //0 
(function (a,b,c= 5){}).lenght //2
```

rest参数也不会计入length属性

```javascript
(function(..args){}).lenght//0
```

如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数

```javascript
(function (a=0,b,c){}).lenght //0
```

###### 7.1.5 作用域

一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域。等到执行结束，这个作用域就会消失。这种语法在不设置默认参数是不会出现的。

```javascript
var x = 1;
function f(x,y = x){
conmsole.log(y)
}
f(2)//2
```

```javascript
let x = 1;
function f(y= x){
let x = 2;
console.log(y)
}
f();//1
```

###### 7.1.6 应用

#### 7.2 rest参数

形式为 （...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了，rest参数搭配的变量时一个数组，该变量将多余的参数放入其中。

#### 7.3 严格模式



#### 7.4 name属性

函数的name属性返回该函数的函数名

```javascript
function foo(){}
foo.name //'foo'
```

#### 7.5 箭头函数

###### 7.5.1 基本使用

```javascript
var f = v => v;
var f = ()=> 5;
//简化回调函数
arr.map(x => x*x);
//rest参数和箭头函数结合
const numbers = (..nums)=> nums;
const headAndTail = (head,...tail) => [head,tail];
headTail(1,2,3,4,5);//[1,[2,3,4,5]]
```

###### 7.5.2 注意事项

① 函数体内的this对象就是定义所在的对象，而并不是使用时所在的对象。

② 不可以当作构造函数。也就是说，不可以使用new命令，否则会抛出一个错误。

③ 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用rest参数代替。

④ 不可以使用yield命令，因此箭头函数不能用作Generator函数。

this对象的指向是可变的，但在箭头函数中它是固定的。

```javascript
function foo(){
setTimeout(() => {
console.log('id',this.id);
},100)
}
var id = 21;
foo.call({id:42});
//id:42 如果是普通函数，执行时this应该指向全局对象window，这时应该输出21，但是箭头函数导致this总是指向函数定义生效时所在的对象(本例是{id：42})，所以是42.
```

箭头函数可以让setTimeout里面的this绑定定义时所在的作用域，而不是指向运行时所在的作用域。

```javascript
function Timer(){
this.s1 = 0;
this.s2 = 0;
//箭头函数
setInterval(() => this.s1++,1000);
//普通函数
setInterval(function(){
this.s2++;
},1000);
}

var timer = new Timer();
setTimeout(() => console.log('s1:',timer.s1),3100);
setTimeout(() => console.log('s2:',timer.s2),3100);
//s1:3
//s2:0
```

上面的代码中，Timer函数内部设置了两个定时器，分别使用箭头函数和普通函数。前者的this绑定定义时所在的作用域(即Timer函数)，后者的this指向运行所在的作用域(即全局对象)。所以3100ms后，timer.s1 被更新了3次，而timer.s2一次都没更新。



箭头函数可以让this指向固定化，这种特性非常有利于封装回调函数。下面是一个例子，DOM事件的回调函数封装在一个对象里面。

```javascript
var hander = {
id:'123456',
init:function(){
 document.addEventListener('click',
 e => this.doSomething(e.type),fasle
 )
},
doSomething:function(){

}
}
```

以上代码的init方法中使用了箭头函数，这导致箭头函数里面的this总是指向handler对象。否则，回调函数运行时，this.doSomething一行会报错，因此此时this指向document对象。



this指向的固定化并不是箭头函数有绑定this的机制，实际原因时箭头函数根本没有自己的this，导致内部的this就是外层的代码块this。正因为它没有自己的this，所以不能用作构造函数。

除了this。一下3个变量在箭头函数中也是不存在的，分别指向外层函数的对象变量arguments，super和new.target.



###### 7.5.3 嵌套的箭头函数



#### 7.6 绑定this

箭头函数可以绑定this对象，大大减少了显式绑定this对象的写法(call，apply，bind）

函数绑定运算符时并排的双冒号(::),冒号左边是一个对象，右边是一个函数。该运算符会自动的将左边的对象作为执行的上下文环境(即this对象)，绑定到右边的函数上。es7提案！

#### 7.7 尾调用优化



#### 7.8 函数参数的尾逗号



## 第8章 数组的拓展

#### 8.1 拓展运算符

###### 8.1.1 含义

拓展运算符是三个点（...），它如同rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。

```javascript
console.log(...[1,2,3]);//1,2,3
console.log(1,...[2,3,4],5);/1,2,3,4,5
```

该运算符主要用于函数调用。

```javascript
function push(array,...item){
array.push(...item)
}
function add(x,y){
return x + y;
}
var numbers = [4,38];
add(...numbers);//42
```

与正常参数使用。

```javascript
function f(v,w,x,y,z){}
var args = [0,1];
f(-1,...args,2,...[3]);
```

后面还可以放置表达式

```javascript
const arr = [...(x > 0?['a']:[]),'b']
```

###### 8.1.2 替代数组的apply方法

```javas
function f(x,y,z){
//...
}
var args = [0,1,2];
f(...args);
```

将一个数组添加到另一个数组的尾部

```javascript
var arr1 = [0,1,2];
var arr2 = [3,4,5];
arr1.push(...arr2)
```

###### 8.1.3 拓展运算符的运用

**合并数组**

```javascript
//es5
[1,2].concat(more);
//es6
[1,2,...more]
[...arr1,...arr2,...arr3];
```

**与解构赋值结合**

拓展运算符可以与解构赋值结合起来用于生成数组。

```
[a,...rest] = [1,2,3,4,5];
a//1
rest//[2,3,4,5]
```

note:如果用于数组赋值，则只能将其放在参数的最后一位，否则会报错。

**函数的返回值**

```javascript
var dateFields = readDateFields(database);
var d = new Date(...dateFields)
```

**字符串**

可以将字符串转为真正的数组

```javascript
[...'hello']
//['h','e','l','l','o']
```

**实现了iterator接口的对象**

任何iterator接口的对象都可以用拓展运算符转为真正的数组。

```javascript
var nodelist = document.querSelectorAll('div');
var array = [...nodelist]
```

**Map和Set结构，Generator函数**

拓展运算符内部调用的是数据结构的Iterator接口，因此只要具有Iterator接口的对象，都i可以使用拓展运算符，如Map结构。

```javascript
let map = new Map([
[1,'one'],
[2,'two'],
[3,'three']
])
let arr = [...map.keys()];//[1,2,3]
```

Generator函数运行后会返回一个遍历器对象，因此可以使用拓展运算符。

```javascript
var go = function *(){
yield 1;
yield 2;
yield 3;
}
[...go()] //[1,2,3]
```

note：没有Iterator接口的对象，使用会报错。

#### 8.2 Array.from()

用于将两类对象转化为真正的数组：类似数组的对象和可遍历对象

```javascript
let arrayLike = {
'0':'a',
'1':'b',
'2':'c',
length:3
}

let arr2 = Array.from(arrayLike);//['a','b','c']
```

只要部署了Iterator接口的数据结构，该方法都能将其转为数组。

```javascript
Array.from('hello');
//['h','e','l','l','o']

let nameSet = new Set(['a','b']);
Array.from(nameSet) //['a','b'];
```

拓展运算符也可以将某些数据转为数组（前面有提到过）

拓展运算符背后调用的是遍历器接口(Symbol.iterator),如果一个对象没有部署该接口，就无法转化。

Array.from()还支持类似数组的对象，所谓的类数组就是，即必须有length属性。因此，任何具有length属性的对象，都可以通该方法转为数组，而拓展运算符就没法做到。

```javascript
Array.from({length:3});//[undefined,undefined,undefined]
```

接收第二个参数，作用类似数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组中。

```javascript
Array.from([1,2,3],x => x*x);//[1,4,9]
//取出DOM节点的文本内容
let spans = document.querySelectorAll('span.name')
let names1 = Array.from(spans,s => s.textContent)
```

如果map函数中用到了this关键字，还可以传入Array.from第三个参数，用来绑定this。



#### 8.3 Array.of()

用于将一组值转换为数组。

```javascript
Array.of(3,11,8) //[3,11,8]
Array.of(3)//[3]
```

Array.of基本上可以用来替代Array()或new Array()，并且不存在参数不同导致的重载。它的行为非常统一。

#### 8.4 数组实例的copyWithin()

会在当前数组内部将指定位置的成员复制到其他位置(会覆盖原有成员)，然后返回当前数组。也就是说，这个方法会修改当前数组。

```javascript
Array.prototype.copyWithin(target,start = 0,end = this.length)
```

接收三个参数

target（必选）：从该位置开始替换数据。

start（可选）：从该位置开始读取数据，默认为0。如果为负数，表示倒数。

end（可选）：到该位置前停止读取数据，默认等于数据长度。如果为负数，表示倒数。

这三个参数都应该是数值，如果不是，则会自动转为数值。

```javascript
//将3号位复制到0号位
[1,2,3,4,5].copyWithin(0,3,4)
//[4,2,3,4,5]
```

#### 8.5 数组实例的find()和findIndex()

find()——用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，知道找出第一个返回值为true的成员，然后返回该成员，如果没有符合条件的成员，则返回undefined。

```javascript
[1,5,10,15].find((item,index,arr) => {
return item > 9
}) // 10
```

findIndex() —— 与find方法类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1

```javascript
[1,5,10,15].findIndex((item,index,arr)=> {
return item > 9
})//2
```

这两个方法都可以接受第二个参数，用来绑定回调函数的this对象。另外还可以发现NaN，弥补了数组的indexOf方法的不足。

#### 8.6 数组实例的fill()

使用给定值填充一个数组。

```javascript
['a','b','c'].fill(7) //[7,7,7]
['a','b','c'].fill(7,1,2) //['a',7,'c']
```

#### 8.7 数组实例的entries()、keys()和values()

用于遍历数组。它们都返回一个遍历器对象，可用for...of循环遍历，唯一的区别在于，keys()是对**键名**的遍历，values()是对**键值**的遍历，entries()是对**键值对**的遍历

```javascript
for(let index of ['a','b'].keys()){
console.log(index)
}
//0
//1
for(let elem of ['a','b'].values()){
    console.log(elem)
}
//'a'
//'b'
for(let [index,elem] of ['a','b'].entries()){
    console.log(index,elem)
}
//0 'a'
//1 'b'
```

如果不使用for...of循环，可以手动调用遍历器对象的**next**方法进行遍历

#### 8.8 数组实例的includes()

该方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。

```javascript
[1,2,3].includes(2) //true
[1,2,NaN].includes(NaN) //true
```

没有该方法，通常使用indexOf方法检查是否包含某个值。有两个缺点，一是不够语义化，其含义是找到参数值的第一个出现的位置，所以要比较是否不等于-1，表达起来不够直观；二是，其内部使用严格相等运算符（===）进行判断，会导致对NaN的误判。

另外，Map和Set数据结构有一个has方法，需要注意与includes区分。

Map结构的has方法是用来查找键名的，比如Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target,propertyKey)。

Set结构的has方法是用来查找值得，比如Set.prototype.has(value)、WeakSet.prototype.has(value)

#### 8.9 数组的空位

数组的空位是指数组的某一个位置没有任何值。比如Array构造函数返回的数组都是空位

```
Array(3)//[,,,]
```

空位不是undefined，一个位置的值等于undefined依然是有值的。空位是没有任何值，in运算符可以说明这一点。

```javascript
0 in [undefined,undefined,undefined] //true
0 in [,,,] //false
```

forEach()、filter()、every()和some()都会跳过空位。

map()会跳过这个空位，但会保留这个值。

join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串。



## 第9章 对象的拓展

#### 9.1 属性的简洁表示法

允许直接写入变量和函数作为对象的属性和方法。

```javascript
var foo = 'bar';
var baz = {foo}
baz //{foo:'bar'}
```

允许只写属性名，不写属性值。这时，属性值等于属性名所代表的变量。

```javascript
function f(x,y){
return {x,y}
}
```

方法简写：

```javascript
var o ={
method(){
return 'hello'
}
}
```

commJS模块输出变量就非常适合使用简洁写法

```javascript
module.exports = {getItem,setItem,clear}
```

属性赋值器（setter）和取值器（getter）事实上也 采用了这种写法。

#### 9.2 属性名表达式

```javascript
let propKey = 'foo';
let obj = {
[propKey]:true,
['a' + ''bc]:123,
['h' + 'ello'](){
    return 'hi'
}
}
```

#### 9.3 方法的name属性

函数的name属性返回函数名。对象方法也是函数，因此也有name属性。

```javascript
const person = {
sayName(){
console.log('hello')
}
}
person.sayName.name //'sayName'
```

#### 9.4 Object.is()

 ES5比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。前者会自动转换数据类型，后者NaN不等于自身，以及+0等于-0。

Object.is() —— 它用来比较两个值是否严格相等，与严格相等运算符的行为一致。

#### 9.5 Object.assign()

###### 9.5.1 基本用法

用于将源对象的所有可枚举属性复制到目标对象。第一个参数就是目标对象，后面的参数都是源对象。

```javascript
var target = {a:1};
var source1 = {b:2};
var source2 = {c:3};

Object.assign(target,source1,source2);
target//{a:1,b:2,c:3}
```

note：如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

如果只有一个参数，会直接返回该参数。

如果该参数不是对象，则会先转成对象，然后返回。

由于undefined和null无法转成对象，所以如果将他们作为参数，则会报错。

###### 9.5.2 注意点

Object.assign方法执行的是浅复制，而不是深复制。如果源对象某个属性的值是对象，那么目标对象得到这个对象的引用。

```javascript
var obj1 = {a:{b:1}};
var obj2 = Object.assign({},obj1);
obj1.a.b = 2;
obj2.a.b // 2
```

###### 9.5.3 常见用途

**为对象添加属性**

```javascript
calss Point {
constructor(x,y){
Object.assign(this,{x,y})
}
}
```

assign方法将x属性和y属性添加到了Point类的对象实例中。

**为对象添加方法**

```javascript
Object.assign(SomeClass.prototype,{
someMethod(arg1,arg2){

}
})
```

**克隆对象**

```javascript
function clone(orgin){
return Object.assign({},orgin);
}
```

将原始对象复制到一个空对象中，就得到了原始对象的克隆。

不过，采用这种方法只能克隆原始对象自身的值，不能克隆它继承的值。如果想要继承原型链，采用如下代码：

```javascript
function clone(origin){
let originProto = Object.getPrototypeOf(origin);
return Object.assign(Object.create(originProto),origin)
}
```

**合并多个对象**

将多个对象合并到某个对象

```javascript
const  merge = (target,..sources)=> Object.assign({},...sources);
```

**为属性指定默认值**



#### 9.6 属性的可枚举属性

对象的每一个属性都具有一个描述对象（Descriptor），用于控制该属性的行为。

Object.getOwnPropertyDescriptor方法可以获取该属性的描述对象。

```javascript
let obj = {foo:123};
Object.getOwnPropertyDescriptor(obj,'foo')
//{
//value:123,
//writable:true,
//enumerable:true,
//configurable:true
}
```

ES5有三个操作会忽略enumerable为false的属性。

for...in属性：只遍历对象自身的和继承的可枚举属性。

Object.keys():返回对象自身的所有可枚举属性的键名。

JSON.stringify()：只串行化对象自身的可枚举属性。

ES6 Object.assign()也会忽略enumerable为false的属性，只复制对象自身的可枚举属性。

ES6 所有的Class的原型的方法都是不可枚举的

#### 9.7 属性的遍历

###### **1.for...in**

循环遍历对象自身的和继承的可枚举属性（不含Symbol属性）。

###### 2.Object.keys(obj)

返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含Symbol属性）。

###### 3.Object.getOwnPropertyNames()

返回一个数组，包含对象自身的所有属性（不含Symbol属性，但是包括不可枚举属性）

###### 4.Object.getOwnPropertySymbols(obj)

返回一个数组，包含对象自身的所有所有Symbol属性。

###### 5.Reflect.ownKeys(obj)

返回一个数组，包含对象自身的所有属性，不管属性名是Symbol还是字符串，也不管是否可枚举。

#### 9.8 _proto_属性、Object.setPrototypeOf()、Object.getPrototypeOf()

###### 9.8.1 _proto_属性

用来读取或设置当前对象的prototype对象

###### 9.8.2 Object.setPrototypeOf()

与proto相同，用来设置一个对象的prototype对象，返回参数对象本身。它是ES6正式推荐的设置原型对象的方法。

```javascript
//格式
Object.setPrototypeOf(object,prototype)

let proto = {}
let obj = {x:10};
Object.setPrototypeOf(obj,proto);
proto.y = 20
proto.z = 40;

obj.x//10
obj.y//20
obk.z//40
```

上面的代码将proto对象设置为obj对象的原型，所以从obj对象可以读取proto对象的属性。

###### 9.8.3 Object.getPrototypeOf()

该方法与setPrototypeOf方法配套，用于读取一个对象的prototype对象。

```javascript
function Rectangle(){
//...
}
var rec = new Rectangle();
Object.getPrototypeOf(rec) === Rectangle.prototype
```

#### 9.9 Object.keys()、Object.values()、Object.entries()

###### 9.9.1 Object.keys()

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键名

```javascript
var obj = {foo:'bar',baz:42};
Object.keys(obj)//['foo','baz']
```

###### 9.9.2 Object.values()

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值

```javascript
Object.values(obj)//['bar',42]
```

###### 9.9.3 Object.entries ()

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值对数组

```javascript
Object.entries(obj)//[['foo','bar'],['baz',42]]
```

#### 9.10 对象的拓展运算符

```javascript
let {x,y,...z} = {x:1,y:2,a:3,b:4}
x//1
b//2
z//{a:3,b:4}
```

**解构赋值的复制时浅复制**

```javascript
let obj = {a:{b:1}};
let {...x} = obj;
obj.a.b = 2;
x.a.b//2
```

**解构赋值不会复制继承自原型对象的属性**

```javascript
let o1 = {a:1};
let o2 = {b:2};
o2._proto_ = o1;
let {...o3} = o2;
o3 //{b:2}
o3.a //undefined
```

拓展某个函数的参数，引入其他操作。

```javascript
function baseFunction({a,b}){
//...
}
function wrapperFunction({x,y,...restconfig}){
//使用x和y参数进行操作
//其余参数传给原始函数
return baseFunction(restconfig)
}
```

拓展运算符用于取出参数对象的所有可遍历属性，并将其复制到当前对象之中。

```javascript
let z = {a:3,b:4};
let n = {...z}
n//{a:3,b:4} 这等同于使用Object.assign方法
```

拓展运算符可用于合并两个对象

```javas
let ab = {...a,...b}
```

#### 9.11 Object.getOwnPropertyDescriptors()

返回指定对象所有自身属性（非继承属性）的描述对象。

```javascript
const obj = {
foo:123,
get bar(){ return 'abc'}
}
Object.getOwnPropertyDescriptors(obj)
{
foo:{
vale:123,
writable:true,
enumerable:true,
configurable:true
},
bar:{
get:[function,bar],
set:undefined,
enumrable:true,
configurable:true
}
}
```

## 第14 章 Promise













































































































