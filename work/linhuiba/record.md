# 树形组件的同层级拖拽
  使用antd的Tree组件 
```ts
 // 拖拽元素 只支持同层级拖拽
  const onDrop: TreeProps['onDrop'] = (info) => {
    // node         代表当前被drop 的对象
    // dragNode     代表当前需要drop 的对象
    // dropPosition 代表drop后的节点位置；不准确
    // dropToGap    代表移动到非最顶级组第一个位置
    const dropKey = info.node.key;
    const dragKey = info.dragNode.key;
    const dropPos = info.node.pos.split('-');
    // dropPosition: ( -1 | 0 | 1 ) dropPosition计算前的值
    // -1 代表移动到最顶级组的第一个位置 inside 0, top -1, bottom 1
    const dropPosition = info.dropPosition - Number(dropPos[dropPos.length - 1]);
    // 判断是否处于同层拖拽
    const canDrop = (isDropToFirst(info.dragNode, info.node) ||
      (isSameParent(info.dragNode, info.node) && isSameLevel(info.dragNode, info.node)) && info.dropToGap);
    if (!canDrop) return;
    const loop = (
      data: TreeDataNode[],
      key: React.Key,
      callback: (node: TreeDataNode, i: number, data: TreeDataNode[]) => void,
    ) => {
      for (let i = 0; i < data.length; i++) {
        if (data[i].key === key) {
          return callback(data[i], i, data);
        }
        if (data[i].children) {
          loop(data[i].children!, key, callback);
        }
      }
    };
    const data = [...gData];
    // 递归找到拖拽的node
    let dragObj: TreeDataNode;
    loop(data, dragKey, (item, index, arr) => {
      arr.splice(index, 1);
      dragObj = item;
    });

    if (!info.dropToGap) {
      loop(data, dropKey, (item) => {
        item.children = item.children || [];
        // 插入位置
        item.children.unshift(dragObj);
      });
    } else {
      let ar: TreeDataNode[] = [];
      let i: number;
      loop(data, dropKey, (_item, index, arr) => {
        ar = arr;
        i = index;
      });
      if (dropPosition === -1) {
        // Drop on the top of the drop node
        ar.splice(i!, 0, dragObj!);
      } else {
        // Drop on the bottom of the drop node
        ar.splice(i! + 1, 0, dragObj!);
      }
    }
    setGData(data);
  };
```

抽象为具体算法：
描述：给定一个树状结构的数据，需要被移动元素的key和需要移动元素的位置索引
根据key递归找到指定元素
递归
```js
 fcuntion loop<T>(
      data: T[],
      key: React.Key,
      callback: (node: T, i: number, data: T[]) => void,
    ) => {
      for (let i = 0; i < data.length; i++) {
        if (data[i].key === key) {
          return callback(data[i], i, data);
        }
        if (data[i].children) {
          loop(data[i].children!, key, callback);
        }
      }
    };

    let dragObj: TreeDataNode;
    loop(data, dragKey, (item, index, arr) => {
        // do something
        // 移动
    });
```
移动 
1、通过splice(index,1)先将指定key元素从原数组中剔除，再通过splice(i,0,obj)插入到 指定位置 两次for 时间复杂度On
2、一次for循环
```js
  const _splice = (arr, index, item) => {
    let temp;
    for (let i = 0; i < arr.length; i++) {
      console.log(index, i);
      if (index === i) {
        temp = arr[i];
        arr[i] = item;
      }
      if (index < i) {
        const current = arr[i];
        arr[i] = temp;
        temp = current;
      }
    }
    return arr;
  };
  console.log('_splioce', _splice([1, 23, 4, 5], 1,1));

```
