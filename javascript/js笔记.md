





## 面向对象

#### 1.创建对象：

###### **1.1  工厂模式** 

def:用函数来封装以特定接口创建对象的细节  notes:没有解决对象识别的我呢提(即怎么样知道一个对象的类型) 

```javascript
function createPerson(name,age,job){
var o = new Object();
o.name = name;
o.age= age;
o.job = job;
o.sayName = function(){
alert(this.name)
};
return o;
}
var person1 = createPerson('cai',24,'enginner');
var person2 = createPerson('jia',24,'enginner');
```

###### **1.2  构造函数模式**

```javascript
function Person(name,age,job){          //创建Person实例 必须使用new操作符 创建的过程：
this.name = name;                       // ① 创建一个新对象
this.age= age;                          // ② 将构造函数的作用域赋值给新对象(因此this就指向这个新对象)
this.job = job;                         // ③ 执行构造函数中的代码
this.sayName = function(){              // ④ 返回新对象
alert(this.name)
};                                     
} 
var person1 = new Person('cai',24,'enginner');// person1和person2的constructor都是指向Person
var person2 = new Person('jia',24,'enginner');//创建的实例 既是Person的实例又是Object 可采用instanceOf操作符检验
```

###### **1.3  原型模式**

def：每个函数都有prototype(原型属性)，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法；

```javascript
function Person(){};                           //
Person.prototype.name = 'cai';
Person.prototype.age = 24;
Person.prototype.job = "software";
Person.prototype.sayName = function(){
alert(this.name);
};
var person1 = new Person();
person1.sayName();//'cai'
var person2 = new Person();
person2.sayName();//cai
alert(person1.sayName = person2.sayName);//true
```

**Object.getPrototypeTypeOf()** —— 返回的对象实际就是这个对象的原型。

```javascript
alert(Object.getPrototypeTypeOf(person1) == Person.prototype) //true
alert(Object.getPrototypeTypeOf(person1).name)//'cai'
```

**isPrototypeOf()** ——确定对象之间是否存在关系

```javascript
alert(Person.prototype.isPrototypeof(person1));//true
alert(Person.prototype.isPrototypeof(person2));//true
```

**delete操作符** ——可以完全删除实例属性

**hasOwnProperty()** ——可以检测一个属性是存在于实例还是原型中

```javascript
alert(person1.hasOwnProperty('name'));//false  来自于原型
person1.name = 'gar';
alert(person1.hasOwnProperty('name'));//true   来自于实例
```

**Object.keys()** ——取得对象上所有的实例属性，接受一个对象作为参数，返回一个包含所有**可枚举**属性的字符串数组。

```javascript
var keys = Object.keys(Person.prototype);
alert(keys);//'name,age,job,sayName';//原型属性
```

**Object.getOwnPropertyNames()**——获取所有实例属性，无论它是否可枚举

```javascript
var keys = Object.getOwnPropertyNames(Person.prototype);
alert(keys);//'constructor,name,age,job,sayName' constructor是不可枚举属性
```

**in操作符**

单独使用和for-in循环中使用

```javascript
alert('name' in person1);//true   in 访问实例和原型
alert(person1.hasOwnProperty('name'));//false hasOwnProperty只访问实例
```

for-in 返回的是能够通过对象访问的，可枚举的属性，其中也包括存在于实例中的属性，也包括存在于原型中的属性。屏蔽了原型中不可枚举的属性(enumerable为false)

```javascript
var person = new Object();
person.name = 'cai';
person.age = 23;
person.sayName = function(){
console.log(this.name)
}
```

######  **1.4  组合使用构造函数模式和原型模式**

**原型对象的问题**

```javascript
function Person(){};
Person.prototype = {
constructor:Person,
name:'cai',
age:23,
friends:['sei,jia,mei'],
sayName:function(){
alert(this.name)
}
};
var person1 = new Person();
var person2 = new Person();
person1.friends.push('van');
alert(person1.friends);//'sei,jia,mei,van'  // person1和person2的friends都是指向同一个数组
alert(person2.friends);//'sei,jia,mei,van'
alert(person1.friends == person2.friends);//true 
```

###### **混合模式** （最广泛，认同度最高得方法 默认模式）

```javascript
function Person(name,age){
this.name = name;
this.age = age;
this.friends = ['jia,li']
};
Person.prototype = {
constructor:Person,
sayName:function(){
alert(this.name)
}
};
var person1 = new Person('cai',24);
var person2 = new Person('mei',22);

person1.friends.push('van');
alert(person1.friends);//'jia,li,cai ,van'  
alert(person2.friends);//'jia,li,mei'      
alert(person1.friends == person2.friends);//false
alert(person1.sayName == person2.sayName);//true 
```

######  **1.5  动态原型模式**

 不能使用对象字面量重写原型 这样会切断现有实例与新原型之间的联系。

```javascript
function(name,age,job){
this.name = name;
this.age =age;
this.job = job;
 if(typeOf this.sayName != "function"){
 Person.prototype.sayName = function (){
 alert(this.name)
 }
 }
}
```

###### **1.6  寄生构造函数模式**

很像典型的构造函数

```javascript
function Person(name,age,job){
var o = new Object();
o.name = name;
o.age= age;
o.job = job;
o.sayName = function(){
alert(this.name)
};
return o;
}
var person1 = Person('cai',24,'enginner');
person1.sayName();//'cai'
```

###### **1.7 稳妥构造函数模式**

```javascript
function Person(name,age,job){
//创建需要返回的对象
var o = new Object();
//可以在这里定义私有变量和函数
//添加方法
o.sayName = function(){
alert(name)
}
return o;
}
```

###### **1.8  字面量语法**

```javascript
var person = {
name:'cai',
age:23,
sayName:function(){
console.log(this.name)
}
}
```

------



#### 2.对象属性类型

分为**数据属性**和**访问器属性**

###### 2.1 **数据属性**

它包含一个数据之的位置。

**Configurable** ：表示能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。默认值为true。

**Enumerable**：表示能否通过for-in循环返回属性。默认值为true。

**WriteAble**：表示能否修改属性的值。默认值为true。

**Value**：包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性的时候，把新的值保存在这个位置。默认值为true。

**Object.defineProperty()** —— 修改属性默认的特性。三个参数：属性所处的对象、属性的名字和一个描述符对象。描述符对象的属性必须包含数据属性的一个或多个，可以修改对应的特征性。

**notes**：可以多次调用Object.defineProperty()方法修改同一属性，但是在把configurable设置为false之后就会有限制。使用该方法创建一个新的属性时，如果不指定configurable、enumerable和writeable特性的默认值都为false。

```javascript
var person ={};
Object.deineProperty(person,'name',{
configurable:false,
value:'cai'
});
alert(person.name);//'cai'
delete person.name;//因为configurable为false 所以delete不能删除属性
aler(person.name);//'cai'
```

###### **2.2访问器属性**

访问器属性不包含数据值；它们包含一对getter和setter函（非必须），在读取访问器属性时，会调用getter函数，负责返回有效值；写入访问器属性时，会调用setter函数并传入新值，负责决定如何处理数据。

**Configurable**：表示能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为shu属性。默认值为true。

**Enumerable**：表示能否通过for-in循环返回属性。默认值为true。

**Get：**在读取属性时调用的函数。默认值为undefined。

**Set：**在写入属性时调用的函数。默认值为undefined。

```javascript
var book ={
year:2004,
edition:1
}
Object.definProperty(book,'year',{
get:function(){
return this.year;
},
set:function(newValue){
if(newValue>2004){
this.year = newValue;
this.edition +=newValue -2004;
}
}
});
book.year = 2005;
alert(book.edition);//2
```

**note:**访问器属性不能直接定义，必须使用Objec.defineProperty()来定义。

**定义多个属性**

**Object.defineProperties() **——  可以定义一个对象的多个属性。两个参数：第一个对象是要添加和修改其属性的对象，第二个对象的属性与第一个对象中要添加或修改的属性一一对应

```javascript
var book = {};
Object.defineProperty(book,{
year:{
writable:true,
value:2004
},
edition:{
writable:true,
value:1
},
year:{
get:function(){
return this.year;
},
set:function(newValue){
if(newValue>2004){
this.year = newValue;
this.edition += newValue - 2004;
}
}
}
})
```

**读取属性的特性**

Object.getOwnProperDescriptor() ------可以取得给定属性的描述。接受两个参数：属性所在的对象和要读取其描述符的属性名称。返回值是一个对象，如果是访问器属性，这个对象的属性有 configurable，enumerable，get和set；如果是数据属性，这个对象的属性有configurable，enumrable，writable和value。

```javascript
var book = {};
Object.defineProperty(book,{
_year:{
writable:true,
value:2004
},
edition:{
writable:true,
value:1
},
year:{
get:function(){
return this.year;
},
set:function(newValue){
if(newValue>2004){
this.year = newValue;
this.edition += newValue - 2004;
}
}
}
});

var descriptor = Object.getOwnPropetyrDescriptor(book,'_year');
alert(descriptor.value);//2004
alert(descriptor.configurable);//false
alert(typeOf descriptor.get);//'undefined'

var descriptor = Object.getOwnPropertyDescriptor(book,'year');
alert(descriptor.value);//undefined
alert(descriptor.enumerable);//false
alert(typeOf descriptor.get);//function


```



------

#### 3.  继承

###### 3.1 原型链

基本思想:利用原型让一个引用类型继承另一个引用类型的属性和方法。

构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。

原型链：让原型对象等于另一个类型的实例，此时的原型对象将包含一个指向另一个原型的指针，相应的另一个原型中也包含着一个指向另一个构造函数的指针。

```javascript
function SuperType(){
this.property = true;
}
superType.prototype.getSuperValue = function(){
return this.property;
}
function SubType(){
this.subproperty = false;
}
//继承SuperType
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function(){
    return this.subproperty;
};
var instance = new SubType();
alert(instance.getSubValue);//true
```

![原型链](C:\Users\穿花裤衩的岑岑\Desktop\notebook\notebook\img\原型链.webp)

原型链存在的问题：

第一：引用类型值得原型属性会被所有实例共享

```javascript
function SperType(){
this.colors = ['red','blue','green'];
}
function SubType(){
    
};

//继承了SuperType
SubType.prototype = new SuperType();
var instance1 = new SubType();
instance1.colors.push('black');
alert(instance.colors); //'red,blue,green,black'

var instance2 = new SubType();
alert(instance2.colors);//'red,blue,green,black'
```

第二：在创建子类型得实例时，不能向超类型得构造函数传递参数

###### 3.2 借用构造函数

```javascript
function SperType(){
this.colors = ['red','blue','green'];
}
function SubType(){
    //继承了SuperType
    SuperType.call(this)
};
var instance1 = new SubType();
instance1.colors.push('black');
alert(instance.colors); //'red,blue,green,black'

var instance2 = new SubType();
alert(instance2.colors);//'red,blue,green'
```

传递参数：借用构造函数有一个很大得优势，即可以在子类型构造函数中向超类型构造函数传递参数。

```javascript
function SuperType(name){
this.name = name;
}
function SubType(){
//继承了SuperType,同时还传递了参数
SuperType.call(this,"cai");
//实例属性
this.age = 29
}

var instance = new SubType();
alert(instance.name);//'cai'
alert(instance.age);//29
```



###### 3.3 组合继承

有时候也叫做伪经典继承，指的是将原型链和借用构造函数得技术组合到一块，从而发挥二者之长的一种继承模式。其背后思路就是使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。

```javascript
function SuperType(name){
this.name = name;
this.colors = ['red','blue','green']
}
SuperType.prototype.sayName = function(){
alert(this.name)
}
function SubType(name.age){
//继承属性
SuperType.call(this,name);
this.age = age
}
//继承方法
SubType.protoType = new SuperType();
SubType.protoType.constructor = SubType;
SubType.protoType.sayAge = function(){
alert(this.age)
};
var instance1 = new SubType('cai',23);
instance1.colors.push('black');
alert(instance1.colors);//'red,blue,green,black'
instance1.sayName();//'cai'
instance1.sayAge();//23

var instance2 = new SubType('jia',23);
alert(instance1.colors);//'red,blue,green'
instance1.sayName();//'jia'
instance1.sayAge();//23

```

· 将SubType的原型指定为SuperType的一个实例，大致步骤和原型链继承类似，只是多了在SubType中借调SuperType的过程。

· 实例属性定义在构造函数中，而方法则定义在构造函数的新原型中，同时将新原型的constructor指向构造函数。

· 可以通过instanceof和isPrototypeOf()来识别基于组合继承创建的对象。

· 避免了原型链和借用构造函数的缺陷，融合了它们的优点，成为JS中最常用的继承模式。

实际上是借用了构造函数，以覆盖的方式，解决了在原型链继承中原型的引用类型属性共享在所有实例中的问题。

因为在子类型中借调构造函数(SuperType.call(this))时，会在自己的所有实例中执行一遍SuperType中的代码，由于每个实例this都是不同的，因此SuperType中定义的属性会在每个实例中有一份副本，也就避免了原型链继承中，原型属性共享的问题（覆盖了原型属性）。


###### 3.4 原型式继承

一种实现继承的方法: 借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型。

```javascript
function object(o){
 function F(){}
 F.prototype = o;
 return new F();
}

var person = {
 name: "Nicholas",
 friends: ["Shelby", "Court", "Van"]
};

var anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");
alert(person.friends); //"Shelby,Court,Van,Rob,Barbie" 
```

> 在 object()函数内部，先创建了一个临时性的构造函数，然后将传入的对象作为这个构造函数的原型，最后返回了这个临时类型的一个新实例。从本质上讲，object()对传入其中的对象执行了一次浅复制

- 这种原型式继承，要求必须要有一个对象可以作为另一个对象的基础
- 用这种方式创建的对象相当于是传入参数对象的副本

> ES5新增了`Object.create()`方法规范化了原型式继承。这个方法接受两个参数，一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象。在传入一个参数的情况下，`Object.create()`与`object()`方法的行为相同。

- `Object.create()`方法的第二个参数与`Object.defineProperties()`方法的第二个参数格式相同：每个属性都是通过自己的描述符定义的。以这种方式指定的任何属性都会覆盖原型对象上的同名属性。
- IE8及以下不支持`Object.create()`方法
- 在只想让一个对象与另一个对象保持类似的情况下，原型继承是完全可以胜任的。不过别忘了，原型模式下的缺点：引用类型属性的共享问题。

###### 3.5 寄生式继承

寄生式继承与原型式继承紧密相关，与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再返回对象。

```javascript
function createAnother(original) {
	var clone = object(original); //通过调用函数创建一个新对象
	clone.sayHi = function(){		// 以某种方式来增强这个对象
		alert("hi");
	};
	return clone;			// 返回这个对象
}
```

- 在主要考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式。

###### 3.6 寄生组合式

#### 4.对象的相关方法

###### Object.create

该方法接受一个对象作为参数，然后以它为原型，返回一个实例对象。该实例完全继承原型对象的属性。

###### Object.getPrototypeOf

返回参数对象的原型。这是获取原型对象的标准方法。

###### Object.setPrototypeOf

`Object.setPrototypeOf`方法为参数对象设置原型，返回该参数对象。它接受两个参数，第一个是现有对象，第二个是原型对象。

###### Object.prototype.isPrototypeOf()

实例对象的`isPrototypeOf`方法，用来判断该对象是否为参数对象的原型。

###### Object.prototype.__proto__

实例对象的`__proto__`属性（前后各两个下划线），返回该对象的原型。该属性可读写。

###### Object.getOwnPropertyNames()

`Object.getOwnPropertyNames`方法返回一个数组，成员是参数对象本身的所有属性的**键名**，不包含继承的属性键名。

###### Object.prototype.hasOwnProperty()

对象实例的`hasOwnProperty`方法返回一个布尔值，用于判断某个属性定义在对象自身，还是定义在原型链上。

###### in 运算符和 for...in 循环

`in`运算符返回一个布尔值，表示一个对象是否具有某个属性。它不区分该属性是对象自身的属性，还是继承的属性。

`for...in`获得对象的所有可遍历属性（不管是自身的还是继承的）



------



## Array类型

###### 1. 检测数组

**instanceOf** 操作符

```javascript
value instanceOf Array;
```

**Array.isArray()**

```javascript
Array.isArray(value);
```

###### 2. 转换方法

**toLocaleString()、toString()、valueOf()**

###### 3. 栈方法（后进先出）

**push()** ——可以接受任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度

```javascript
var colors = new Array();
var count = colors.push('red','green');//推入两项
alert(count);//2
count = colors.push('black');
alert(count);//3 返回的是数组被修改后的长度
```

**pop()** ——从数组的末尾移除最后一项，减少数组长度length，然后返回移除的项。

```javascript
var item = colors.pop();//取得最后一项
alert(item); //"black"
alert(colors.length);//2
```

###### 4. 队列方法（先进先出）

**shift()** —— 移除数组中的第一个项并返回该项，同时将数组长度减1.

```javascript
var item = colors.shift();// colors = ['red','green',black]
alert(item);//'red'
alert(colors/length);//2
```

**unshift() **—— 与shift的用途相反，它能在数组前端添加任意个项并返回新数组的长度。

```javascript
var count = colors.unshift('red','green');//推入两项
colors.unshift('black');
alert(colors);//['black','red','green'];
```

###### **5. 重排序方法 **

**reverse()** —— 反转数组项的顺序,返回被排序后的数组

```javascript
var values =[1,2,3,4,5];
values.reverse();//[5,4,3,2,1]
```

**sort()** —— 默认情况下，采用升序排列数组项，它会调用每个数组项的toString()转型方法，然后比较得到的字符串，以确定如何排序。接收一个比较函数作为参数，以便指定那个值在哪个值得前面。

```javascript
function compare(value1,value2){
if(value1 < value2){
return -1
}else if(value > value2){
return 1 
}else{
return 0
}
}
var values = [0,1,5,10,15];
values.sort(compare);//0,1,5,10,15
```

###### **6. 操作方法**

**concat()** —— 基于当前数组中得所有项创建一个新数组，首先会创建当前数组一个副本，然后接收到得参数添加到这个副本得末尾，并返回新构建得数组。并不修改原数组！

```javascript
var colors = ['red','grenn','black'];
var colors2 = colors.concat('yellow',['black','brown']);
alert(colors);//red,green,blue
alert(colors2);//red,green,blue,yellow,black,brown
```

**slice()** —— 基于当前数组的一个或多个项创建一个新数组，可以接收一或两个参数，即返回项的起始和结束位置。在只有一个参数的情况下，slice()方法返回从该指定位置开始到当前数组末尾的所有项。如果是两个参数，则包含起始到与结束位置之间的项，但不包括结束位置的项。不会影响原数组！

```javascript
var colors = ['red','green','blue','yellow','purple'];
var colors2 = colors.slice(1);//green,blue,yellow,purple
var colors3 = colors.slice(1,4);//green,blue,yellow
```

**splice()** ——主要向数组的中部插入项。

**删除** ——可以删除任意数量的项，只需指定2个参数：要删除的第一项的位置和要删除的项数 

```javascript
var colors = ['red','grenn','black'];
var remove = colors.splice(0,1);//'red'
alert(colors);//'green','blacke'
```

**插入** —— 可以指定位置插入任意数量的项，只需提供3个参数：起始位置，0(要删除的项数)和要插入的项。

```javascript
removed = colors.splice(1,0,'yellow','orange');//从位置1开始插入项
alert(colors);//green,yellow,orange,blue
alert(removed);//返回的是一个空数组
```

**替换** —— 可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3个 参数：起始位置，要删除的项和要插入的项的任意数量的项。插入的项和删除的项数不必相等。

```javascript
removed = colors.splice(1,1,'red','purple');//插入两项，删除一项
alert(colors);//green,red,purple,orange,blue
alert(removed);//yellow 返回数组中删除的项
```

**notes**:splice()方法始终都会返回一个数组，该数组中包含从原数组中删除的项（如果没有删除任何项，则返回一个空数组）

###### 7. 位置方法

**indexOf()** —— 从数组的开头(位置0)开始向后查找，接收两个参数：要查找的项和（可选的）表示查找起点位置的索引,返回要查找项在数组中的位置索引。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
alert(numbers.indexOf(4));//3
```

**lastIndexOf()** —— 从数组的末尾开始向前查找

```javascript
alert(numbers.lastIndexOf(4));//5
```

###### 8. 迭代方法

**every()** —— 对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var everyresult = numbers.every((item,index,array)=>{
return(item >2)
})
alert(everyResult);//false
```

**some() **—— 对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true。

```javascript
var someresult = numbers.some((item,index,array)=>{
return(item >2)
})
alert(someresult);//true
```

**fliter() **—— 对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。

```javascript
var fliterResult = numbers.fliter((item,index,array)=>{
return(item >2)
})
alert(fliterResult);//[3,4,5,4,3]
```

**map()** —— 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。

```javascript
var mapResult = numbers.map((item,index,array)=>{
return item*2;
})
alert(mapResult);//[2,4,6,8,10,8,6,4,2]
```

**forEach()** —— 对数组中的每一项运行给定函数，没有返回值

```javascript
numbers.forEach((item,index,array)=>{
//执行某些操作
})
```

**notes:** 这些方法均不会修改原数组

###### 9. 归并方法

迭代数组的所有项，然后构建一个最终返回值。这两个方法接收两个参数，一个在每项上调用的函数和(可选)作为归并基础的初始值。函数接收4个参数：前一个值，当前值，项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数就是数组的第二项

**reduce()** —— 从第一项开始，逐个遍历到最后一项。

**reduceRight()** —— 从数组的最后一项开始，向前遍历到第一项。

```javascript
var values = [1,2,3,4,5];
var sum = values.reduce((pre,cur,index,array)=>{
return pre + cur
})
alert(sum);//15

var sum2 = values.reduceRight((pre,cur,index,array)=>{
return pre + cur
});
alert(sum);//15
```





## 函数

定义函数有两种方式：一种是函数声明，另一种是函数表达式

notes:只有函数声明才会有函数声明提升

###### 1.1 变量提升

```javascript
console.log(name) // undefined
var name = 'Sunshine_Lin'

if (false) {
  var age = 23
}
console.log(age) // undefined 不会报错


//面试题
console.log(v1);
var v1 = 100;
function foo(){
    console.log(v1);
     var v1 = 100;
    console.log(v1)
}
foo();
console.log(v1);
//undefiend
//undefiend
//200
//100
```

###### 1.2 函数提升

```javascript
console.log(fun) // function fun() {}
function fun() {}

if (false) {
  function fun2(){}
}
console.log(fun2) // undefined 不会报错
```

- 函数提升优先级 > 变量提升优先级

```javascript
console.log(fun) // function fun() {}
var fun = 'Sunshie_Lin'
function fun() {}
console.log(fun) // 'Sunshie_Lin'
```

###### 1.3 闭包

**定义**：一个闭包是一个可以自己拥有独立的环境与变量的的表达式（通常是函数，因为ES6有了块级作用域的概念），是指有权访问另一个函数作用域中的变量的函数。也就是说，闭包让你可以在一个内层函数中访问到其外层函数的作用域。

闭包其实只是一个绑定了**执行环境的函数**

在 JavaScript 中，根据词法作用域的规则，**内部函数总是可以访问其外部函数中声明的变量，当通过调用一个外部函数返回一个内部函数后，即使该外部函数已经执行结束了，但是内部函数引用外部函数的变量依然保存在内存中，我们就把这些变量的集合称为闭包**

1. 可以在函数的外部访问到函数内部的局部变量。 
2. 让这些变量始终保存在内存中，不会随着函数的结束而自动销毁。

```javascript
function init() {
  var name = "Mozilla"; // name 是一个被 init 创建的局部变量
  function displayName() { // displayName() 是内部函数，一个闭包
      alert(name); // 使用了父函数中声明的变量
  }
  displayName();
}
init();
```

###### 小结

匿名函数，也称为拉姆达函数。

函数表达式的特点：

函数表达式不同于函数声明。函数声明要求有名字，但函数表达式不需要。没有名字的函数也称为匿名函数。

在无法确定如何引用函数的情况下，递归函数就会变得比较复杂。

递归函数应该始终使用arguments.callee来递归地调用自身，不需要使用函数名——函数名可能会发生变化。

闭包的原理：

在后台执行环境中，闭包的作用域链包含着它自己的作用域，包含函数的作用域和全局作用域。

通常，函数的作用域及其所有变量都会在函数执行后结束后被销毁。

但是，当函数返回了一个闭包时，这个函数的作用域将会一直存在内存中保存到闭包不存在为止。使用闭包可以在javascript中模仿块级作用域，要点如下：

创建并立即调用一个函数，这样既可以执行其中的代码，又不会在内存中留下对该函数的引用。

结果就是函数内部的所有变量都会被立即销毁——除非将某些变量赋值给包含作用域(即外部作用域)中的变量。

闭包还可以在对象中创建私有变量，相关概念如下：

即使javascript中没有正式的私有对象属性的概念，但可以使用闭包来实现公有方法，而通过共有方法可以访问在包含作用域中定义的变量。

有权访问私有变量的公有方法叫做特权方法。

可以使用构造函数模式，原型模式来实现自定义类型的特权方法，也可以使用模块模式，增的模块模式来实现单例的特权方法。

过度使用闭包会占用大量内存。









## BOM

#### 1.window对象

###### 1.1  全局作用域 

在全局作用域中声明的变量，函数都会成为window对象的属性和方法。

```javascript
var age = 24;
function sayAge(){
alert(this.age);
}
alert(window.age);//24
sayAge();//24;
window.sayAge();//24
```

###### 1.2 窗口关系及其框架



###### 1.3 窗口位置

screenLeft——窗口相对于屏幕左边的位置

screenTop——窗口相对于屏幕上边的位置

screenX

screenY  

###### 1.4 窗口大小

innerWidth

innerHeight

outerWidth

outerHeight

document.body.clientWidth

document.body.clientHeight

和调整浏览器窗口大小

resizeTo() —— 两个参数：浏览器窗口的新宽度和高度

resizeBy() —— 两个参数：新窗口和原窗口高度之差

###### 1.5 导航和打开窗口

window.open() —— 导航到一个特定的URL，也可以打开一个新的浏览器窗口。4个参数：要加载的URL，窗口目标，一个特定的字符串以及一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值。

alert()

confim

###### 1.6 间歇调用和超时调用

```javascript
//超时调用 
var timeout = setTimeout(function(){

alert('hello world!')

},1000);

clearTimeout(timeout);

```

```javascript
//间歇定时器
num = 0;
max = 10;
var interval = null;

var interval = setInterval(function(){
num++;
if(num ==max){
clearInterval(interval)
}
},500)
```

###### 1.7 系统提示框

alert()、config()、prompt()

#### 2.  location对象

###### Location 对象属性

| 属性                                                         | 描述                                          |
| :----------------------------------------------------------- | :-------------------------------------------- |
| [hash](https://www.w3school.com.cn/jsref/prop_loc_hash.asp)  | 设置或返回从井号 (#) 开始的 URL（锚）。       |
| [host](https://www.w3school.com.cn/jsref/prop_loc_host.asp)  | 设置或返回主机名和当前 URL 的端口号。         |
| [hostname](https://www.w3school.com.cn/jsref/prop_loc_hostname.asp) | 设置或返回当前 URL 的主机名。                 |
| [href](https://www.w3school.com.cn/jsref/prop_loc_href.asp)  | 设置或返回完整的 URL。                        |
| [pathname](https://www.w3school.com.cn/jsref/prop_loc_pathname.asp) | 设置或返回当前 URL 的路径部分。               |
| [port](https://www.w3school.com.cn/jsref/prop_loc_port.asp)  | 设置或返回当前 URL 的端口号。                 |
| [protocol](https://www.w3school.com.cn/jsref/prop_loc_protocol.asp) | 设置或返回当前 URL 的协议。                   |
| [search](https://www.w3school.com.cn/jsref/prop_loc_search.asp) | 设置或返回从问号 (?) 开始的 URL（查询部分）。 |

###### Location 对象方法

| 属性                                                         | 描述                     |
| :----------------------------------------------------------- | :----------------------- |
| [assign()](https://www.w3school.com.cn/jsref/met_loc_assign.asp) | 加载新的文档。           |
| [reload()](https://www.w3school.com.cn/jsref/met_loc_reload.asp) | 重新加载当前文档。       |
| [replace()](https://www.w3school.com.cn/jsref/met_loc_replace.asp) | 用新的文档替换当前文档。 |



## DOM

###### 1.1 层次节点



###### 1.2 Node类型

每个节点nodeType











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
             var Fn = {}
             obj.__proto__ = Fn.prototype
             Fn.call(obj, name)
             return Fn                                 
         }('Jack')
```

说明:

1.创建一个新的空对象:var obj = {}

2.obj的proto属性指向Fn的prototype属性:obj.proto = Fn.prototype（形成原型链:obj.proto -->Fn.prototype --->Object.prototype --->null）

3.将新对象obj赋值给函数内部的this关键字 执行构造函数:Fn.call(obj,”jack“)

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



### this关键字



**概念：**this就是this指针，**指向我们所调用函数的对象**；this是javascript中的一个关键字，它是函数运行时，在函数体内自动生成的一个**对象**，只能在函数体内部使用。



1.全局环境中的this指向全局变量window。

2.函数中的this，由调用函数的方式来决定，

(1) 如果函数时独立调用的，在严格模式下是指向undefined的，在非严格模式下，指向window。

(2)如果这个函数被某一个对象调用，那么this指向被调用的对象;

3.构造函数和原型里面的this

构造函数中的this以及原型里的this对象指的都是生成的实例；(由new决定)

通过new操作符可以初始化一个constructor的指向，new的作用就是创建一个对象的实例，constructor也就是指向了新的执行环境"在这个对象之中"；

4 箭头函数按词法作用域来绑定它的上下文，所以this实际上会引用到原来的上下文。

（箭头函数会保持它当前执行上下文的词法作用域不变，而普通函数则不会，箭头函数从包含它的词法作用域中继承了this的值）。

5 注意数组处理方法中的this 这个this实际上是指向window

绑定this指向问题 call apply bind

### js的作用域和作用域链

**作用域**

在 Javascript 中，作用域分为 **全局作用域** 和 **函数作用域**

**全局作用域**：代码在程序的任何地方都能被访问，window 对象的内置属性都拥有全局作用域。

**函数作用域：**在固定的代码片段才能被访问

```javascript
 var a = 10 ,*//全局作用域*
  b = 20;

  function fn(){
   var a = 100,*//fn作用域*
   c = 3;
      
   function bar(){
​    var a = 1000,*//bar作用域*
​    d = 40000
   }
  }
```

作用域有上下级关系，上下级关系的确定就看这个函数在哪个作用域下创建的，fn的作用域下创建了bar函数，那么fn作用域就是bar作用域的上级

作用域最大的作用就是隔离变量，不同的作用域下同名的变量不会有冲突

变量取值：到创建 这个变量的函数的作用域中取值

**作用域链**

一般情况下，变量取值到创建这个变量的函数的作用域中取值

但是如果当前的作用域中没有查到值，就会向上一级的作用域去查，直到查到全局作用域，这个查找的过程形成的链条就是作用域链

### 重排和重绘

重排：元素的位置发生改动时发生重排，也叫回流。此时在关键渲染路径中的 Layout 阶段，计算每一个元素在设备视口内的确切位置和大小。当一个元素位置发生变化时，其父元素及其后边的元素位置都可能发生变化，代价极高

重绘：元素的样式发生变动，但是位置没有改变。此时在关键渲染路径中的 Paint 阶段，将渲染树中的每个节点转换成屏幕上的实际像素，这一步通常称为绘制或栅格化

### 深拷贝和浅拷贝

https://segmentfault.com/a/1190000021239893

**1、JavaScript变量的类型分为两种： 基本类型和引用类型。**

- 基本类型是指简单的数据段，有5种类型： `Undefined`、`Null`、`Boolean`、`Number` 和`String`
- 引用类型是指可能有多个值构成的对象，一般为： `Object`、 `Array`、 `function` 等

浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。引用传递 (指针)

深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象。值传递

**浅拷贝的实现方式**

Object.assign()

使用拓展运算符实现浅拷贝

**深拷贝的实现方式**

JSON.parse(JSON.stringify())实现

Object.create()实现

### instanceof

在判断一个对象的原型是null的时候会失效 比如Object.create(null)

### 浏览器模型

浏览器的核心是两部分：**渲染引擎**和 **JavaScript 解释器**（又称 JavaScript 引擎

**渲染引擎工作机制**

1. 解析代码：HTML 代码解析为 DOM，CSS 代码解析为 CSSOM（CSS Object Model）。
2. 对象合成：将 DOM 和 CSSOM 合成一棵渲染树（render tree）。
3. 布局：计算出渲染树的布局（layout）。
4. 绘制：将渲染树绘制到屏幕。

以上四步并非严格按顺序执行，往往第一步还没完成，第二步和第三步就已经开始了。所以，会看到这种情况：网页的 HTML 代码还没下载完，但浏览器已经显示出内容了。

 **JavaScript 引擎**

1. 读取代码，进行词法分析（Lexical analysis），将代码分解成词元（token）。
2. 对词元进行语法分析（parsing），将代码整理成“语法树”（syntax tree）。
3. 使用“翻译器”（translator），将代码转为字节码（bytecode）。
4. 使用“字节码解释器”（bytecode interpreter），将字节码转为机器码。

**`script`**工作原理

1. 浏览器一边下载 HTML 网页，一边开始解析。也就是说，不等到下载完，就开始解析。
2. 解析过程中，浏览器发现`<script>`元素，就暂停解析，把网页渲染的控制权转交给 JavaScript 引擎。
3. 如果`<script>`元素引用了外部脚本，就下载该脚本再执行，否则就直接执行代码。
4. JavaScript 引擎执行完毕，控制权交还渲染引擎，恢复往下解析 HTML 网页。

为了解决阻塞渲染问题，script元素使用defer或async属性

**`defer`属性**

1. 浏览器开始解析 HTML 网页。
2. 解析过程中，发现带有`defer`属性的`<script>`元素。
3. 浏览器继续往下解析 HTML 网页，同时并行下载`<script>`元素加载的外部脚本。
4. 浏览器完成解析 HTML 网页，此时再回过头执行已经下载完成的脚本。

**`async`属性**

`async`属性的作用是，使用另一个进程下载脚本，下载时不会阻塞渲染。

1. 浏览器开始解析 HTML 网页。
2. 解析过程中，发现带有`async`属性的`script`标签。
3. 浏览器继续往下解析 HTML 网页，同时并行下载`<script>`标签中的外部脚本。
4. 脚本下载完成，浏览器暂停解析 HTML 网页，开始执行下载的脚本。
5. 脚本执行完毕，浏览器恢复解析 HTML 网页。

### js单线程

js是单线程，单线程模型，指的是js只在一个线程上运行，也就是说js执行同时执行一个任务，其他任务必须排队等待。

但是js引擎并不是只有一个线程，事实上js有多个线程，单个脚本只能在一个线程上运行（主线程），其他线程都是在后台配合

### 事件循环

js在执行时，遇到同步任务相继执行，遇到i/o就挂起到等待队列，继续执行后面的任务，等到任务执行完成，就开始执行等待队列中的任务。

为了利用多核cpu的计算能力，HTML5提出web worker标准，允许js创建多个线程，但是子线程完全受控于主线程控制，且不得操作DOM。

### 同步任务和异步任务

同步任务：指那些没有被引擎挂起，在主线程上排队执行的任务，只有前一个任务完成，才执行后一个。

异步任务：指那些会被引擎放在一边，不进入主线程的任务，只有引擎认为他可以执行了，才会让他进入主线程执行，排在异步任务后面的任务不用等待异步任务结束会马上执行，也就说，异步任务不存在“阻塞效应”。

异步任务分为微任务和宏任务

微任务：

- new Promise().then(回调)
- await 
- MutationObserver(html5 新特性)

宏任务：

- script(整体代码)
- setTimeout()
- setInterval()
- postMessage
- I/O
- UI交互事件

当前执行栈执行完毕后时会立刻处理所有微任务队列中的事件，然后再去宏任务队列中取出一个事件。同一次事件循环中，微任务永远在宏任务之前执行。

执行宏任务，然后执行该宏任务产生的微任务，若微任务在执行过程中产生了新的微任务，则继续执行微任务，微任务执行完毕后，再回到宏任务中进行下一轮循环。

- 执行一个宏任务（栈中没有就从事件队列中获取）
- 执行过程中如果遇到微任务，就将它添加到微任务的任务队列中
- 宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）
- 当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染
- 渲染完毕后，JS线程继续接管，开始下一个宏任务（从事件队列中获取）

### 闭包

闭包是一个绑定了词法环境的函数，可以从内部函数访问到外部函数的作用域，闭包有权访问另外一个函数作用域中的变量的函数。



### 事件委托

事件处理的数量关乎到页面的整体运行效能，每个函数都是一个对象，都会占用内存，内存的对象越多，性能越差。

对“事件处理程序过多”问题的解决办法就是事件委托

事件委托利用的事件冒泡，只指定一个事件处理程序，就可以管理一类的所有事件。使用事件委托，只需要在DOM树中尽量最高层上添加一个事件处理程序。

只获取一个DOM元素，只添加一个事件处理程序。

优点：

document对象可以很快被访问，而且可以在页面生命周期的任何时点上为它添加事件处理程序。无需等到DOMContentLoad或load事件。

在页面中设置事件处理程序所需的时间更少。只添加一个事件处理所需的DOM引用更少，所花的时间也更少。

整个页面占用的内存空间更少，能够提升整体性能。

```
const list = document.getElementById('id');
EventUntil.addEventListener(list,'click',function(Event){
    switch(Event.target){
     
    }
})
```



