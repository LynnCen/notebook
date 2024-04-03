# Why React Re-Rende

pre-answer：

（1）state的改变

（2）props的改变

（3）context中的value改变


（1）**a state variable changes**

state的改变只会影响当前组件和它的子组件 不会影响父组件

（2）**A component will re-render because its props change**

并不是因为props的改变 假如子组件时一个纯组件 当父组件re-render的时候 子组件就会render react很难追踪到哪些子组件使用到了state值 所以当前组件re-render的时候会将所有的子组件都render一次 

（3）What about context?

当 Context 的 value 发生改变的时候，所有的子组件都会更新。那么为什么即使不依赖 Context 的子组件也会更新呢？Context 本身并不是一个状态管理工具，只是一种状态传递工具。Context 的 value 发生改变的根本原因还是状态的改变


