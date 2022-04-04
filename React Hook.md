# React 高级

### React Hook前言

没有破坏性改动

完全可选

百分百向后兼容

没有计划从react移出 class

### useState（）

在函数组件中使用useState()  结构数组[arr1,function]

**第一个参数是当前的state，第二个参数是改变state的函数类似setState()**

![6](C:\Users\岑岑\Desktop\笔记\图片\6.PNG)

state也可以是一个**对象** 不过每次修改的时候，**必须带有所有的参数**

![7](C:\Users\岑岑\Desktop\笔记\图片\7.PNG)

**注意：**一般我们不采取这样的操作，建议以下的操作，将state分开管理

![8](C:\Users\岑岑\Desktop\笔记\图片\8.PNG)



### useEff()

**纯函数** 输入确定的值 那么输出的值也就确定

##### 无需清除的Effect

在class中实现

![9获](C:\Users\岑岑\Desktop\笔记\图片\9获.PNG)

在函数组件中添加

​	![10](C:\Users\岑岑\Desktop\笔记\图片\10.PNG)

##### 需要清除的Effect

在class中实现

![11](C:\Users\岑岑\Desktop\笔记\图片\11.PNG)

在函数组件中实现

![12](C:\Users\岑岑\Desktop\笔记\图片\12.PNG)

##### 控制Effect执行

第一个参数是回调函数监听第二个参数中任意一项发生改变，第二个参数是一个数组

useEffect(callback,listener)

### useMemo()-性能优化

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

传入 `useMemo` 的函数会在渲染期间执行。请不要在这个函数内部执行与渲染无关的操作，诸如副作用这类的操作属于 `useEffect` 的适用范畴，而不是 `useMemo`。

**你可以把 `useMemo` 作为性能优化的手段，但不要把它当成语义上的保证。**

![image-20210510164620511](C:\Users\岑岑\AppData\Roaming\Typora\typora-user-images\image-20210510164620511.png)

### Refs转发

react不推荐直接操作DOM操作，但是在一些场景中还是无法避免，因此提供了 **ref 用于访问在 render 方法中创建的 DOM 元素或者是 React 组件实例。**

文章推荐https://segmentfault.com/a/1190000010307650?utm_source=sf-similar-article

**函数组件使用ref**

![image-20210514151309034](C:\Users\岑岑\AppData\Roaming\Typora\typora-user-images\image-20210514151309034.png)

类组件

![image-20210514151359631](C:\Users\岑岑\AppData\Roaming\Typora\typora-user-images\image-20210514151359631.png)



### 高阶组件

文章：https://segmentfault.com/a/1190000010307650?utm_source=sf-similar-article

![image-20210514171315586](C:\Users\岑岑\AppData\Roaming\Typora\typora-user-images\image-20210514171315586.png)

