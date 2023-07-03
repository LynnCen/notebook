[TOC]

### 文档: https://wangdoc.com/es6/

## 第 2 章 let and const

#### 2.1 let 命令

不存在变量提升，必须在声明后使用。

暂时性死区：在代码块内，使用 let 命令声明变量之前，该变量都是不可使用的。

不允许重复声明。

#### 2.2 块级作用域

取代了立即执行函数(IIFE);

###### 块级作用域和函数声明

允许在块级作用域中声明函数

函数声明类似 var，即会提升到全局作用域或函数作用域的头部

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

#### 2.3 const 命令

基本用法：const 声明一个只读的常量。一旦声明，常量的值就不能改变。

const 命令声明的常量也不会提升，同样存在暂时性死区，只能在声明后使用且不可重复声明。

本质：const 实际上保证的不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于简单类型的数据（数值，字符串，布尔值），值就保存在变量指向的内存地址中，因此等同于常量。但是对于复杂类型的数据（主要是对象和数组），变量就指向的内存地址保存的只是一个指针，const 只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，这完全不受控制。

Object.freeze ——冻结对象

es6 声明变量的 6 中方法——var function let const import class

## 第 3 章 变量的解构与赋值

#### 3.1 数组的解构赋值

基本使用：

```javascript
let [head, ...tail] = [1, 2, 3, 4];
head; //1
tail; //[2,3,4]
```

指定默认值：

```javascript
let [foo = true] = [];
foo; //true
```

#### 3.2 对象的解构赋值

对象解构赋值和数组解构赋值有一个不同：数组的元素是按次序排列的，变量的取值是由它的位置决定的；而对象的属性没有次序，变量必须与属性同名才能取到正确的值。

```javascript
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo; //'aaa'
bar; //'bbb'

let { baz } = { foo: "aaa", bar: "bbb" };
baz; //undefined
```

指定默认值：

```javascript
var { x = 3 } = {};
x; //3
//默认值生效的条件是：对象属性值严格等于undefined
```

#### 3.3 字符串的解构赋值

字符串也可以解构赋值，因此字符串被转换成一个类似数组的对象

```javascript
const [a, b, c, d, e] = "hello";
a; //'h'
b; //'e'
c; //'l'
d; //'l'
e; //'o'
```

#### 3.4 数值和布尔值的解构赋值

解构赋值时，如果等号的右边是数值和布尔值。则会先转为对象

#### 3.5 函数参数的解构赋值

#### 3.6 用途

**交换变量的值**

```javascript
let x = 1;
let y = 2;
[x, y] = [y, x];
```

**从函数中返回多个值**

```javascript
function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

//返回一个对象
function example() {
  return {
    foo: 1,
    bar: 2,
  };
}
let { foo, bar } = example();
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

**提取 JSON 数据**

```javascript
let jsonData = {
  id: 22,
  status: "ok",
  data: [213, 323],
};
let { id, status, data: number } = jsonData;
```

**函数参数的默认值**

```javascript
jquery.ajax = function (
  url,
  {
    async = true,
    beforeSend = function () {},
    cache = true,
    complete = function () {},
    crossDomain = false,
    global = true,
    //...more
  }
) {
  //...do
};
```

**遍历 Map 结构**

任何部署了 iterator 接口的对象都可以使用 for of 遍历循环。

```javascript
var map = new Map();
map.set("first", "hello");
map.set("second", "world");

for (let [key, value] of map) {
  console.log(key + "is" + value);
}
//first is hello
//second is world
```

**输入模块的指定方法**

```javascript
const { SoourceMapconsumer, SourceNode } = require("source-map");
```

## 第 4 章 字符串的拓展

#### 4.1 字符的 Unicode 表示法

#### 4.2 codePointAt()

能够正确处理 4 个字符存储的字符，返回一个字符的码点。这个方法定义在字符串的实例对象上。

参数：字符在字符串中的位置

#### 4.3 String.fromCodePoint()

用于从码点返回对应字符，但是这个方法不能识别 32 位的 UTF-16 字符。这个方法定义在 String 对象上。

#### 4.4 字符串的遍历器接口

字符串可由 for...of 循环遍历

```javascript
for (let codePoint of "foo") {
  console.log(codePoint);
}
//'f'
//'o'
//'o'
```

#### 4.5 at()

es5 对字符串对象提供了 charAt 方法，返回字符串给定位置的字符。该方法不能识别码点大于 oxFFFF 的字符.而 at 方法则可以。

```javascript
"abc".charAt(0); //'a'
"吉".charAt(0); //'\uD842'
"吉".charAt(0); //'吉'
```

#### 4.6 normalize()

#### 4.7 includes()、statsWith()、endsWith()

includes()：返回布尔值，表示是否找到了这个参数字符串。

statsWith()：返回布尔值，表示参数字符串是否在源字符串的头部。

endsWith()：返回布尔值，表示参数字符串是否在源字符串的尾部。

```javascript
var s = "hello world!";

s.startsWith("hello"); //true
s.endsWith("!"); //true
s.includes("o"); //true
```

第二个参数表示开始搜索的位置

#### 4.8 repeat()

repeat()：返回一个新的字符串，表示将源字符串重复 n 次

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
`hello ${name},how are you${time}?`;
```

如果在模板字符串中需要使用反引号，则在其前面要用反斜杠来转义

```javascript
var greeting = `\`yo\`world!`;
```

模板字符串中还能调用函数

```javascript
function fn() {}
`foo ${fn()} bar`;
```

#### 4.11 实例：模板编译

#### 4.12 标签模板

#### 4.13 String.raw()

用来充当模板字符串的处理函数，返回一个反斜线都被转移(即反斜线前面再加一个反斜线)的字符串，对应于替换变量后的模板字符串。

#### 4.14 模板字符串的限制

## 第 5 章 正则的拓展

## 第 6 章 数值的拓展

#### 6.1 二进制和八进制的表示法

#### 6.2 Number.isFinite() 、Number.isNaN()

**Number.isFinite()**:用来检查一个数值是否为有限的(finite).

**NUmber.isNaN()**：用来检查一个值是否为 NaN.

#### 6.3 Number.parselnt()、Number.parseFloat()

```javascript
Number.parselnt("12.34"); //12
Number.parseFloat("123.34#"); //123.34
```

#### 6.4 Number.islntteger()

**Number.islntteger()** —— 用来判断一个值是否为整数。

#### 6.5 Number.EPSILON()

**Number.EPSILON()** —— 新增一个极小的常量

#### 6.6 安全整数和 Number.isSafeinteger()

新引入了 Number.MAX_SAFE_INTEGER 和 Number.MIN_INTEGER 两个常量来表示整数的范围。

**Number.isSafeinteger()** —— 用来判断一个整数是否落在这个范围之内。

#### 6.7 Math 对象的拓展

###### 6.7.1 Math.trunc()

**Math.trunc()** —— 用于去除一个数的小数部分，返回整数部分，对于非数值，其内部使用 Number 方法将其先转为数值，对于空值和无法截取整数的值，返回 NAN

###### 6.7.2 Math.sign()

**Math.sign()** —— 用来判断一个数到底是正数，负数，还是零。对于非数值会先将其转为数值。

返回值的 5 种情况：

参数为正数，返回+1；

参数为负数，返回-1；

参数为 0，返回 0；

参数为-0，返回-0；

其他值，返回 NaN；

###### 6.7.3 Math.cbrt()

**Math.cbrt()** —— 用于计算一个数的立方根

###### 6.7.4 Math.clz32()

###### 6.7.5 Math.imul()

**Math.imul()** —— 返回两个数以 32 位带符号整数形式相乘的结果，返回也是一个 32 位的带符号整数。

## 第 7 章 函数的拓展

#### 7.1 函数参数的默认值

###### 7.1.1 基本用法

```javascript
function Person(x = 0, y = 0) {
  this.x = x;
  this.y = y;
}
var p = new Person();
p; //{x:0,y:0}
```

**note：**

① 参数变量是默认生命，所以不能用 let 或 const 再次声明

② 使用参数默认值时，函数不能有同名参数。

###### 7.1.2 与解构赋值默认值结合使用

```javascript
function foo({ x, y = 5 }) {
  console.log(x, y);
}
foo({}); //undefiend,5
foo({ x: 1 }); // 1,5
foo(); // TypeError:cannot read property 'x' of undefiend
```

不能够省略第一个参数，改进：

```javascript
function fetch(url, { method = "GET" } = {}) {
  console.log(method);
}
fetch("http://example.com"); //'GET'
```

###### 7.1.3 参数默认值的位置

通常定义了默认值的参数应该是函数的尾参数，如果非尾部的参数设置默认值，实际上这个参数就是无法省略的。

```javascript
function f(x = 1, y) {
  return [x, y];
}
f(); //[1,undefiend]
```

###### 7.1.4 函数的 length 属性

指定默认值以后，函数的 length 属性将返回没有指定默认值的参数个数。也就是说指定默认值之后，length 属性将失真。

```javascript
(function (a) {}
  .length(
    //1
    function (a = 5) {}
  )
  .length(
    //0
    function (a, b, c = 5) {}
  ).lenght); //2
```

rest 参数也不会计入 length 属性

```javascript
(function(..args){}).lenght//0
```

如果设置了默认值的参数不是尾参数，那么 length 属性也不再计入后面的参数

```javascript
(function (a = 0, b, c) {}.lenght); //0
```

###### 7.1.5 作用域

一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域。等到执行结束，这个作用域就会消失。这种语法在不设置默认参数是不会出现的。

```javascript
var x = 1;
function f(x, y = x) {
  conmsole.log(y);
}
f(2); //2
```

```javascript
let x = 1;
function f(y = x) {
  let x = 2;
  console.log(y);
}
f(); //1
```

###### 7.1.6 应用

#### 7.2 rest 参数

形式为 （...变量名），用于获取函数的多余参数，这样就不需要使用 arguments 对象了，rest 参数搭配的变量时一个数组，该变量将多余的参数放入其中。

#### 7.3 严格模式

#### 7.4 name 属性

函数的 name 属性返回该函数的函数名

```javascript
function foo() {}
foo.name; //'foo'
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

① 函数体内的 this 对象就是定义所在的对象，而并不是使用时所在的对象。

② 不可以当作构造函数。也就是说，不可以使用 new 命令，否则会抛出一个错误。

③ 不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

④ 不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

this 对象的指向是可变的，但在箭头函数中它是固定的。

```javascript
function foo() {
  setTimeout(() => {
    console.log("id", this.id);
  }, 100);
}
var id = 21;
foo.call({ id: 42 });
//id:42 如果是普通函数，执行时this应该指向全局对象window，这时应该输出21，但是箭头函数导致this总是指向函数定义生效时所在的对象(本例是{id：42})，所以是42.
```

箭头函数可以让 setTimeout 里面的 this 绑定定义时所在的作用域，而不是指向运行时所在的作用域。

```javascript
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  //箭头函数
  setInterval(() => this.s1++, 1000);
  //普通函数
  setInterval(function () {
    this.s2++;
  }, 1000);
}

var timer = new Timer();
setTimeout(() => console.log("s1:", timer.s1), 3100);
setTimeout(() => console.log("s2:", timer.s2), 3100);
//s1:3
//s2:0
```

上面的代码中，Timer 函数内部设置了两个定时器，分别使用箭头函数和普通函数。前者的 this 绑定定义时所在的作用域(即 Timer 函数)，后者的 this 指向运行所在的作用域(即全局对象)。所以 3100ms 后，timer.s1 被更新了 3 次，而 timer.s2 一次都没更新。

箭头函数可以让 this 指向固定化，这种特性非常有利于封装回调函数。下面是一个例子，DOM 事件的回调函数封装在一个对象里面。

```javascript
var hander = {
  id: "123456",
  init: function () {
    document.addEventListener("click", (e) => this.doSomething(e.type), fasle);
  },
  doSomething: function () {},
};
```

以上代码的 init 方法中使用了箭头函数，这导致箭头函数里面的 this 总是指向 handler 对象。否则，回调函数运行时，this.doSomething 一行会报错，因此此时 this 指向 document 对象。

this 指向的固定化并不是箭头函数有绑定 this 的机制，实际原因时箭头函数根本没有自己的 this，导致内部的 this 就是外层的代码块 this。正因为它没有自己的 this，所以不能用作构造函数。

除了 this。一下 3 个变量在箭头函数中也是不存在的，分别指向外层函数的对象变量 arguments，super 和 new.target.

###### 7.5.3 嵌套的箭头函数

#### 7.6 绑定 this

箭头函数可以绑定 this 对象，大大减少了显式绑定 this 对象的写法(call，apply，bind）

函数绑定运算符时并排的双冒号(::),冒号左边是一个对象，右边是一个函数。该运算符会自动的将左边的对象作为执行的上下文环境(即 this 对象)，绑定到右边的函数上。es7 提案！

#### 7.7 尾调用优化

#### 7.8 函数参数的尾逗号

## 第 8 章 数组的拓展

#### 8.1 拓展运算符

###### 8.1.1 含义

拓展运算符是三个点（...），它如同 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。

```javascript
console.log(...[1,2,3]);//1,2,3
console.log(1,...[2,3,4],5);/1,2,3,4,5
```

该运算符主要用于函数调用。

```javascript
function push(array, ...item) {
  array.push(...item);
}
function add(x, y) {
  return x + y;
}
var numbers = [4, 38];
add(...numbers); //42
```

与正常参数使用。

```javascript
function f(v, w, x, y, z) {}
var args = [0, 1];
f(-1, ...args, 2, ...[3]);
```

后面还可以放置表达式

```javascript
const arr = [...(x > 0 ? ["a"] : []), "b"];
```

###### 8.1.2 替代数组的 apply 方法

```javas
function f(x,y,z){
//...
}
var args = [0,1,2];
f(...args);
```

将一个数组添加到另一个数组的尾部

```javascript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1.push(...arr2);
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
var d = new Date(...dateFields);
```

**字符串**

可以将字符串转为真正的数组

```javascript
[..."hello"];
//['h','e','l','l','o']
```

**实现了 iterator 接口的对象**

任何 iterator 接口的对象都可以用拓展运算符转为真正的数组。

```javascript
var nodelist = document.querSelectorAll("div");
var array = [...nodelist];
```

**Map 和 Set 结构，Generator 函数**

拓展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都 i 可以使用拓展运算符，如 Map 结构。

```javascript
let map = new Map([
  [1, "one"],
  [2, "two"],
  [3, "three"],
]);
let arr = [...map.keys()]; //[1,2,3]
```

Generator 函数运行后会返回一个遍历器对象，因此可以使用拓展运算符。

```javascript
var go = function *(){
yield 1;
yield 2;
yield 3;
}
[...go()] //[1,2,3]
```

note：没有 Iterator 接口的对象，使用会报错。

#### 8.2 Array.from()

用于将两类对象转化为真正的数组：类似数组的对象和可遍历对象

```javascript
let arrayLike = {
  0: "a",
  1: "b",
  2: "c",
  length: 3,
};

let arr2 = Array.from(arrayLike); //['a','b','c']
```

只要部署了 Iterator 接口的数据结构，该方法都能将其转为数组。

```javascript
Array.from("hello");
//['h','e','l','l','o']

let nameSet = new Set(["a", "b"]);
Array.from(nameSet); //['a','b'];
```

拓展运算符也可以将某些数据转为数组（前面有提到过）

拓展运算符背后调用的是遍历器接口(Symbol.iterator),如果一个对象没有部署该接口，就无法转化。

Array.from()还支持类似数组的对象，所谓的类数组就是，即必须有 length 属性。因此，任何具有 length 属性的对象，都可以通该方法转为数组，而拓展运算符就没法做到。

```javascript
Array.from({ length: 3 }); //[undefined,undefined,undefined]
```

接收第二个参数，作用类似数组的 map 方法，用来对每个元素进行处理，将处理后的值放入返回的数组中。

```javascript
Array.from([1, 2, 3], (x) => x * x); //[1,4,9]
//取出DOM节点的文本内容
let spans = document.querySelectorAll("span.name");
let names1 = Array.from(spans, (s) => s.textContent);
```

如果 map 函数中用到了 this 关键字，还可以传入 Array.from 第三个参数，用来绑定 this。

#### 8.3 Array.of()

用于将一组值转换为数组。

```javascript
Array.of(3, 11, 8); //[3,11,8]
Array.of(3); //[3]
```

Array.of 基本上可以用来替代 Array()或 new Array()，并且不存在参数不同导致的重载。它的行为非常统一。

#### 8.4 数组实例的 copyWithin()

会在当前数组内部将指定位置的成员复制到其他位置(会覆盖原有成员)，然后返回当前数组。也就是说，这个方法会修改当前数组。

```javascript
Array.prototype.copyWithin(target, (start = 0), (end = this.length));
```

接收三个参数

target（必选）：从该位置开始替换数据。

start（可选）：从该位置开始读取数据，默认为 0。如果为负数，表示倒数。

end（可选）：到该位置前停止读取数据，默认等于数据长度。如果为负数，表示倒数。

这三个参数都应该是数值，如果不是，则会自动转为数值。

```javascript
//将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4);
//[4,2,3,4,5]
```

#### 8.5 数组实例的 find()和 findIndex()

find()——用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，知道找出第一个返回值为 true 的成员，然后返回该成员，如果没有符合条件的成员，则返回 undefined。

```javascript
[1, 5, 10, 15].find((item, index, arr) => {
  return item > 9;
}); // 10
```

findIndex() —— 与 find 方法类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1

```javascript
[1, 5, 10, 15].findIndex((item, index, arr) => {
  return item > 9;
}); //2
```

这两个方法都可以接受第二个参数，用来绑定回调函数的 this 对象。另外还可以发现 NaN，弥补了数组的 indexOf 方法的不足。

#### 8.6 数组实例的 fill()

使用给定值填充一个数组。

```javascript
["a", "b", "c"]
  .fill(7) //[7,7,7]
  [("a", "b", "c")].fill(7, 1, 2); //['a',7,'c']
```

#### 8.7 数组实例的 entries()、keys()和 values()

用于遍历数组。它们都返回一个遍历器对象，可用 for...of 循环遍历，唯一的区别在于，keys()是对**键名**的遍历，values()是对**键值**的遍历，entries()是对**键值对**的遍历

```javascript
for (let index of ["a", "b"].keys()) {
  console.log(index);
}
//0
//1
for (let elem of ["a", "b"].values()) {
  console.log(elem);
}
//'a'
//'b'
for (let [index, elem] of ["a", "b"].entries()) {
  console.log(index, elem);
}
//0 'a'
//1 'b'
```

如果不使用 for...of 循环，可以手动调用遍历器对象的**next**方法进行遍历

#### 8.8 数组实例的 includes()

该方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的 includes 方法类似。

```javascript
[1, 2, 3]
  .includes(2) //true
  [(1, 2, NaN)].includes(NaN); //true
```

没有该方法，通常使用 indexOf 方法检查是否包含某个值。有两个缺点，一是不够语义化，其含义是找到参数值的第一个出现的位置，所以要比较是否不等于-1，表达起来不够直观；二是，其内部使用严格相等运算符（===）进行判断，会导致对 NaN 的误判。

另外，Map 和 Set 数据结构有一个 has 方法，需要注意与 includes 区分。

Map 结构的 has 方法是用来查找键名的，比如 Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target,propertyKey)。

Set 结构的 has 方法是用来查找值得，比如 Set.prototype.has(value)、WeakSet.prototype.has(value)

#### 8.9 数组的空位

数组的空位是指数组的某一个位置没有任何值。比如 Array 构造函数返回的数组都是空位

```
Array(3)//[,,,]
```

空位不是 undefined，一个位置的值等于 undefined 依然是有值的。空位是没有任何值，in 运算符可以说明这一点。

```javascript
0 in [undefined, undefined, undefined]; //true
0 in [, , ,]; //false
```

forEach()、filter()、every()和 some()都会跳过空位。

map()会跳过这个空位，但会保留这个值。

join()和 toString()会将空位视为 undefined，而 undefined 和 null 会被处理成空字符串。

## 第 9 章 对象的拓展

#### 9.1 属性的简洁表示法

允许直接写入变量和函数作为对象的属性和方法。

```javascript
var foo = "bar";
var baz = { foo };
baz; //{foo:'bar'}
```

允许只写属性名，不写属性值。这时，属性值等于属性名所代表的变量。

```javascript
function f(x, y) {
  return { x, y };
}
```

方法简写：

```javascript
var o = {
  method() {
    return "hello";
  },
};
```

commJS 模块输出变量就非常适合使用简洁写法

```javascript
module.exports = { getItem, setItem, clear };
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

#### 9.3 方法的 name 属性

函数的 name 属性返回函数名。对象方法也是函数，因此也有 name 属性。

```javascript
const person = {
  sayName() {
    console.log("hello");
  },
};
person.sayName.name; //'sayName'
```

#### 9.4 Object.is()

ES5 比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。前者会自动转换数据类型，后者 NaN 不等于自身，以及+0 等于-0。

Object.is() —— 它用来比较两个值是否严格相等，与严格相等运算符的行为一致。

#### 9.5 Object.assign()

###### 9.5.1 基本用法

用于将源对象的所有可枚举属性复制到目标对象。第一个参数就是目标对象，后面的参数都是源对象。

```javascript
var target = { a: 1 };
var source1 = { b: 2 };
var source2 = { c: 3 };

Object.assign(target, source1, source2);
target; //{a:1,b:2,c:3}
```

note：如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

如果只有一个参数，会直接返回该参数。

如果该参数不是对象，则会先转成对象，然后返回。

由于 undefined 和 null 无法转成对象，所以如果将他们作为参数，则会报错。

###### 9.5.2 注意点

Object.assign 方法执行的是浅复制，而不是深复制。如果源对象某个属性的值是对象，那么目标对象得到这个对象的引用。

```javascript
var obj1 = { a: { b: 1 } };
var obj2 = Object.assign({}, obj1);
obj1.a.b = 2;
obj2.a.b; // 2
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

assign 方法将 x 属性和 y 属性添加到了 Point 类的对象实例中。

**为对象添加方法**

```javascript
Object.assign(SomeClass.prototype, {
  someMethod(arg1, arg2) {},
});
```

**克隆对象**

```javascript
function clone(orgin) {
  return Object.assign({}, orgin);
}
```

将原始对象复制到一个空对象中，就得到了原始对象的克隆。

不过，采用这种方法只能克隆原始对象自身的值，不能克隆它继承的值。如果想要继承原型链，采用如下代码：

```javascript
function clone(origin) {
  let originProto = Object.getPrototypeOf(origin);
  return Object.assign(Object.create(originProto), origin);
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

Object.getOwnPropertyDescriptor 方法可以获取该属性的描述对象。

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

ES5 有三个操作会忽略 enumerable 为 false 的属性。

for...in 属性：只遍历对象自身的和继承的可枚举属性。

Object.keys():返回对象自身的所有可枚举属性的键名。

JSON.stringify()：只串行化对象自身的可枚举属性。

ES6 Object.assign()也会忽略 enumerable 为 false 的属性，只复制对象自身的可枚举属性。

ES6 所有的 Class 的原型的方法都是不可枚举的

#### 9.7 属性的遍历

###### **1.for...in**

循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。

###### 2.Object.keys(obj)

返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）。

###### 3.Object.getOwnPropertyNames()

返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）

###### 4.Object.getOwnPropertySymbols(obj)

返回一个数组，包含对象自身的所有所有 Symbol 属性。

###### 5.Reflect.ownKeys(obj)

返回一个数组，包含对象自身的所有属性，不管属性名是 Symbol 还是字符串，也不管是否可枚举。

#### 9.8 *proto*属性、Object.setPrototypeOf()、Object.getPrototypeOf()

###### 9.8.1 *proto*属性

用来读取或设置当前对象的 prototype 对象

###### 9.8.2 Object.setPrototypeOf()

与 proto 相同，用来设置一个对象的 prototype 对象，返回参数对象本身。它是 ES6 正式推荐的设置原型对象的方法。

```javascript
//格式
Object.setPrototypeOf(object, prototype);

let proto = {};
let obj = { x: 10 };
Object.setPrototypeOf(obj, proto);
proto.y = 20;
proto.z = 40;

obj.x; //10
obj.y; //20
obk.z; //40
```

上面的代码将 proto 对象设置为 obj 对象的原型，所以从 obj 对象可以读取 proto 对象的属性。

###### 9.8.3 Object.getPrototypeOf()

该方法与 setPrototypeOf 方法配套，用于读取一个对象的 prototype 对象。

```javascript
function Rectangle() {
  //...
}
var rec = new Rectangle();
Object.getPrototypeOf(rec) === Rectangle.prototype;
```

#### 9.9 Object.keys()、Object.values()、Object.entries()

###### 9.9.1 Object.keys()

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键名

```javascript
var obj = { foo: "bar", baz: 42 };
Object.keys(obj); //['foo','baz']
```

###### 9.9.2 Object.values()

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值

```javascript
Object.values(obj); //['bar',42]
```

###### 9.9.3 Object.entries ()

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值对数组

```javascript
Object.entries(obj); //[['foo','bar'],['baz',42]]
```

#### 9.10 对象的拓展运算符

```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x; //1
b; //2
z; //{a:3,b:4}
```

**解构赋值的复制时浅复制**

```javascript
let obj = { a: { b: 1 } };
let { ...x } = obj;
obj.a.b = 2;
x.a.b; //2
```

**解构赋值不会复制继承自原型对象的属性**

```javascript
let o1 = { a: 1 };
let o2 = { b: 2 };
o2._proto_ = o1;
let { ...o3 } = o2;
o3; //{b:2}
o3.a; //undefined
```

拓展某个函数的参数，引入其他操作。

```javascript
function baseFunction({ a, b }) {
  //...
}
function wrapperFunction({ x, y, ...restconfig }) {
  //使用x和y参数进行操作
  //其余参数传给原始函数
  return baseFunction(restconfig);
}
```

拓展运算符用于取出参数对象的所有可遍历属性，并将其复制到当前对象之中。

```javascript
let z = { a: 3, b: 4 };
let n = { ...z };
n; //{a:3,b:4} 这等同于使用Object.assign方法
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

## Symbol

es5 对象的属性名都是字符串，为了防止属性名冲突，引入了 Symbol。属性名独一无二。

## Set 和 Map

set 定义：它类似于数组，但是成员的值都是唯一的，没有重复的值。

#### Set 实例的属性和方法

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。

- `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。
- `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `Set.prototype.clear()`：清除所有成员，没有返回值。

Set 结构的实例有四个遍历方法，可以用于遍历成员。

- `Set.prototype.keys()`：返回键名的遍历器
- `Set.prototype.values()`：返回键值的遍历器
- `Set.prototype.entries()`：返回键值对的遍历器
- `Set.prototype.forEach()`：使用回调函数遍历每个成员

#### 应用

数组去重

```javascript
function dedupe(array) {
  return Array.from(new Set(array));
}
```

并集（Union）、交集（Intersect）和差集

```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter((x) => b.has(x)));
// set {2, 3}

// （a 相对于 b 的）差集
let difference = new Set([...a].filter((x) => !b.has(x)));
// Set {1}
```

#### weakSet

WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。

首先，WeakSet 的成员只能是对象，而不能是其他类型的值。

WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。

WeakSet 不能遍历，是因为成员都是弱引用，随时可能消失，遍历机制无法保证成员的存在，很可能刚刚遍历结束，成员就取不到了。WeakSet 的一个用处，是储存 DOM 节点，而不用担心这些节点从文档移除时，会引发内存泄漏。

#### Map

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

## Proxy

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

下面是 Proxy 支持的拦截操作一览，一共 13 种。

- **get(target, propKey, receiver)**：拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`。
- **set(target, propKey, value, receiver)**：拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值。
- **has(target, propKey)**：拦截`propKey in proxy`的操作，返回一个布尔值。
- **deleteProperty(target, propKey)**：拦截`delete proxy[propKey]`的操作，返回一个布尔值。
- **ownKeys(target)**：拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性。
- **getOwnPropertyDescriptor(target, propKey)**：拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。
- **defineProperty(target, propKey, propDesc)**：拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。
- **preventExtensions(target)**：拦截`Object.preventExtensions(proxy)`，返回一个布尔值。
- **getPrototypeOf(target)**：拦截`Object.getPrototypeOf(proxy)`，返回一个对象。
- **isExtensible(target)**：拦截`Object.isExtensible(proxy)`，返回一个布尔值。
- **setPrototypeOf(target, proto)**：拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
- **apply(target, object, args)**：拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`。
- **construct(target, args)**：拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`。

## Reflect

`Reflect`对象与`Proxy`对象一样，也是 ES6 为了操作对象而提供的新 API。`Reflect`对象的设计目的有这样几个。

（1） 将`Object`对象的一些明显属于语言内部的方法（比如`Object.defineProperty`），放到`Reflect`对象上。现阶段，某些方法同时在`Object`和`Reflect`对象上部署，未来的新方法将只部署在`Reflect`对象上。也就是说，从`Reflect`对象上可以拿到语言内部的方法。

（2） 修改某些`Object`方法的返回结果，让其变得更合理。比如，`Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false`。

（3） 让`Object`操作都变成函数行为。某些`Object`操作是命令式，比如`name in obj`和`delete obj[name]`，而`Reflect.has(obj, name)`和`Reflect.deleteProperty(obj, name)`让它们变成了函数行为。

（4）`Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。这就让`Proxy`对象可以方便地调用对应的`Reflect`方法，完成默认行为，作为修改行为的基础。也就是说，不管`Proxy`怎么修改默认行为，你总可以在`Reflect`上获取默认行为。

`Reflect`对象一共有 13 个静态方法。

- Reflect.apply(target, thisArg, args)
- Reflect.construct(target, args)
- Reflect.get(target, name, receiver)
- Reflect.set(target, name, value, receiver)
- Reflect.defineProperty(target, name, desc)
- Reflect.deleteProperty(target, name)
- Reflect.has(target, name)
- Reflect.ownKeys(target)
- Reflect.isExtensible(target)
- Reflect.preventExtensions(target)
- Reflect.getOwnPropertyDescriptor(target, name)
- Reflect.getPrototypeOf(target)
- Reflect.setPrototypeOf(target, prototype)

上面这些方法的作用，大部分与`Object`对象的同名方法的作用都是相同的，而且它与`Proxy`对象的方法是一一对应的。下面是对它们的解释。

## 第 14 章 Promise

所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

`Promise`对象有以下两个特点。

（1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

`Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

#### Promise.prototype.then()

Promise 实例具有`then`方法，也就是说，`then`方法是定义在原型对象`Promise.prototype`上的。它的作用是为 Promise 实例添加状态改变时的回调函数。前面说过，`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数是`rejected`状态的回调函数，它们都是可选的。

`then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）。因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法。

#### Promise.prototype.catch()

`Promise.prototype.catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

#### Promise.prototype.finally()

`finally()`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。

### Promise.all()

`Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

`p`的状态由`p1`、`p2`、`p3`决定，分成两种情况。

（1）只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

（2）只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

#### Promise.race()

`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数。

`Promise.race()`方法的参数与`Promise.all()`方法一样，如果不是 Promise 实例，就会先调用下面讲到的`Promise.resolve()`方法，将参数转为 Promise 实例，再进一步处理。

#### Promise.allSettled()

`Promise.allSettled()`方法，用来确定一组异步操作是否都结束了（不管成功或失败）。所以，它的名字叫做”Settled“，包含了”fulfilled“和”rejected“两种情况。

`Promise.allSettled()`方法接受一个数组作为参数，数组的每个成员都是一个 Promise 对象，并返回一个新的 Promise 对象。只有等到参数数组的所有 Promise 对象都发生状态变更（不管是`fulfilled`还是`rejected`），返回的 Promise 对象才会发生状态变更。

#### Promise.any()

只要参数实例有一个变成`fulfilled`状态，包装实例就会变成`fulfilled`状态；如果所有参数实例都变成`rejected`状态，包装实例就会变成`rejected`状态。

`Promise.any()`跟`Promise.race()`方法很像，只有一点不同，就是`Promise.any()`不会因为某个 Promise 变成`rejected`状态而结束，必须等到所有参数 Promise 变成`rejected`状态才会结束。

#### Promise.resolve()

有时需要将现有对象转为 Promise 对象，`Promise.resolve()`方法就起到这个作用。

#### Promise.reject()

`Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected`。

## Iterator 和 for...of 循环

遍历器（Iterator）就是这样一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

Iterator 的作用有三个：

一是为各种数据结构，提供一个统一的、简便的访问接口；

二是使得数据结构的成员能够按某种次序排列；

三是 ES6 创造了一种新的遍历命令`for...of`循环，Iterator 接口主要供`for...of`消费。

Iterator 的遍历过程是这样的。

（1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。

（2）第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员。

（3）第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员。

（4）不断调用指针对象的`next`方法，直到它指向数据结构的结束位置。

每一次调用`next`方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含`value`和`done`两个属性的对象。其中，`value`属性是当前成员的值，`done`属性是一个布尔值，表示遍历是否结束。

一种数据结构只要部署了 Iterator 接口，我们就称这种数据结构是“可遍历的”（iterable）。

原生具备 Iterator 接口的数据结构如下。

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

#### 调用 Iterator 接口的场合

**（1）解构赋值**

对数组和 Set 结构进行解构赋值时，会默认调用`Symbol.iterator`方法。

**（2）扩展运算符**

扩展运算符（...）也会调用默认的 Iterator 接口。

**（3）yield\***

`yield*`后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。

**（4）其他场合**

由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口。下面是一些例子。

- for...of
- Array.from()
- Map(), Set(), WeakMap(), WeakSet()（比如`new Map([['a',1],['b',2]])`）
- Promise.all()
- Promise.race()

#### 字符串的 Iterator 接口

字符串是一个类似数组的对象，也原生具有 Iterator 接口。

#### Iterator 接口与 Generator 函数

#### 遍历器对象的 return()，throw()

`return()`方法的使用场合是，如果`for...of`循环提前退出（通常是因为出错，或者有`break`语句），就会调用`return()`方法。如果一个对象在完成遍历前，需要清理或释放资源，就可以部署`return()`方法。

#### for...of 循环

一个数据结构只要部署了`Symbol.iterator`属性，就被视为具有 iterator 接口，就可以用`for...of`循环遍历它的成员。也就是说，`for...of`循环内部调用的是数据结构的`Symbol.iterator`方法。

`for...of`循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如`arguments`对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串。

JavaScript 原有的`for...in`循环，只能获得对象的键名，不能直接获取键值。ES6 提供`for...of`循环，允许遍历获得键值。

#### 与其他遍历语法的比较

无法中途跳出`forEach`循环，`break`命令或`return`命令都不能奏效。

- 数组的键名是数字，但是`for...in`循环是以字符串作为键名“0”、“1”、“2”等等。
- `for...in`循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。
- 某些情况下，`for...in`循环会以任意顺序遍历键名。

总之，`for...in`循环主要是为遍历对象而设计的，不适用于遍历数组。

`for...of`循环相比上面几种做法，有一些显著的优点。

- 有着同`for...in`一样的简洁语法，但是没有`for...in`那些缺点。
- 不同于`forEach`方法，它可以与`break`、`continue`和`return`配合使用。
- 提供了遍历所有数据结构的统一操作接口。

## Generator 函数的语法

Generator 函数有多种理解角度。语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态。

执行 Generator 函数会返回一个遍历器对象(Iterator)，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

Generator 函数的调用方法与普通函数一样，也是在函数名后面加上一对圆括号。不同的是，调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是上一章介绍的遍历器对象（Iterator Object）。

```javascript
function* helloWorldGenerator() {
  yield "hello";
  yield "world";
  return "ending";
}

var hw = helloWorldGenerator();
hw.next();
// { value: 'hello', done: false }

hw.next();
// { value: 'world', done: false }

hw.next();
// { value: 'ending', done: true }

hw.next();
// { value: undefined, done: true }
```

第一次调用，Generator 函数开始执行，直到遇到第一个`yield`表达式为止。`next`方法返回一个对象，它的`value`属性就是当前`yield`表达式的值`hello`，`done`属性的值`false`，表示遍历还没有结束。

第二次调用，Generator 函数从上次`yield`表达式停下的地方，一直执行到下一个`yield`表达式。`next`方法返回的对象的`value`属性就是当前`yield`表达式的值`world`，`done`属性的值`false`，表示遍历还没有结束。

第三次调用，Generator 函数从上次`yield`表达式停下的地方，一直执行到`return`语句（如果没有`return`语句，就执行到函数结束）。`next`方法返回的对象的`value`属性，就是紧跟在`return`语句后面的表达式的值（如果没有`return`语句，则`value`属性的值为`undefined`），`done`属性的值`true`，表示遍历已经结束。

第四次调用，此时 Generator 函数已经运行完毕，`next`方法返回对象的`value`属性为`undefined`，`done`属性为`true`。以后再调用`next`方法，返回的都是这个值。

总结一下，调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的`next`方法，就会返回一个有着`value`和`done`两个属性的对象。`value`属性表示当前的内部状态的值，是`yield`表达式后面那个表达式的值；`done`属性是一个布尔值，表示是否遍历结束。

#### yield 表达式

遍历器对象的`next`方法的运行逻辑如下。

（1）遇到`yield`表达式，就暂停执行后面的操作，并将紧跟在`yield`后面的那个表达式的值，作为返回的对象的`value`属性值。

（2）下一次调用`next`方法时，再继续往下执行，直到遇到下一个`yield`表达式。

（3）如果没有再遇到新的`yield`表达式，就一直运行到函数结束，直到`return`语句为止，并将`return`语句后面的表达式的值，作为返回的对象的`value`属性值。

（4）如果该函数没有`return`语句，则返回的对象的`value`属性值为`undefined`。

#### for...of 循环

`for...of`循环可以自动遍历 Generator 函数运行时生成的`Iterator`对象，且此时不再需要调用`next`方法。

```javascript
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```

上面代码使用`for...of`循环，依次显示 5 个`yield`表达式的值。这里需要注意，一旦`next`方法的返回对象的`done`属性为`true`，`for...of`循环就会中止，且不包含该返回对象，所以上面代码的`return`语句返回的`6`，不包括在`for...of`循环之中。

#### Generator.prototype.throw()

Generator 函数返回的遍历器对象，都有一个`throw`方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

#### Generator.prototype.return()

Generator 函数返回的遍历器对象，还有一个`return()`方法，可以返回给定的值，并且终结遍历 Generator 函数。

#### next()、throw()、return() 的共同点

`next()`、`throw()`、`return()`这三个方法本质上是同一件事，可以放在一起理解。它们的作用都是让 Generator 函数恢复执行，并且使用不同的语句替换`yield`表达式。

#### yield\* 表达式

如果在 Generator 函数内部，调用另一个 Generator 函数。需要在前者的函数体内部，自己手动完成遍历。

ES6 提供了`yield*`表达式，作为解决办法，用来在一个 Generator 函数里面执行另一个 Generator 函数。

`yield*`返回的是一个遍历器 `yield`返回的是具体的值

#### Generator 函数的`this`

Generator 函数总是返回一个遍历器，ES6 规定这个遍历器是 Generator 函数的实例，也继承了 Generator 函数的`prototype`对象上的方法。

Generator 函数也不能跟`new`命令一起用，会报错。

#### Generator 与状态机

Generator 是实现状态机的最佳结构。

```JavaScript
var clock = function* () {
  while (true) {
    console.log('Tick!');
    yield;
    console.log('Tock!');
    yield;
  }
};
```

#### Generator 与协程

协程（coroutine）是一种程序运行的方式，可以理解成“协作的线程”或“协作的函数”。协程既可以用单线程实现，也可以用多线程实现。前者是一种特殊的子例程，后者是一种特殊的线程。

Generator 函数是 ES6 对协程的实现，但属于不完全实现。Generator 函数被称为“半协程”（semi-coroutine），意思是只有 Generator 函数的调用者，才能将程序的执行权还给 Generator 函数。如果是完全执行的协程，任何函数都可以让暂停的协程继续执行。

如果将 Generator 函数当作协程，完全可以将多个需要互相协作的任务写成 Generator 函数，它们之间使用`yield`表达式交换控制权。

#### Generator 与上下文

JavaScript 代码运行时，会产生一个全局的上下文环境（context，又称运行环境），包含了当前所有的变量和对象。然后，执行函数（或块级代码）的时候，又会在当前上下文环境的上层，产生一个函数运行的上下文，变成当前（active）的上下文，由此形成一个上下文环境的堆栈（context stack）。

这个堆栈是“后进先出”的数据结构，最后产生的上下文环境首先执行完成，退出堆栈，然后再执行完成它下层的上下文，直至所有代码执行完成，堆栈清空。

Generator 函数不是这样，它执行产生的上下文环境，一旦遇到`yield`命令，就会暂时退出堆栈，但是并不消失，里面的所有变量和对象会冻结在当前状态。等到对它执行`next`命令时，这个上下文环境又会重新加入调用栈，冻结的变量和对象恢复执行。

#### 应用

异步操作的同步化表达

控制流管理

部署 Iterator 接口

作为数据结构

## Generator 函数的异步应用

#### 协程

协程有点像函数，又有点像线程。它的运行流程大致如下。

- 第一步，协程`A`开始执行。
- 第二步，协程`A`执行到一半，进入暂停，执行权转移到协程`B`。
- 第三步，（一段时间后）协程`B`交还执行权。
- 第四步，协程`A`恢复执行。

Generator 函数是协程在 ES6 的实现，最大特点就是可以交出函数的执行权（即暂停执行）。

#### Generator 函数的数据交换和错误处理

Generator 函数可以暂停执行和恢复执行，这是它能封装异步任务的根本原因。除此之外，它还有两个特性，使它可以作为异步编程的完整解决方案：函数体内外的数据交换和错误处理机制。

`next`返回值的 value 属性，是 Generator 函数向外输出数据；`next`方法还可以接受参数，向 Generator 函数体内输入数据。

#### co 模块

co 模块可以让你不用编写 Generator 函数的执行器。

Generator 函数只要传入`co`函数，就会自动执行。

co 模块其实就是将两种自动执行器（Thunk 函数和 Promise 对象），包装成一个模块。使用 co 的前提条件是，Generator 函数的`yield`命令后面，只能是 Thunk 函数或 Promise 对象。如果数组或对象的成员，全部都是 Promise 对象，也可以使用 co

## async 函数

#### 含义

async 函数是什么？一句话，它就是 Generator 函数的语法糖。

`async`函数就是将 Generator 函数的星号（`*`）替换成`async`，将`yield`替换成`await`，仅此而已。

`async`函数对 Generator 函数的改进，体现在以下四点。

（1）内置执行器。

Generator 函数的执行必须靠执行器，所以才有了`co`模块，而`async`函数自带执行器。也就是说，`async`函数的执行，与普通函数一模一样，只要一行。

（2）更好的语义。

`async`和`await`，比起星号和`yield`，语义更清楚了。`async`表示函数里有异步操作，`await`表示紧跟在后面的表达式需要等待结果。

（3）更广的适用性。

`co`模块约定，`yield`命令后面只能是 Thunk 函数或 Promise 对象，而`async`函数的`await`命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）

（4）返回值是 Promise。

`async`函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用`then`方法指定下一步的操作。

进一步说，`async`函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而`await`命令就是内部`then`命令的语法糖。

await 是微任务 因为返回得是 promise 对象

## Class 的基本语法

#### constructor() 方法

`constructor()`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。一个类必须有`constructor()`方法，如果没有显式定义，一个空的`constructor()`方法会被默认添加。

#### 类的实例

生成类的实例的写法，与 ES5 完全一样，也是使用`new`命令。

#### 实例属性的新写法

[ES2022](https://github.com/tc39/proposal-class-fields) 为类的实例属性，又规定了一种新写法。实例属性现在除了可以定义在`constructor()`方法里面的`this`上面，也可以定义在类内部的最顶层。

```javascript
class IncreasingCounter {
  _count = 0; //最顶层
  get value() {
    console.log("Getting the current value!");
    return this._count;
  }
  increment() {
    this._count++;
  }
}
class foo {
  bar = "hello";
  baz = "world";

  constructor() {
    // ...
  }
}
```

#### 取值函数（getter）和存值函数（setter）

与 ES5 一样，在“类”的内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

#### 属性表达式

类的属性名，可以采用表达式。

```javascript
let methodName = "getArea";

class Square {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
```

#### 静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```JavaScript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

#### 静态属性

静态属性指的是 Class 本身的属性，即`Class.propName`，而不是定义在实例对象（`this`）上的属性。

```javascript
class MyClass {
  static myStaticProp = 42;

  constructor() {
    console.log(MyClass.myStaticProp); // 42
  }
}
```

#### 私有方法和私有属性

[ES2022](https://github.com/tc39/proposal-class-fields)正式为`class`添加了私有属性，方法是在属性名之前使用`#`表示。

```JavaScript
class IncreasingCounter {
  #count = 0;
  get value() {
    console.log('Getting the current value!');
    return this.#count;
  }
  increment() {
    this.#count++;
  }
}
```

#### 类的注意点

##### 不存在提升

类不存在变量提升（hoist），这一点与 ES5 完全不同。

##### name 属性

`name`属性总是返回紧跟在`class`关键字后面的类名。

##### Generator 方法

##### this 的指向

类的方法内部如果含有`this`，它默认指向类的实例。

##### new.target 属性

`new`是从构造函数生成实例对象的命令。ES6 为`new`命令引入了一个`new.target`属性，该属性一般用在构造函数之中，返回`new`命令作用于的那个构造函数。如果构造函数不是通过`new`命令或`Reflect.construct()`调用的，`new.target`会返回`undefined`，因此这个属性可以用来确定构造函数是怎么调用的

## Class 的继承

Class 可以通过`extends`关键字实现继承，让子类继承父类的属性和方法。

`super`在这里表示父类的构造函数，用来新建一个父类的实例对象。

ES6 规定，子类必须在`constructor()`方法中调用`super()`，否则就会报错。这是因为子类自己的`this`对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，添加子类自己的实例属性和方法。如果不调用`super()`方法，子类就得不到自己的`this`对象。

在子类的构造函数中，只有调用`super()`之后，才可以使用`this`关键字，否则会报错。这是因为子类实例的构建，必须先完成父类的继承，只有`super()`方法才能让子类实例继承父类。

#### 私有属性和私有方法的继承

父类所有的属性和方法，都会被子类继承，除了私有的属性和方法。子类无法继承父类的私有属性，或者说，私有属性只能在定义它的 class 里面使用。

在父类中单独设置一个`getP()`是父类用来读取私有属性的方法，通过该方法，子类就可以读到父类的私有属性。

#### 静态属性和静态方法的继承

父类的静态属性和静态方法，也会被子类继承。静态属性是通过软拷贝实现继承的。

由于这种拷贝是浅拷贝，如果父类的静态属性的值是一个对象，那么子类的静态属性也会指向这个对象，因为浅拷贝只会拷贝对象的内存地址。

#### Object.getPrototypeOf()

`Object.getPrototypeOf()`方法可以用来从子类上获取父类。因此，可以使用这个方法判断，一个类是否继承了另一个类。

#### super 关键字

`super`这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

第一种情况，`super`作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次`super()`函数。

第二种情况，`super`作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

另外，在子类的静态方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类，而不是子类的实例。

#### 类的 prototype 属性和**proto**属性

（1）子类的`__proto__`属性，表示构造函数的继承，总是指向父类。

（2）子类`prototype`属性的`__proto__`属性，表示方法的继承，总是指向父类的`prototype`属性。

#### 实例的 **proto** 属性

子类实例的`__proto__`属性的`__proto__`属性，指向父类实例的`__proto__`属性。也就是说，子类的原型的原型，是父类的原型。

## Module 的语法

#### 概述

ES6 模块的设计思想是尽量的**静态化**，使得**编译**时就能确定模块的依赖关系，以及输入和输出的变量。**CommonJS** 和 **AMD** 模块，都只能在**运行**时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

ES6 模块还有以下好处。

- 不再需要`UMD`模块格式了，将来服务器和浏览器都会支持 ES6 模块格式。目前，通过各种工具库，其实已经做到了这一点。
- 将来浏览器的新 API 就能用模块格式提供，不再必须做成全局变量或者`navigator`对象的属性。
- 不再需要对象作为命名空间（比如`Math`对象），未来这些功能可以通过模块提供。

严格模式主要有以下限制。

- 变量必须声明后再使用
- 函数的参数不能有同名属性，否则报错
- 不能使用`with`语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀 0 表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量`delete prop`，会报错，只能删除属性`delete global[prop]`
- `eval`不会在它的外层作用域引入变量
- `eval`和`arguments`不能被重新赋值
- `arguments`不会自动反映函数参数的变化
- 不能使用`arguments.callee`
- 不能使用`arguments.caller`
- 禁止`this`指向全局对象,顶层的`this`指向`undefined`
- 不能使用`fn.caller`和`fn.arguments`获取函数调用的堆栈
- 增加了保留字（比如`protected`、`static`和`interface`）

#### export 命令

一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用`export`关键字输出该变量。

`export`命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。

```javascript
// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName, lastName, year };
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,// 可以使用as关键字重命名。
  v2 as streamV2,
  v2 as streamLatestVersion
};
```

`export`命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错，下一节的`import`命令也是如此。这是因为处于条件代码块之中，就没法做静态优化了，违背了 ES6 模块的设计初衷。

#### import 命令

使用`export`命令定义了模块的对外接口以后，其他 JS 文件就可以通过`import`命令加载这个模块。

`import`命令输入的变量都是只读的，因为它的本质是输入接口。也就是说，不允许在加载模块的脚本里面，改写接口。

```javascript
import { a } from "./xxx.js";

a = {}; // Syntax Error : 'a' is read-only;
a.foo = "hello"; // 合法操作
```

#### 模块的整体加载

除了指定加载某个输出值，还可以使用整体加载，即用星号（`*`）指定一个对象，所有输出值都加载在这个对象上面。

#### export 与 import 的复合写法

如果在一个模块之中，先输入后输出同一个模块，`import`语句可以与`export`语句写在一起。

```javascript
export { foo, bar } from "my_module";

// 可以简单理解为
import { foo, bar } from "my_module";
export { foo, bar };
```

#### 模块的继承

#### 跨模块常量

```javascript
// constants/db.js
export const db = {
  url: "http://my.couchdbserver.local:5984",
  admin_username: "admin",
  admin_password: "admin password",
};

// constants/user.js
export const users = ["root", "admin", "staff", "ceo", "chief", "moderator"];
```

#### import()

`import`和`export`命令只能在模块的顶层，不能在代码块之中

`require`是运行时加载模块，`import()`函数，支持动态加载模块。

`import()`类似于 Node.js 的`require()`方法，区别主要是前者是异步加载，后者是同步加载。

由于`import()`返回 Promise
对象，所以需要使用`then()`方法指定处理函数。考虑到代码的清晰，更推荐使用`await`命令。

下面是`import()`的一些适用场合。

（1）按需加载。

（2）条件加载

`import()`可以放在`if`代码块，根据不同的情况，加载不同的模块。

注意：

`import()`加载模块成功以后，这个模块会作为一个对象，当作`then`方法的参数。因此，可以使用对象解构赋值的语法，获取输出接口。

```JavaScript
import('./myModule.js')
.then(({export1, export2}) => {
  // ...·
});
```

同时加载多个模块，可以采用下面的写法。

```JavaScript
Promise.all([
  import('./module1.js'),
  import('./module2.js'),
  import('./module3.js'),
])
.then(([module1, module2, module3]) => {
   ···
});
```

`import()`也可以用在 async 函数之中。

```JavaScript
async function main() {
  const myModule = await import('./myModule.js');
  const {export1, export2} = await import('./myModule.js');
  const [module1, module2, module3] =
    await Promise.all([
      import('./module1.js'),
      import('./module2.js'),
      import('./module3.js'),
    ]);
}
main();
```

## Module 的加载实现

#### 传统方法

默认情况下，浏览器是同步加载 JavaScript 脚本，即渲染引擎遇到`<script>`标签就会停下来，等到执行完脚本，再继续向下渲染。如果是外部脚本，还必须加入脚本下载的时间。

如果脚本体积很大，下载和执行的时间就会很长，因此造成浏览器堵塞，用户会感觉到浏览器“卡死”了，没有任何响应。这显然是很不好的体验，所以浏览器允许<script>标签打开 defer 或 async 属性，脚本就会异步加载。渲染引擎遇到这一行命令，就会开始下载外部脚本，但不会等它下载和执行，而是直接执行后面的命令。本异步加载，下面就是两种异步加载的语法。

`defer`与`async`的区别是：`defer`要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），才会执行；`async`一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。一句话，`defer`是“渲染完再执行”，`async`是“下载完就执行”。另外，如果有多个`defer`脚本，会按照它们在页面出现的顺序加载，而多个`async`脚本是不能保证加载顺序的。

#### 加载规则

浏览器加载 ES6 模块，也使用`<script>`标签，但是要加入`type="module"`属性。

```javascript
<script type="module" src="./foo.js"></script>
```

浏览器对于带有`type="module"`的`<script>`，都是异步加载，不会造成堵塞浏览器，即等到整个页面渲染完，再执行模块脚本，等同于打开了`<script>`标签的`defer`属性。它们会按照在页面出现的顺序依次执行。

对于外部的模块脚本（上例是`foo.js`），有几点需要注意。

- 代码是在模块作用域之中运行，而不是在全局作用域运行。模块内部的顶层变量，外部不可见。
- 模块脚本自动采用严格模式，不管有没有声明`use strict`。
- 模块之中，可以使用`import`命令加载其他模块（`.js`后缀不可省略，需要提供绝对 URL 或相对 URL），也可以使用`export`命令输出对外接口。
- 模块之中，顶层的`this`关键字返回`undefined`，而不是指向`window`。也就是说，在模块顶层使用`this`关键字，是无意义的。
- 同一个模块如果加载多次，将只执行一次。

#### ES6 模块与 CommonJS 模块的差异

它们有三个重大差异。

- CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
- CommonJS 模块的`require()`是同步加载模块，ES6 模块的`import`命令是异步加载，有一个独立的模块依赖的解析阶段。

#### Node.js 的模块加载方法

CommonJS 模块是 Node.js 专用的，与 ES6 模块不兼容。语法上面，两者最明显的差异是，CommonJS 模块使用`require()`和`module.exports`，ES6 模块使用`import`和`export`。

## 编程风格

#### 块级作用域

**（1）let 取代 var**

**（2）全局常量和线程安全**

在`let`和`const`之间，建议优先使用`const`，尤其是在全局环境，不应该设置变量，只应设置常量。

`const`优于`let`有几个原因。一个是`const`可以提醒阅读程序的人，这个变量不应该改变；另一个是`const`比较符合函数式编程思想，运算不改变值，只是新建值，而且这样也有利于将来的分布式运算；最后一个原因是 JavaScript 编译器会对`const`进行优化，所以多使用`const`，有利于提高程序的运行效率，也就是说`let`和`const`的本质区别，其实是编译器内部的处理不同。

#### 字符串

静态字符串一律使用单引号或反引号，不使用双引号。动态字符串使用反引号。

#### 解构赋值

使用数组成员对变量赋值时，优先使用解构赋值。

函数的参数如果是对象的成员，优先使用解构赋值。

#### 对象

单行定义的对象，最后一个成员不以逗号结尾。多行定义的对象，最后一个成员以逗号结尾。

对象尽量静态化，一旦定义，就不得随意添加新的属性。如果添加属性不可避免，要使用`Object.assign`方法。

如果对象的属性名是动态的，可以在创造对象的时候，使用属性表达式定义。

#### 数组

使用扩展运算符（...）拷贝数组。

使用 Array.from 方法，将类似数组的对象转为数组。

#### 函数

立即执行函数可以写成箭头函数的形式。

#### Map 结构

注意区分 Object 和 Map，只有模拟现实世界的实体对象时，才使用 Object。如果只是需要`key: value`的数据结构，使用 Map 结构。因为 Map 有内建的遍历机制。

#### Class

总是用 Class，取代需要 prototype 的操作。因为 Class 的写法更简洁，更易于理解。

使用`extends`实现继承，因为这样更简单，不会有破坏`instanceof`运算的危险。

#### 模块

使用`import`取代`require()`。

其次，使用`export`取代`module.exports`。

如果模块只有一个输出值，就使用`export default`，如果模块有多个输出值，除非其中某个输出值特别重要，否则建议不要使用`export default`，即多个输出值如果是平等关系，`export default`与普通的`export`就不要同时使用。

如果模块默认输出一个函数，函数名的首字母应该小写，表示这是一个工具方法。

如果模块默认输出一个对象，对象名的首字母应该大写，表示这是一个配置值对象。

## 异步遍历器

#### 同步遍历器的问题

```JavaScript
function idMaker() {
  let index = 0;

  return {
    next: function() {
      return new Promise(function (resolve, reject) {
        setTimeout(() => {
          resolve({ value: index++, done: false });
        }, 1000);
      });
    }
  };
}
```

上面代码中，`next()`方法返回的是一个 Promise 对象，这样就不行，不符合 Iterator 协议，只要代码里面包含异步操作都不行。也就是说，Iterator 协议里面`next()`方法只能包含同步操作。

目前的解决方法是，将异步操作包装成 Thunk 函数或者 Promise 对象，即`next()`方法返回值的`value`属性是一个 Thunk 函数或者 Promise 对象，等待以后返回真正的值，而`done`属性则还是同步产生的。

```JavaScript
function idMaker() {
  let index = 0;

  return {
    next: function() {
      return {
        value: new Promise(resolve => setTimeout(() => resolve(index++), 1000)),
        done: false
      };
    }
  };
}

const it = idMaker();

it.next().value.then(o => console.log(o)) // 0
it.next().value.then(o => console.log(o)) // 1
it.next().value.then(o => console.log(o)) // 2
```

#### 异步遍历的接口

异步遍历器的最大的语法特点，就是调用遍历器的`next`方法，返回的是一个 Promise 对象。

由于异步遍历器的`next`方法，返回的是一个 Promise 对象。因此，可以把它放在`await`命令后面。

```JavaScript
asyncIterator
  .next()
  .then(
    ({ value, done }) => /* ... */
  );
```

#### for await...of

`for...of`循环用于遍历同步的 Iterator 接口。新引入的`for await...of`循环，则是用于遍历异步的 Iterator 接口。

#### 异步 Generator 函数

就像 Generator 函数返回一个同步遍历器对象一样，异步 Generator 函数的作用，是返回一个异步遍历器对象。在语法上，异步 Generator 函数就是`async`函数与 Generator 函数的结合。

普通的 async 函数返回的是一个 Promise 对象，而异步 Generator 函数返回的是一个异步 Iterator 对象。可以这样理解，async 函数和异步 Generator 函数，是封装异步操作的两种方法，都用来达到同一种目的。区别在于，前者自带执行器，后者通过`for await...of`执行，或者自己编写执行器。

异步 Generator 函数出现以后，JavaScript 就有了四种函数形式：普通函数、async 函数、Generator 函数和异步 Generator 函数。请注意区分每种函数的不同之处。基本上，如果是一系列按照顺序执行的异步操作（比如读取文件，然后写入新内容，再存入硬盘），可以使用 async 函数；如果是一系列产生相同数据结构的异步操作（比如一行一行读取文件），可以使用异步 Generator 函数。

## ArrayBuffer

## 装饰器

#### 简介（新语法

装饰器是一种函数，写成`@ + 函数名`，可以用来装饰四种类型的值。

- 类
- 类的属性
- 类的方法
- 属性存取器（accessor）

#### 装饰器 API（新语法

```JavaScript
type Decorator = (value: Input, context: {
  kind: string;
  name: string | symbol;
  access: {
    get?(): unknown;
    set?(value: unknown): void;
  };
  private?: boolean;
  static?: boolean;
  addInitializer?(initializer: () => void): void;
}) => Output | void;
```

装饰器函数有两个参数。运行时，JavaScript 引擎会提供这两个参数。

- `value`：所要装饰的值，某些情况下可能是`undefined`（装饰属性时）。
- `context`：上下文信息对象。

装饰器函数有两个参数。运行时，JavaScript 引擎会提供这两个参数。

- `value`：所要装饰的值，某些情况下可能是`undefined`（装饰属性时）。
- `context`：上下文信息对象。

装饰器函数的返回值，是一个新版本的装饰对象，但也可以不返回任何值（void）。

`context`对象有很多属性，其中`kind`属性表示属于哪一种装饰，其他属性的含义如下。

- `kind`：字符串，表示装饰类型，可能的取值有`class`、`method`、`getter`、`setter`、`field`、`accessor`。
- `name`：被装饰的值的名称: The name of the value, or in the case of private elements the description of it (e.g. the readable name).
- `access`：对象，包含访问这个值的方法，即存值器和取值器。
- `static`: 布尔值，该值是否为静态元素。
- `private`：布尔值，该值是否为私有元素。
- `addInitializer`：函数，允许用户增加初始化逻辑。

装饰器的执行步骤如下。

1. 计算各个装饰器的值，按照从左到右，从上到下的顺序。
2. 调用方法装饰器。
3. 调用类装饰器。

#### 类的装饰

```JavaScript
@testable
class MyTestableClass {
  // ...
}

function testable(target) {
  target.isTestable = true;
}

MyTestableClass.isTestable // true
//如果觉得一个参数不够用，可以在装饰器外面再封装一层函数。
function testable(isTestable) {
  return function(target) {
    target.isTestable = isTestable;
  }
}

@testable(true)
class MyTestableClass {}
MyTestableClass.isTestable // true

@testable(false)
class MyClass {}
MyClass.isTestable // false
```
