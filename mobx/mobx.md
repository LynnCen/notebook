# mobx

状态管理库

observable state 

actions 修改state的方法

computeds  计算值可以用来从其他可观察对象state中派生信息，会缓存其值，如果observable中state改变，那么就会触发计算

reactions 对自动发生的副作用进行建模。 它们的意义在于为你的可观察状态创建消费者，以及每当*关联*的值发生变化时，*自动*运行副作用。

例如autorun：autorun中的可观察数据如果发生改变，则自动运行一次，注意：**第一次定义autorun也会运行一次**，接收一个无参函数

when：接收两个函数作为参数，当第一个函数返回true，就会执行一次第二个函数，**且全局只执行一次**，不再被触发

reaction：接收两个函数作为参数，第一个函数中的可观察值变化时执行第二个函数，相当于when的循环版。



使用class类的方法封装某个模型的store。

挂载到最外层的provider中。

类组件通过Inject 将store注入组件 可以使用this.props使用。

函数组件 inject("xxxStore")(observer(react组件))

**注意：**

**mobx 6版本以前不支持hook**，需要更新，更新后6版本移除了装饰器

方法一：使用`makeObservable`，但是需要标注一下哪些属性进行转换

```JavaScript
makeObservable(this, {
            value: observable,
            double: computed,
            increment: action,
            fetch: flow
        })
```

方法二：使用`makeAutoObservable`，自动转换所有属性，无需装饰器标注，注意：函数默认为@action，字段默认为@observable

```JavaScript
constructor() {
        makeAutoObservable(this)
    }
```

