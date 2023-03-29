[TOC]



# TypeScript

###  typesript的安装

npm install -g typescript

npx create-react-app ts-with-react --template typescript

react 安装 

### 重启ts服务

 Command + Shift + P 然后输入 restart TS 

### 基础类型

##### 布尔值

`let isDone:boolean = false;`

##### 数值（es6中的十六进制）

`let age:number = 11;`

`let binaryNumber: number = 0b11111;`

##### 字符串

`let firstNmae: string = 'vike';`

`let message: string = ``hello,${firstNmae},age,is${age}`;`

##### undefined 和 null

`let u: undefined = undefined;`

`let n: null = null;`

`let num: number = undefined;`**它们是所有类型的子类型**

### any类型

会丧失具体的类型

`let notSure: any = 4;`

`notSure = 'not string ';`

`notSure = true;`

`notSure.myName`

`notSure.getName();`

### 联合类型

`let numberOrString: number | string = 123;`

`numberOrString = 'abc';`

 `numberOrString=12`;

### Array

`let arrOfNumber: number[] = [1, 2, 3, 4]`

`arrOfNumber.push(5);`

##### 类数组arguments

`function test() {`

 `console.log(*arguments*)`

 `*// arguments.length*`

 `*// arguments[1]*`

`}`

### 元组Tuple

合并了含有不同类型的数组，指定了长度

 `let user: [string, number] = ['sting', 0]`

### interface接口(使用最多)

对对象的形状(shape)进行描述

对类(class)进行抽象

Duck Typing(鸭子类型)  

`interface IPerson {`

`readonly id:number;`readonly只读属性 不能修改 用在属性上 和const(用在变量上)的区别

 `name: string;`(分号结尾)

 `age?: number;`可选属性?

`}`

`let viki: IPerson = {`

 `name: '111',`

 `age: 20`

//不能多一个或者少一个，完全匹配

`}`

### 函数

两部分构成 输入(参数) 输出

可选参数只能放在后面 z添加默认值

##### 函数声明

`function add(x: number, y: number, z: number = 10): number {`

 `if (typeof z === 'number') {`

  `return x + y + z`

 `} else {`

  `return x + y`

 `}`

`}`

`let result = add(9, 8, 10)`

##### 函数表达式



`const add = function add(x: number, y: number, z: number = 10): number {`

 `if (typeof z === 'number') {`

  `return x + y + z`

 `} else {`

  `return x + y`

 `}`

`}`

add是一个函数类型

`const add2: (x: number, y: number, z?: number) => number = add`

##### 类型推断 

`let str = 'str'`

`*// str = 123*`

### 类class

类(class)：定义了一切事物的抽象特点

对象(Object)：类的实例

面向对象(OOP)三大特性：封装 继承 多态

##### 创建一个类

`class Animal {`

name:`string`

 `constructor(name: string) {`

  `*this*.name = name`

 `}`

 `run() {`

  `return ${*this*.name} is running`

 `}`

`}`

##### 实例化对象

`const snake = new Animal('nick')`

`console.log(snake.run())`

##### 继承

`class Dog extends Animal {`

 `bark() {`

  `return ${*this*.name} is barking`

 `}`

`}`

`const xiaobao = new Dog('xiaobao');`

`xiaobao.bark();`

拥有了父类的方法

`xiaobao.run();`

##### 方法重写

`class Cat extends Animal {`

必须使用super调用父类的方法

 `constructor(name) {`

  `*super*(name)`

  `console.log(*this*.name)`

 `}`

 `run() {`

  `return meow , + *super*.run()`

 `}`

`}`

`const maomao = new Cat('maomao')`

多态的特性

`console.log(maomao.run)`



##### 修饰符

对类的方法属性添加权限管理

public 共有 所有的都可以访问并且可以重新赋值

*provite* 私有的 不允许实例对象访问 只能在类中访问 子类也不能访问

*protected* 私有的 子类可以访问到 (类似遗产)

*readonly*  只读不能修改

static  静态属性和静态方法 通过类直接访问 不用实例化 类和实例没有太大的关系

### 接口和类

接口对类的一部分行为进行抽象

implements 封装接口

`interface Radio {`

 `switchRadio(): void`

`}`

`interface ReadioWithBattery extends Radio {`

 `checkBatteryStatus()`

`}`

`class Car implements Radio {`

 `switchRadio() {`

 `}`

`}`

`class CellPhone implements ReadioWithBattery {`

 `switchRadio() {`

 `}`

 `checkBatteryStatus() {`

 `}`

`}`



### 枚举enums

一定范围内的一系列常量

##### 数字枚举

如果未赋值，则会依次从0递增,如果赋值number则会根据number依次增加+1

`enum Direction {`

 `Up,`

 `down,`

 `Left,`

 `Right`

`}`

`console.log(Direction.Right)`

`console.log(Direction[0])`



### 泛型(Generics)

##### 初识泛型

 当需要 定义 一个 参数 类型 与 返回 值 类型 不确定的 函数 时 

指在函数指定类或者接口的时候，不事先指定类型而是在使用的时候再指定类型

< >

`function echo<T>(arg: T): T {`

 `return arg`

`}`

`const result = echo(1132)`

传递元组并且将返回的值交换位置

`function swap<t, u>(tuple: [t, u]): [u, t] {`

 `return [tuple[1], tuple[0]]`

`}`

`const reslut2 = swap(['string', 123])`

`reslut2[1]`

##### 约束泛型



`function echoWithArr<T>(arg:T[]):T[]{`

​    `console.log(arg.length)`

​    `return arg`

`}`

`const arrs = echoWithArr(['2',false,3])`

改进 使用extends关键字 对泛型进行约束 限制传入的参数

![1](C:\Users\岑岑\Desktop\笔记\图片\1.PNG)

##### 类class

![2](C:\Users\岑岑\Desktop\笔记\图片\2.PNG)



##### 接口interface

![3](C:\Users\岑岑\Desktop\笔记\图片\3.PNG)



### 类型别名

![4](C:\Users\岑岑\Desktop\笔记\图片\4.PNG)

### 类型断言

![5](C:\Users\岑岑\Desktop\笔记\图片\5.PNG)



### 声明文件

### 配置编译



| {    |                                                              |
| ---- | ------------------------------------------------------------ |
|      | // 此json文件中可以写注释！                                  |
|      | /*                                                           |
|      | tsconfig.json是ts编译器的配置文件，ts编译器可以根据它的信息来对代码进行编译 |
|      | */                                                           |
|      |                                                              |
|      | /*                                                           |
|      | "include" 用来指定哪些ts文件需要被编译                       |
|      | 默认：当前路径下所有文件, **\*                               |
|      | 路径：** 表示任意目录                                        |
|      | * 表示任意文件                                               |
|      | */                                                           |
|      | "include": [                                                 |
|      | "./src/**/*"                                                 |
|      | ],                                                           |
|      | /*                                                           |
|      | "exclude" 不需要被编译的文件目录                             |
|      | 默认值：                                                     |
|      | ["node_modules", "bower_components", "jspm_packages", "./dist"] |
|      | */                                                           |
|      | "exclude": [                                                 |
|      | "./src/exclude/**/*"                                         |
|      | ],                                                           |
|      | /*                                                           |
|      | 被继承的配置文件                                             |
|      | 例如："extends": "。/configs/base",                          |
|      | */                                                           |
|      | //  "extends": "",                                           |
|      | /*                                                           |
|      | 指定被编译文件的列表，只有需要编译的文件少时才会用到         |
|      | */                                                           |
|      | //  "files": [],                                             |
|      | /*                                                           |
|      | compilerOptions 编译器的选项                                 |
|      | */                                                           |
|      | "compilerOptions": {                                         |
|      | // target 用来指定ts被编译为的ES的版本                       |
|      | // 'es3', 'es5', 'es6', 'es2015', 'es2016', 'es2017', 'es2018', ... |
|      | "target": "es2015",                                          |
|      | // module 指定要使用的模块化的规范                           |
|      | // 'none', 'commonjs', 'amd', 'system', 'umd', 'es6', 'es2015', ... |
|      | "module": "es2015",                                          |
|      | // lib用来指定项目中要使用的库                               |
|      | // 在node项目中可以声明用到的库，在前端中可以声明dom(浏览器中的内置库，但是在node中是不需要的！) |
|      | // 默认就是在浏览器中的运行环境！                            |
|      | //'es5', 'es6', 'es2015', 'es7', 'es2016', 'es2017', 'es2018', 'es2019', 'es2020', |
|      | // 'esnext', 'dom', 'dom.iterable', ...                      |
|      | "lib": [                                                     |
|      | "es6",                                                       |
|      | "dom"                                                        |
|      | ],                                                           |
|      | // outDir 用来指定编译后文件所在的目录                       |
|      | "outDir": "./dist",                                          |
|      | // 将代码合并为一个文件                                      |
|      | // 设置outFile后，所有的全局作用域中的代码会合并到同一个文件中 |
|      | //"outFile": "./dist/app.js"                                 |
|      | // 是否对js文件进行编译，默认是false                         |
|      | "allowJs": true,                                             |
|      | // 是否检查js代码是否符合语法规范，默认是false               |
|      | "checkJs": true,                                             |
|      | // 是否移除注释                                              |
|      | "removeComments": true,                                      |
|      | // 不生成编译后的文件                                        |
|      | // 仅用TS检查语法                                            |
|      | "noEmit": false,                                             |
|      | // 当有错误时不生成编译后的文件                              |
|      | "noEmitOnError": true,                                       |
|      | /*                                                           |
|      | 语法检查属性                                                 |
|      | */                                                           |
|      | // 所有严格检查的总开关                                      |
|      | "strict": true,                                              |
|      | // 用来设置编译后的文件是否使用严格模式，默认false           |
|      | // 在ES6中的模块化会自动使用严格模式，而无需在文件开头添加`'use strict'` |
|      | "alwaysStrict": true,                                        |
|      | // 不允许隐式的any类型                                       |
|      | "noImplicitAny": true,                                       |
|      | // 不允许不明确类型的this                                    |
|      | "noImplicitThis": true,                                      |
|      | // 严格的检查空值                                            |
|      | "strictNullChecks": true                                     |
|      | }                                                            |
|      | }                                                            |



### webpack整合

通常情况下，实际开发中我们都需要使用构建工具对代码进行打包；

TS同样也可以结合构建工具一起使用，下边以webpack为例介绍一下如何结合构建工具使用TS；

步骤如下：

#### 初始化项目

进入项目根目录，执行命令 `npm init -y`，创建package.json文件

#### 下载构建工具

命令如下：

```
npm i -D webpack webpack-cli webpack-dev-server typescript ts-loader clean-webpack-plugin
```

共安装了7个包:

- webpack：构建工具webpack
- webpack-cli：webpack的命令行工具
- webpack-dev-server：webpack的开发服务器
- typescript：ts编译器
- ts-loader：ts加载器，用于在webpack中编译ts文件
- html-webpack-plugin：webpack中html插件，用来自动创建html文件
- clean-webpack-plugin：webpack中的清除插件，每次构建都会先清除目录

#### 配置webpack

根目录下创建webpack的配置文件`webpack.config.js`：

```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = {
   optimization:{
       minimize: false // 关闭代码压缩，可选
   },

   entry: "./src/index.ts",

   devtool: "inline-source-map",

   devServer: {
       contentBase: './dist'
   },

   output: {
       path: path.resolve(__dirname, "dist"),
       filename: "bundle.js",
       environment: {
           arrowFunction: false // 关闭webpack的箭头函数，可选
       }
   },

   resolve: {
       extensions: [".ts", ".js"]
   },

   module: {
       rules: [
           {
               test: /\.ts$/,
               use: {
                   loader: "ts-loader"     
               },
               exclude: /node_modules/
           }
       ]
   },

   plugins: [
       new CleanWebpackPlugin(),
       new HtmlWebpackPlugin({
           title:'TS测试'
       }),
   ]
}
```

#### 配置TS编译选项

根目录下创建tsconfig.json，配置可以根据自己需要

```
{
   "compilerOptions": {
       "target": "ES2015",
       "module": "ES2015",
       "strict": true
   }
}
```

#### 修改package.json配置

修改package.json添加如下配置

```
{
   ...
   "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "build": "webpack",
       "start": "webpack serve --open chrome.exe"
   },
   ...
}
```

#### 项目使用

在src下创建ts文件，并在并命令行执行`npm run build`对代码进行编译；

或者执行`npm start`来启动开发服务器；



### Babel

除了webpack，开发中还经常需要结合babel来对代码进行转换；

以使其可以兼容到更多的浏览器，在上述步骤的基础上，通过以下步骤再将babel引入到项目中；

> 虽然TS在编译时也支持代码转换，但是只支持简单的代码转换；
>
> 对于例如：Promise等ES6特性，TS无法直接转换，这时还要用到babel来做转换；

安装依赖包：

```
npm i -D @babel/core @babel/preset-env babel-loader core-js
```

共安装了4个包，分别是：

- @babel/core：babel的核心工具
- @babel/preset-env：babel的预定义环境
- @babel-loader：babel在webpack中的加载器
- core-js：core-js用来使老版本的浏览器支持新版ES语法

修改webpack.config.js配置文件

```
...
module: {
    rules: [
        {
            test: /\.ts$/,
            use: [
                {
                    loader: "babel-loader",
                    options:{
                        presets: [
                            [
                                "@babel/preset-env",
                                {
                                    "targets":{
                                        "chrome": "58",
                                        "ie": "11"
                                    },
                                    "corejs":"3",
                                    "useBuiltIns": "usage"
                                }
                            ]
                        ]
                    }
                },
                {
                    loader: "ts-loader",

                }
            ],
            exclude: /node_modules/
        }
    ]
}
...
```

如此一来，使用ts编译后的文件将会再次被babel处理；

使得代码可以在大部分浏览器中直接使用；

同时可以在配置选项的targets中指定要兼容的浏览器版本；



## 面向对象

要想面向对象，操作对象，首先便要拥有对象；

要创建对象，必须要先定义类，所谓的类可以理解为对象的模型；

程序中可以根据类创建指定类型的对象；

举例来说：

可以通过Person类来创建人的对象，通过Dog类创建狗的对象，不同的类可以用来创建不同的对象；

### 定义类

```
class 类名 {
    属性名: 类型;
    
    constructor(参数: 类型){
        this.属性名 = 参数;
    }
    
    方法名(){
        ....
    }

}
```

示例：

```
    class Person{
        name: string;
        age: number;
    
        constructor(name: string, age: number){
            this.name = name;
            this.age = age;
        }
    
        sayHello(){
            console.log(`大家好，我是${this.name}`);
        }
    }
```

使用类：

```
const p = new Person('孙悟空', 18);
p.sayHello();
```

### 构造函数

可以使用`constructor`定义一个构造器方法；

> **注1：在TS中只能有一个构造器方法！**

例如：

```
class C{
    name: string;
    age: number

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}
```

同时也可以直接将属性定义在构造函数中：

```
class C {
    constructor(public name: string, public age: number) {
    }
}
```

上面两种定义方法是完全相同的！

**注2：子类继承父类时，必须调用父类的构造方法（如果子类中也定义了构造方法）！**

例如：

```
class A {
    protected num: number;
    constructor(num: number) {
        this.num = num;
    }
}

class X extends A {
    protected name: string;
    constructor(num: number, name: string) {
        super(num);
        this.name = name;
    }
}
```

如果在X类中不调用`super`将会报错！

### 封装

对象实质上就是属性和方法的容器，它的主要作用就是存储属性和方法，这就是所谓的封装

默认情况下，对象的属性是可以任意的修改的，为了确保数据的安全性，在TS中可以对属性的权限进行设置

- 静态属性（static）：
  - 声明为static的属性或方法不再属于实例，而是属于类的属性；
- 只读属性（readonly）：
  - 如果在声明属性时添加一个readonly，则属性便成了只读属性无法修改
- TS中属性具有三种修饰符：
  - public（默认值），可以在类、子类和对象中修改
  - protected ，可以在类、子类中修改
  - private ，可以在类中修改

示例：

public：

```
class Person{
    public name: string; // 写或什么都不写都是public
    public age: number;

    constructor(name: string, age: number){
        this.name = name; // 可以在类中修改
        this.age = age;
    }

    sayHello(){
        console.log(`大家好，我是${this.name}`);
    }
}

class Employee extends Person{
    constructor(name: string, age: number){
        super(name, age);
        this.name = name; //子类中可以修改
    }
}

const p = new Person('孙悟空', 18);
p.name = '猪八戒';// 可以通过对象修改
```

protected：

```
class Person{
    protected name: string;
    protected age: number;

    constructor(name: string, age: number){
        this.name = name; // 可以修改
        this.age = age;
    }

    sayHello(){
        console.log(`大家好，我是${this.name}`);
    }
}

class Employee extends Person{

    constructor(name: string, age: number){
        super(name, age);
        this.name = name; //子类中可以修改
    }
}

const p = new Person('孙悟空', 18);
p.name = '猪八戒';// 不能修改
```

private：

```
class Person{
    private name: string;
    private age: number;

    constructor(name: string, age: number){
        this.name = name; // 可以修改
        this.age = age;
    }

    sayHello(){
        console.log(`大家好，我是${this.name}`);
    }
}

class Employee extends Person{

    constructor(name: string, age: number){
        super(name, age);
        this.name = name; //子类中不能修改
    }
}

const p = new Person('孙悟空', 18);
p.name = '猪八戒';// 不能修改
```

### 属性存取器

对于一些不希望被任意修改的属性，可以将其设置为private

直接将其设置为private将导致无法再通过对象修改其中的属性

我们可以在类中定义一组读取、设置属性的方法，这种对属性读取或设置的属性被称为属性的存取器

读取属性的方法叫做setter方法，设置属性的方法叫做getter方法

示例：

```
class Person{
    private _name: string;

    constructor(name: string){
        this._name = name;
    }

    get name(){
        return this._name;
    }

    set name(name: string){
        this._name = name;
    }

}

const p1 = new Person('孙悟空');
// 实际通过调用getter方法读取name属性
console.log(p1.name);
// 实际通过调用setter方法修改name属性 
p1.name = '猪八戒'; 
```

### 静态属性

静态属性（方法），也称为类属性。使用静态属性无需创建实例，通过类即可直接使用

静态属性（方法）使用static开头

示例：

```
class Tools{
    static PI = 3.1415926;
    
    static sum(num1: number, num2: number){
        return num1 + num2
    }
}

console.log(Tools.PI);
console.log(Tools.sum(123, 456));
```

### this

在类中，使用this表示当前对象

### 继承

继承时面向对象中的又一个特性

通过继承可以将其他类中的属性和方法引入到当前类中

示例：

```
class Animal{
    name: string;
    age: number;

    constructor(name: string, age: number){
        this.name = name;
        this.age = age;
    }
}

class Dog extends Animal{

    bark(){
        console.log(`${this.name}在汪汪叫！`);
    }
}

const dog = new Dog('旺财', 4);
dog.bark();
```

通过继承可以在不修改类的情况下完成对类的扩展

### 重写

发生继承时，如果子类中的方法会替换掉父类中的同名方法，这就称为方法的重写

示例：

```
class Animal{
    name: string;
    age: number;

    constructor(name: string, age: number){
        this.name = name;
        this.age = age;
    }

    run(){
        console.log(`父类中的run方法！`);
    }
}

class Dog extends Animal{

    bark(){
        console.log(`${this.name}在汪汪叫！`);
    }

    run(){
        console.log(`子类中的run方法，会重写父类中的run方法！`);
    }
}

const dog = new Dog('旺财', 4);
dog.bark();
```

**在子类中可以使用super来完成对父类的引用**

### 抽象类（abstract class）

抽象类是专门用来被其他类所继承的类，它只能被其他类所继承不能用来创建实例

```
abstract class Animal{
  abstract run(): void;
  bark(){
      console.log('动物在叫~');
  }
}

class Dog extends Animals{
  run(){
      console.log('狗在跑~');
  }
}
```

使用abstract开头的方法叫做抽象方法，抽象方法没有方法体只能定义在抽象类中，继承抽象类时抽象方法必须要实现;

# 问题收集

```css
yarn add -D @types/XXX
```





# 内置类型

###  Partial<Type> （设置可选）

 构造一个类型，其**所有 Type 属性都设置为可选**。此实用程序将返回表示给定类型的所有子集的类型。 

```typescript
interface Todo {
  title: string;
  description: string;
}

//Partial将接受的类型Todo中的所有子类型的属性都设置为可选 然后返回下面
// fieldsToUpdate:{
//  title?: string;
//  description?: string;
//}  

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}
 
const todo1 = {
  title: "organize desk",
  description: "clear clutter",
};
 
const todo2 = updateTodo(todo1, {
  description: "throw out trash",
});
```



###  Required<Type> （设置必选）

 构造一个包含 Type 设置为 **required 的所有属性的类型**。**与 Partial 相反**。 

```typescript
interface Props {
  a?: number;
  b?: string;
}
 
const obj: Props = { a: 5 };
 
 //Required 将Props中的所有属性都设置为必选
const obj2: Required<Props> = { a: 5 };
Property 'b' is missing in type '{ a: number; }' but required in type 'Required<Props>'.
```

 

### Readonly<Type> （设置只读）

构造具有 Type 的**所有属性设置为只读的类型**，这意味着**不能重新分配构造类型的属性**。 

```typescript
interface Todo {
  title: string;
}
 
const todo: Readonly<Todo> = {
  title: "Delete inactive users",
};
 
todo.title = "Hello";
Cannot assign to 'title' because it is a read-only property.
```

 这个实用程序对于表示在运行时失败的赋值表达式(即试图重新分配冻结对象的属性时)很有用。 

##### `Object.freeze`

```typescript
 function freeze<Type>(obj: Type): Readonly<Type>; 
```

###  

### Record<Keys, Type> 

 构造**属性键为 Keys**、**属性值为 Type** 的对象类型。此实用工具可用于将一个类型的属性映射到另一个类型。 

```typescript
interface CatInfo {
  age: number;
  breed: string;
}
 
type CatName = "miffy" | "boris" | "mordred";
 //cats对象的健是CatName 值是CatInfo
const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
 
cats.boris;
 
const cats: Record<CatName, CatInfo>
```

###  

### Pick<Type, Keys> (挑选)

 通过从 Type 中选择一组属性 Keys (字符串文字或字符串文字的并集)来构造类型 

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
 
type TodoPreview = Pick<Todo, "title" | "completed">;
 
const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
 
todo;
 
const todo: TodoPreview
```



### Omit<Type,Keys>

 通过从 Type 中选取所有属性，然后移除 Keys (字符串文字或字符串文字的并集)来构造类型。 

```typescript
interface Tod{
title:string;
description:string;
completed:boolean;
createAt:number;
}

type TodoPreview = Omit<Todo,"description">;

const todo:TodoPreview = {
  title:"Study",
  completed:true,
  description:"math"  //error description 类型被Omit 所以todo不能设置description
}
type TodoPreview = Omit<Todo,"description" | "createdAt">;

```



###  Exclude<UnionType, ExcludedMembers> （排除）

 通过从 UnionType 中排除可分配给 ExcludedMembers 的所有联合成员来构造类型。 

```typescript
//接受第一个参数为联合类型 第二个参数为需要排除的类型 返回排除后的联合类型
type T0 = Exclude<"a" | "b" | "c", "a">;   
type T0 = "b" | "c"

type T1 = Exclude<"a" | "b" | "c", "a" | "b">;  
type T1 = "c"

type T2 = Exclude<string | number | (() => void), Function>;  
type T2 = string | number
```



###  Extract<Type, Union> （提取）

通过从 Type 中提取可分配给 Union 的所有联合成员来构造类型。

```typescript
//第一个参数是联合类型 若是对象类型的话 返回值是never类型
type T0 = Extract<"a" | "b" | "c", "a" | "f">;
type T0 = "a"

type T1 = Extract<string | number | (() => void), Function>;
type T1 = () => void

type sud = {
    name:string;
    age:number;
    phone:number;
    description:string
}
type T2 = Extract<sud,"name"> // never
let c :Extract<sud,'name'>= {
    name:"cai" //Type 'string' is not assignable to type 'never'.
}
```



###  NonNullable<Type> （排除null undef）

 通过从 Type 中排除 null 和 undefined 来构造类型。 

```typescript
type T0 = NonNullable<string | number | undefined>;   
type T0 = string | number

type T1 = NonNullable<string[] | null | undefined>; 
type T1 = string[]
```



###  Parameters<Type> （参数类型）

 从函数类型类型的参数中使用的类型构造元组类型。 

```typescript
declare function f1(arg: { a: number; b: string }): void;
declare function f2(name:string,age:number):void;
type T0 = Parameters<() => string>;
//    ^?
type T1 = Parameters<(s: string) => void>;
//    ^?
type T2 = Parameters<<T>(arg: T) => T>;
//    ^?
type T3 = Parameters<typeof f1>;
//    ^?
type T4 = Parameters<any>;
//    ^?
type T5 = Parameters<never>;
//    ^?
type T6 = Parameters<string>;
//    ^?
type T7 = Parameters<Function>;
//    ^?
//接受一个函数参数 获取这个函数的参数 返回包含该参数的元组类型
interface student {
   name:string;
   age:number;
   getName:(name:string, age:number)=> void
}
type Detail = Parameters<student["getName"]>
function getDetail(param:Detail){
    return param
}
getDetail(['cai',23])
```



###  ConstructorParameters<Type> （构造函数参数类型）

 从构造函数类型的类型构造元组或数组类型。它生成具有所有参数类型的元组类型(如果 Type 不是函数，则生成 never 类型)。 

```typescript
type T0 = ConstructorParameters<ErrorConstructor>;
type T0 = [message?: string]


type T1 = ConstructorParameters<FunctionConstructor>;
type T1 = string[]


type T2 = ConstructorParameters<RegExpConstructor>;  
type T2 = [pattern: string | RegExp, flags?: string]


type T3 = ConstructorParameters<any>;
type T3 = unknown[]
 
//必须接受一个构造函数参数
type T4 = ConstructorParameters<Function>;
Type 'Function' does not satisfy the constraint 'abstract new (...args: any) => any'.
  Type 'Function' provides no match for the signature 'new (...args: any): any'. 
type T4 = never
```



###  ReturnType<Type> (函数返回类型)

 构造一个类型，由**函数类型的返回类型**组成。 

```typescript
declare function f1(): { a: number; b: string };
 
type T0 = ReturnType<() => string>; 
type T0 = string

type T1 = ReturnType<(s: string) => void>;
type T1 = void

type T2 = ReturnType<<T>() => T>; 
type T2 = unknown

type T3 = ReturnType<<T extends U, U extends number[]>() => T>;
type T3 = number[]
//这里的f1 是一个具体的值 ReturnType接受的是一个type 所以使用typeof做类型推断
type T4 = ReturnType<typeof f1>;
type T4 = {
    a: number;
    b: string;
}

type T5 = ReturnType<any>;  
type T5 = any

type T6 = ReturnType<never>;
type T6 = never

type T7 = ReturnType<string>;
Type 'string' does not satisfy the constraint '(...args: any) => any'.
type T7 = any

type T8 = ReturnType<Function>;
Type 'Function' does not satisfy the constraint '(...args: any) => any'.
  Type 'Function' provides no match for the signature '(...args: any): any'.
type T8 = any
```



###  InstanceType<Type> （实例类型）

 构造由 Type 中的**构造函数的实例类型组成的类型**。 

```typescript
class C {
  x = 0;
  y = 0;
}
 
type T0 = InstanceType<typeof C>;
type T0 = C
                       
type T1 = InstanceType<any>;
type T1 = any
                       
                       
type T2 = InstanceType<never>;   
type T2 = never
                       
type T3 = InstanceType<string>;
Type 'string' does not satisfy the constraint 'abstract new (...args: any) => any'.    
type T3 = any
                       
type T4 = InstanceType<Function>;
Type 'Function' does not satisfy the constraint 'abstract new (...args: any) => any'.
  Type 'Function' provides no match for the signature 'new (...args: any): any'.  
type T4 = any
```



###  ThisParameterType<Type> 

 Extracts the type of the [this](https://www.typescriptlang.org/docs/handbook/functions.html#this-parameters) parameter for a function type, or [unknown](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-0.html#new-unknown-top-type) if the function type has no `this` parameter. 



```typescript
function toHex(this: Number) {
  return this.toString(16);
}
 
function numberToString(n: ThisParameterType<typeof toHex>) {
  return toHex.apply(n);
}
```



###  OmitThisParameter<Type> 

 从 Type 中移除**this**参数。如果 Type 没有显式声明此参数，则结果只是 Type。否则，将从 Type 创建没有此参数的新函数类型。泛型被擦除，只有最后一个重载签名被传播到新的函数类型中。 

```typescript
function toHex(this: Number) {
  return this.toString(16);
}
 
const fiveToHex: OmitThisParameter<typeof toHex> = toHex.bind(5);
 
console.log(fiveToHex());
```



###  ThisType<Type> 

 此实用程序不返回转换后的类型。相反，它可以作为这种类型上下文的标记。请注意，必须启用 noImplicitThis 标志才能使用此实用程序。 



# 函数





# 对象



