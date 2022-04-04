# js笔记

### new关键字

**new的作用:创建实例对象**

创建实例对象的过程:

1.创建一个空对象obj

2.将空对象的 _proto_ 属性指向构造函数的prototype属性

3.将空对象作为构造函数的this的上下文，执行构造函数

4.返回新对象

```js
function Fn(name){
    this.name = name
}

var f1 = new Fn('Jack')
       = function(name){
             var obj = {}
             obj.__proto__ = Fn.prototype
             Fn.call(obj, name)
             return obj                                 
         }('Jack')
```

说明:

1.创建一个新的空对象:var obj = {}

2.obj的proto属性指向Fn的prototype属性:obj.proto = Fn.prototype（形成原型链:obj.proto -->Fn.prototype --->Object.prototype --->null）

3.执行构造函数:Fn.call(obj,”jack“)

4.返回新的对象：return obj



### js的原型和原型链

**构造函数创建对象：**

```jsx
function Person() {

}
var person = new Person();
person.name = 'Kevin';
console.log(person.name) // Kevin
```

Person就是一个构造函数，我们使用new创建了一个实例对象person(可以查看new关键字的使用)

**prototype**

每一个**函数**都有一个prototype属性

每一个Javascript对象(null除外)在创建的时候就会与之关联另一个对象，这个对象就是我们所说的**原型**(原型对象)，每一个对象的都会从原型"继承"属性

```jsx
function Person() {

}
// 虽然写在注释里，但是你要注意：
// prototype是函数才会有的属性
Person.prototype.name = 'Kevin';
var person1 = new Person();
var person2 = new Person();
console.log(person1.name) // Kevin
console.log(person2.name) // Kevin
```

![](C:\Users\岑岑\Desktop\1.webp)

**proto**

每一个javascript对象(null除外)都具有的一个属性 proto，这个属性会指向该对象的原型

```jsx
function Person() {

}
var person = new Person();
console.log(person.__proto__ === Person.prototype); // true
```

![](C:\Users\岑岑\Desktop\2.webp)

**constructor**

每一个原型对象都有一个construtor属性指向关联的构造函数  实例原型指向构造函数

```tsx
function Person() {

}
console.log(Person === Person.prototype.constructor); // true
```

![](C:\Users\岑岑\Desktop\3.webp)

```jsx
function Person() {

}

var person = new Person();

console.log(person.__proto__ == Person.prototype) // true
console.log(Person.prototype.constructor == Person) // true
// 顺便学习一个ES5的方法,可以获得对象的原型
console.log(Object.getPrototypeOf(person) === Person.prototype) // true
```

**实例对象与原型对象**

```jsx
function Person() {

}

Person.prototype.name = 'Kevin';

var person = new Person();

person.name = 'Daisy';
console.log(person.name) // Daisy

delete person.name;
console.log(person.name) // Kevin
```

在这个例子中，我们给实例对象person添加了name属性,当我们打印person.name的时候，自然是实例对象上的name 所以为Daisy。

但是当我们删除了person的name属性时，读取person.name，从person对象中找不到name属性就会从person的原型上去找，也就是person.proto，也就是Person.prototype中查找，所以结果为Kevin

**原型与原型**

console.log(Object.prototype.__proto__ === null) // true

![](C:\Users\岑岑\Desktop\4.webp)

JavaScript 默认并不会**复制**对象的属性，相反，JavaScript 只是在两个对象之间创建一个**关联**，这样，一个对象就可以通过委托访问另一个对象的属性和函数，所以与其叫继承，**委托**的说法反而更准确些

### js中call()和apply()以及bind()的区别

**方法定义：**

apply()：调用一个对象的一个方法，用另外一个对象替换当前对象。例如B.apply(A,arg)即A对象调用B对象的方法

call()：调用一个对象的一个方法，用另外一个对象替换当前对象。例如B.call(A,args1,args2)即A对象调用B对象的方法

**call和apply的相同点：**

方法的含义和功能事一样的

第一个参数的作用是一样的

**call和apply的不相同点：**

call可以传入多个参数

apply只能传入两个参数，所以第二个参数往往是作为数组形式传入的

**存在的意义：实现(多重)继承**

call()，apply()，bind()都是用来重定义this这个对象

```javascript
var* name = '小张',
     age = 17;
var* obj = {
   name: '小王',
   objName: *this*.age,
   myFun: *function* () {
​    console.log(*this*.name + '年龄' + *this*.objName)
   }
  }
var* db = {
   name: '小绿',
   age: 99
  }

  obj.myFun.call(*this*, name, age)*//小张年龄17*
  obj.myFun.call(obj, name, age)*//小王年龄undefineds*
      
  obj.myFun.apply(*this*, [name, age])*//小张年龄17*
  obj.myFun.apply(obj, [name, age])*//小王年龄undefined*

  obj.myFun.bind(*this*)()*// 小张年龄undefined*

  obj.myFun.bind(obj)()*// 小王年龄17
```

**注意：**

call 的参数是直接放进去的，第二第三第n个参数全都用逗号分隔

apply的所有参数都必须放在一个数组里面传进去。

bind除了**返回函数**以外，它的参数和call 一样，需要执行新的函数还需要加一个()。





### js数组方法

##### find()

方法返回通过测试（**函数内判断**）的数组的**第一个**元素的值。

find() 方法为数组中的每个元素都调用一次函数执行：

- 当数组中的元素在**测试条件时返回 *true*** 时, find() 返回符合条件的元素，**之后的值不会再调用执行函数**。
- 如果没有符合条件的元素返回 undefined

**注意:** find() 对于空数组，函数是不会执行的。

**注意:** find() 并没有改变数组的原始值。

```javascript
array.find((item,index,arr)=>{})
```



##### findIndex()

findIndex() 方法返回传入一个测试条件（函数）符合条件的数组**第一个元素位置**。

findIndex() 方法为数组中的每个元素都调用一次函数执行：

- 当数组中的元素在测试条件时返回 *true* 时, findIndex() **返回符合条件的元素的索引位置**，之后的值不会再调用执行函数。
- 如果没有符合条件的元素返回 -1

**注意:** findIndex() 对于**空数组**，函数是不会执行的。

**注意:** findIndex() 并没有改变数组的原始值。

```javascript
array.findIndex((item,index,arr)=>{})
```



##### indexOf()

indexOf() 方法可返回数组中**某个指定的元素位置**。

该方法将从头到尾地检索数组，看它是否含有对应的元素。开始检索的位置在数组 start 处或数组的开头（没有指定 start 参数时）。如果找到一个 item，则返回 item 的第一次出现的位置。开始位置的索引为 0。

如果在数组中没找到指定元素则返回 -1。

```javascript
array.indexOf((item,index)=>{})
```

##### lastIndexOf()

lastIndexOf() 方法可返回**一个指定的元素在数组中最后出现的位置**，从该字符串的后面向前查找。

如果要检索的元素没有出现，则该方法返回 -1。

该方法将从尾到头地检索数组中指定元素 item。开始检索的位置在数组的 start 处或数组的结尾（没有指定 start 参数时）。如果找到一个 item，则返回 item 从尾向前检索第一个次出现在数组的位置。数组的索引开始位置是从 0 开始的。

如果在数组中没找到指定元素则返回 -1。

```javascript
array.lastIndexOf((item,index)=>{})
```

##### indcludes()

includes() 方法用来判断一个数组是否包含一个指定的值，如果是返回 true，否则false。

```javascript
arr.includes(searchElement)
arr.includes(searchElement, fromIndex)
```

##### filter()

filter() 方法**创建一个新的数组**，新数组中的元素是通过检查指定数组中符合条件的所有元素。

**注意：** filter() 不会对空数组进行检测。

**注意：** filter() 不会改变原始数组。

```javascript
array.filter((item,index)=>{})
```

