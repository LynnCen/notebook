# Redux

UI层触发dispatch事件action，到store中，执行reducer函数，返回一个新的state，ui更新。reducer函数中执行的是同步相关的代码，不能异步。

creatSlice() 创建一个切片reducer，包含name，initialState，reducers处理函数，接受两个参数，一个是state，另外一个是action对象，包含type和playload。

在reducers函数中不能直接修改state的，遵循immer原则，不可变性，而是重新返回一个新的对象，或者修改state中的某个属性。

在configureStore()方法中配置我们刚刚创建的切片reducer，导出store，挂载到provider上。

组件中通过useSelector()使用state，使用dispatch更新。

性能优化：

useSelector，createSelector类似react.memo()可以防止不必要的更新。

异步更新：

thunk和middleware

“thunk” 这个词是一个编程术语，意思是 ["一段做延迟工作的代码"](https://en.wikipedia.org/wiki/Thunk).

- Redux Toolkit 有一个 `createAsyncThunk` API 可以为你 dispatch 这些 action
  - `createAsyncThunk` 接受一个 “payload creator” 回调函数，它应该返回一个 `Promise`，并自动生成 `pending/fulfilled/rejected` action 类型
  - 像 `fetchPosts` 这样生成的 action creator 根据你返回的 `Promise` dispatch 这些 action
  - 可以使用 `extraReducers` 字段在 `createSlice` 中监听这些 action，并根据这些 action 更新 reducer 中的状态。
  - action creator 可用于自动填充 `extraReducers` 对象的键，以便切片知道要监听的 action。
  - Thunk 可以返回 promise。 具体对于`createAsyncThunk`，你可以`await dispatch(someThunk()).unwrap()`来处理组件级别的请求成功或失败。