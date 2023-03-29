# DVA

 dva的作用和[redux](https://so.csdn.net/so/search?q=redux&spm=1001.2101.3001.7020)的作用一样，在umi的项目中都是使用dva来管理状态的,，dva也是基于react-redux和reudx基础上做了进一步封装的，然后语法有点像vuex，因此会react-redux和vuex的小伙伴学起来很快。 

### Model

```typescript
export interface NoticeItem extends NoticeIconData {
  id: string;
  type: string;
  status: string;
}

export interface GlobalModelState {
  collapsed: boolean;
  notices: NoticeItem[];
}

export interface GlobalModelType {
  namespace: 'global';
  state: GlobalModelState;
  effects: {
    clearNotices: Effect;
    changeNoticeReadState: Effect;
  };
  reducers: {
    changeLayoutCollapsed: Reducer<GlobalModelState>;
    saveNotices: Reducer<GlobalModelState>;
    saveClearedNotices: Reducer<GlobalModelState>;
  };
  subscriptions: { setup: Subscription };
}

const GlobalModel: GlobalModelType = {
  namespace: 'global',
  // modle管理的全局状态
  state: {
    collapsed: false,
    notices: [],
  },
  // 用于处理异步操作和业务逻辑，触发时机是dispatch
  effects: {
    // *相当于async，yield相当于await
    *clearNotices({ payload }, { put, select }) {
      // put相当于 dispatch
      yield put({
        type: 'saveClearedNotices',
        payload,
      });
      // getListReq 需要返回promise，options是getListReq的入参
      // const res = yield call(getListReq,options)

      // select的参数是一个函数，函数接收的参数是所有model的状态，该条语句的接受值是函数的返回值
      const count: number = yield select((state: ConnectState) => state.global.notices.length);
      const unreadCount: number = yield select(
        (state: ConnectState) => state.global.notices.filter((item) => !item.read).length,
      );
      yield put({
        type: 'user/changeNotifyCount',
        payload: {
          totalCount: count,
          unreadCount,
        },
      });
    },
    *changeNoticeReadState({ payload }, { put, select }) {
      const notices: NoticeItem[] = yield select((state: ConnectState) =>
        state.global.notices.map((item) => {
          const notice = { ...item };
          if (notice.id === payload) {
            notice.read = true;
          }
          return notice;
        }),
      );

      yield put({
        type: 'saveNotices',
        payload: notices,
      });

      yield put({
        type: 'user/changeNotifyCount',
        payload: {
          totalCount: notices.length,
          unreadCount: notices.filter((item) => !item.read).length,
        },
      });
    },
  },
  // 唯一可以修改state的地方 触发时机是dispatch
  reducers: {
    changeLayoutCollapsed(state = { notices: [], collapsed: true }, { payload }): GlobalModelState {
      // 返回值为更新的状态
      return {
        ...state,
        collapsed: payload,
      };
    },
    saveNotices(state, { payload }): GlobalModelState {
      return {
        collapsed: false,
        ...state,
        notices: payload,
      };
    },
    saveClearedNotices(state = { notices: [], collapsed: true }, { payload }): GlobalModelState {
      return {
        ...state,
        collapsed: false,
        notices: state.notices.filter((item): boolean => item.type !== payload),
      };
    },
  },
  // 该选项中的函数自定义命名 函数的触发时机是初始时 ，主要是用来初始化模块状态或者做一些准备性工作
  // 注意：subscriptions 需要返回unlisten方法 用于取消数据订阅
  subscriptions: {
    setup({ history }): void {
      // Subscribe history(url) change, trigger `load` action if pathname is `/`
      history.listen(({ pathname, search }): void => {
        if (typeof window.ga !== 'undefined') {
          window.ga('send', 'pageview', pathname + search);
        }
      });
    },
  },
};

export default GlobalModel;
```

### 在组件中用hook使用

```typescript
import {useDispatch,getDvaApp,useSelector,useStore} from "umi";
export default ()=>{
    const dispatch = useDispatch()
    const storeState = useSelector({ user, global, loading }: ConnectState) => ({
  collapsed: global.collapsed,
  fetchingMoreNotices: loading.effects['global/fetchMoreNotices'],
  fetchingNotices: loading.effects['global/fetchNotices'],
  notices: global.notices,);
        
        
        return (
        <div>
        {
        storeState.notice
    }
        </div>
        )
        
}
```

### 在组件中用connect使用

```typescript
import {connect} from "umi";
const Page ()=>{
   const {dispatch,notice} = props
        
        
        return (
        <div>
        {
        storeState.notice
          }
        </div>
        )     
}

export default connect(({ user, global, loading }: ConnectState) => ({
  currentUser: user.currentUser,
  collapsed: global.collapsed,
  fetchingMoreNotices: loading.effects['global/fetchMoreNotices'],
  fetchingNotices: loading.effects['global/fetchNotices'],
  notices: global.notices,
}))(Page);
```

