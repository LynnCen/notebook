**1.主要考察基本代码能力(P5), 及对于优化手段(请求并发, 缓存) 等手段的使用(P6)**

```javascript
// 假设本地机器无法做加减乘除法，需要通过远程请求让服务端来实现。 (即不允许使用 + 号)
// 以加法为例，现有远程API的模拟实现 ——
// const addRemote = async (a, b) => new Promise((resolve) => {
//   setTimeout(() => resolve(a + b), 1000)
// })。
// 请实现本地的add方法，调用addRemote，能最优的实现输入数字的加法func 。
// 请用示例验证运行结果 —— add(1, 2) = 3; add(1, 3, 5, 2) = 11

//addRemote 方法 不能做任何修改
const addRemote = async (a, b) => new Promise((resolve) => {
   setTimeout(() => resolve(a + b), 1000)
})。

// ------------------ 请在以下区域作答--------------------

const add = async (...args) => {
  if (args.length === 1) {
    return args[0];
  }

  const [a, b] = args.splice(0, 2);
  const sum = await addRemote(a, b);

  return add(sum, ...args);
};

// 示例验证运行结果
add(1, 2).then(result => console.log(result)); // 输出 3
add(1, 3, 5, 2).then(result => console.log(result)); // 输出 11
```

2.**主要考察基本代码能力(P5)**

如何实现一个组件实时显示窗口的大小？

```javascript
import React, { useState, useEffect } from "react";

function WindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEffect(() => {
    function handleResize() {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    }

    window.addEventListener("resize", handleResize);

    return () => window.removeEventListener("resize", handleResize);
  }, []);

  return (
    <div>
      <h1>
        Window Size: {windowSize.width} x {windowSize.height}
      </h1>
    </div>
  );
}

export default WindowSize;
```

3.**主要考察基本代码能力(P5)**

如何实现一个图片懒加载组件

```JavaScript

import React, { useRef, useEffect, useState } from 'react';

function LazyImage({ src, alt }) {
  const imageRef = useRef(null);
  const [isLoaded, setIsLoaded] = useState(false);

  useEffect(() => {
    // 观察者
    const observer = new IntersectionObserver((entries) => {
      const image = entries[0];
// 判断是否出现在根节点可视交叉区域
      if (image.isIntersecting) {
        setIsLoaded(true);
      }
    });

    if (imageRef.current) {
      observer.observe(imageRef.current);
    }

    return () => {
      if (imageRef.current) {
        observer.unobserve(imageRef.current);
      }
    };
  }, []);

  return (
    <div>
      {isLoaded ? (
        <img ref={imageRef} src={src} alt={alt} />
      ) : (
        <div style={{ height: '200px' }}>Loading...</div>
      )}
    </div>
  );
}
function LazyImage({src,alt}){
const imgRef = useRef();
const [load,setLoad] = useState(false);

useEffect(()=>{
  const observer = new IntersectionObserver((entries)=>{
    const imgage = entries[0];
    if(imgage.isIntersecting){
      setLoad(true);
    }
  })

  if(imgRef.current){
    observer.observe(imgRef.current)
  }
  return ()=>{
    if(imgRef.current){
      observer.ununobserve(imgRef.current)
    }
  }
},[])
return <>{
  load?<img src={src} alt={alt}/>:<div>loading</div>
}
}

export default LazyImage;

function App() {
  return (
    <div>
      <h1>Lazy Loading Images</h1>
      <LazyImage src="image.jpg" alt="Lazy loaded image" />
    </div>
  );
}

export default App;

```

4.**主要考察组件开发能力(P6)**

```javascript
/** 1. 信号灯控制器
用 React 实现一个信号灯（交通灯）控制器，要求：
1. 默认情况下，
1.1. 红灯亮20秒，并且最后5秒闪烁
1.2. 绿灯亮20秒，并且最后5秒闪烁
1.3. 黄灯亮10秒
1.4. 次序为 红-绿-黄-红-绿-黄
2. 灯的个数、颜色、持续时间、闪烁时间、灯光次序都可配置，如：
lights=[{color: '#fff', duration: 10000, twinkleDuration: 5000}, ... ]
*/

import React, { useState, useEffect } from "react";

function TrafficLightController({ lights }) {
  const [currentLight, setCurrentLight] = useState(0);

  useEffect(() => {
    const timer = setTimeout(() => {
      setCurrentLight((currentLight + 1) % lights.length);
    }, lights[currentLight].duration);

    return () => clearTimeout(timer);
  }, [currentLight, lights]);

  return (
    <div className="traffic-light-controller">
      {lights.map((light, index) => (
        <div
          key={index}
          className={`light ${index === currentLight ? "active" : ""}`}
          style={{ backgroundColor: light.color }}
        />
      ))}
    </div>
  );
}

// 使用示例
const lights = [
  { color: "red", duration: 20000, twinkleDuration: 5000 },
  { color: "green", duration: 20000, twinkleDuration: 5000 },
  { color: "yellow", duration: 10000 },
];

function App() {
  return <TrafficLightController lights={lights} />;
}

export default App;
```

**useEffect 如下写各自代表什么**

```JavaScript
useEffect(callback, []);

useEffect(callback);

useEffect(callback, [deps]);
```

**当连续点击 10 次时，会输出多少**

```js
function App() {
  const [count, setCount] = useState(0);
  const incr = () => {
    setTimeout(() => {
      setCount(count + 1);
    }, 3000);
  };
  return <h1 onClick={incr}>{count}</h1>;
}

// 10
```

#### 将手机号中间 4 位变成\* ,**用尽可能多的方式，将手机号中间 4 位变成\***

```javascript
const tel = 18877776666;
let tel1 = "" + tel;

// 方式1
let arr = tel1.split("");
arr.splice(3, 4, "****");
tel1 = arr.join("");
console.log(tel1);

// 方式2
tel1 = tel1.substr(0, 3) + "****" + tel1.substr(7);
console.log(tel1);

// 方式3
tel1 = tel1.replace(tel1.substring(3, 7), "****");
console.log(tel1);

// 方式4
const reg = /(\d{3})\d{4}(\d{4})/;
tel1 = tel1.replace(reg, "$1****$2");
console.log(tel1);
```

#### 红绿灯

**循环打印红黄绿**

**要求：绿灯 1s 亮一次，黄灯 2s 亮一次，红灯 3s 亮一次；如何让三个灯不断交替重复亮灯？**

**三个亮灯函数：**

```javascript
function red() {
	console.log(‘red’)
}
function green() {
	console.log(‘green’)
}
function yellow() {
	console.log(‘yellow’)
}
```

```javascript
const task = (timer, light) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      if (light === "red") {
        red();
      } else if (light === "green") {
        green();
      } else if (light === "yellow") {
        yellow();
      }
    }, timer);
  });
};

const step = () => {
  task(1000, "green")
    .then(() => task(2000, "yellow"))
    .then(() => task(3000, "red"))
    .then(step);
};
```

### 输出结果

**1.以下代码输出结果**

```javascript
const promise = new Promise((resolve, reject) => {
  console.log(1);
  setTimeout(() => {
    console.log("timerStart");
    resolve("success");
    console.log("timerEnd");
  }, 0);
  console.log(2);
});
promise.then((res) => {
  console.log(res);
});
console.log(4);
```

**`1 2 4 timerStart timeEnd success`
**

**1.1 上一的变种（resolve 放到 setTimeout 外调用）：**

```javascript
const promise = new Promise((resolve, reject) => {
  console.log(1);
  setTimeout(() => {
    console.log("timerStart");
  }, 0);
  resolve("success");
  console.log(2);
});
promise.then((res) => {
  console.log(res);
});
console.log(4);
```

`1 2 4 success timerStart`

**1.2 解释为什么这么输出，并询问 event loop**

### 数据结构转换

**将 js 对象转化为树形结构**

```javascript
source = [
  {
    id: 1,
    pid: 0,
    name: "body",
  },
  {
    id: 2,
    pid: 1,
    name: "title",
  },
  {
    id: 3,
    pid: 2,
    name: "div",
  },
];

tree = [
  {
    id: 1,
    pid: 0,
    name: "body",
    children: [
      {
        id: 2,
        pid: 1,
        name: "title",
        children: [
          {
            id: 3,
            pid: 2,
            name: "div",
          },
        ],
      },
    ],
  },
];

function buildTree(data) {
  var tree = []; // 存放最终生成的树形数据

  function findParentNode(parentId, nodes) {
    for (var i = 0; i < nodes.length; i++) {
      if (nodes[i].id === parentId) {
        return nodes[i];
      } else if (nodes[i].children && nodes[i].children.length > 0) {
        var result = findParentNode(parentId, nodes[i].children);
        if (result !== null) {
          return result;
        }
      }
    }

    return null;
  }

  data.forEach((node) => {
    node.children = []; // 初始化每个节点的子节点为空数组

    var parentNode = findParentNode(node.parentId, tree);
    if (parentNode) {
      parentNode.children.push(node);
    } else {
      tree.push(node);
    }
  });

  return tree;
}

// 测试数据
var data = [
  { id: "1", name: "节点1" },
  { id: "2", name: "节点2", parentId: "1" },
  { id: "3", name: "节点3", parentId: "1" },
  { id: "4", name: "节点4", parentId: "2" },
  { id: "5", name: "节点5", parentId: "2" },
  { id: "6", name: "节点6", parentId: "3" },
  { id: "7", name: "节点7", parentId: "3" },
];

console.log(buildTree(data));
```

### 类型判断

**1.如何通过 es5 判断是 array 还是 object**

```javascript
Object.prototype.toString.call([]);
// [object Array]

Object.prototype.toString.call({});
// [object Object]

Object.prototype.toString.call(new Date());
// [object Date]

Object.prototype.toString.call(() => {});
// [object Function]

Object.prototype.toString.call(NaN);
// [object Number]

Object.prototype.toString.call(1);
// [object Number]

Object.prototype.toString.call("1");
// [object String]

Object.prototype.toString.call(null);
// [object Null]

Object.prototype.toString.call(undefined);
// [object Undefined]
```

### queryString 函数

**封装一个 queryString 函数，支持输入 URL 来获取 query 等参数**

```
https://www.antgroup.com?name=ant#/home?job=frontend&extraInfo=%7B%22a%22%3A%22b%22%2C%22c%22%3A%22d%22%7D
可得到一个这样的对象
{name:"ant",from:'alibaba',job:'frontend',extraInfo: {a: 'b', c: 'd'}}
```

```javascript
function queryString(url) {
  const params = new URLSearchParams(url.split("?")[1]);
  const result = {};

  params.forEach((value, key) => {
    if (result[key]) {
      if (typeof result[key] === "string") {
        result[key] = [result[key]];
      }
      result[key].push(value);
    } else {
      result[key] = value;
    }
  });

  return result;
}

// 测试
const url =
  "https://www.antgroup.com?name=ant#/home?job=frontend&extraInfo=%7B%22a%22%3A%22b%22%2C%22c%22%3A%22d%22%7D";
console.log(queryString(url));
```

**5.主要考察基本组件开发能力(P5)**

**// 用 React 实现 **[受控](https://reactjs.org/docs/forms.html#controlled-components) 和 [非受控](https://reactjs.org/docs/uncontrolled-components.html) Input 组件

```javascript

// 受控 Input 组件
import React, {useState} from 'react';

function ControlledInput() {
  const [value, setValue] = useState('');

  const handleChange = (event) => {
    setValue(event.target.value);
  }

  return (
    <input type="text" value={value} onChange={handleChange} />
  )
}

export default ControlledInput;

//非受控 Input 组件：

import React, {useRef} from 'react';

function UncontrolledInput() {
  const inputRef = useRef(null);

  const handleClick = () => {
    console.log(inputRef.current.value);
  }

  return (
    <div>
      <input type="text" ref={inputRef} />
      <button onClick={handleClick}>Submit</button>
    </div>
  )
}

export default UncontrolledInput;

```

**实现一个 useDebounce hook，使用场景：上述受控 input 组件，在停止输入 500ms 后，根据 value 进行 search。具体使用方式如下：**

```javascript
const debouncedValue = debouncedValue(inputValue, 500);
useEffect(() => {
  // do search function
}, [debouncedValue]);
```

```javascript
export default function useDebounce<T>(value: T, delay?: number): T {
  const [debouncedValue, setDebouncedValue] = useState < T > value;

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay || 500);

    return () => {
      clearTimeout(timer);
    };
  }, [value, delay]);

  return debouncedValue;
}
```

### 弹窗组件

**用 react 实现一个弹窗组件可以通过 Modal.show() 、Modal.hide()方式控制弹窗组件的显示隐层，并能挂载到 body 下面**

```javascript
const ModalComponent = () => {}; // ModalComponent具体实现不重要，主要考察Modal

const Modal = (Component) => {
  const container = document.createElement("div");
  const hide = function () {
    ReactDOM.unmountComponentAtNode(container);
    document.body.removeChild(container);
  };
  const show = function (props) {
    document.body.appendChild(container);
    ReactDOM.render(
      ReactDOM.createPortal(<Component {...props} hide={hide} />, container),
      container
    );
  };

  return {
    show,
    hide,
  };
};

export default Modal(ModalComponent);
```

### flex 布局

```
flex: 0 1 auto; 各自代表的意思？
参考答案：
flex属性是flex-grow，flex-shrink和flex-basis的简写
flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
 flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
 flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小
```

6.**用 react 实现一个 input 组件能将输入包含 逗号(半角,全角)、空格、 换行符分隔的字符串转换为数组进行 onChange 输出， 输入框内展示为以半角逗号分隔的字符串。**请考虑用户体验， 不能影响用户输入\*\*\*\*

```javascript
import React, { useState } from "react";

function Input() {
  const [inputValue, setInputValue] = useState("");
  const [outputValue, setOutputValue] = useState([]);

  const handleInputChange = (event) => {
    const inputValue = event.target.value;
    setInputValue(inputValue);

    const parsedValue = parseInputValue(inputValue);
    setOutputValue(parsedValue);
  };

  const parseInputValue = (value) => {
    const separators = [",", "，", " ", "\n"];
    const parsedValue = value
      .split(new RegExp(separators.join("|")))
      .filter((val) => val !== "")
      .map((val) => val.trim());

    return parsedValue;
  };

  return (
    <div>
      <textarea value={inputValue} onChange={handleInputChange} rows={3} />
      <div>Output: {outputValue.join(", ")}</div>
    </div>
  );
}

export default Input;
```

7.**编写一个函数，判断一个字符串是否为回文字符串（正着读和倒着读都一样），例如 "racecar" 就是一个回文字符串。**

```javascript
function isPalindrome(str) {
  // 将字符串转成数组并反转
  const reversed = str.split("").reverse().join("");
  // 判断反转后的字符串是否与原字符串相同
  return str === reversed;
}

// 测试
console.log(isPalindrome("racecar")); // true
console.log(isPalindrome("hello")); // false
```

8.**实现一个函数，输入一个整数数组和一个数字 k，查找数组中第 k 大的数。假设 k 总是有效的。**

```javascript
function findKthLargest(nums, k) {
  // 使用快速排序的思想
  const pivot = nums[0]; // 取第一个数作为 pivot
  let left = [];
  let right = [];
  for (let i = 1; i < nums.length; i++) {
    if (nums[i] > pivot) {
      right.push(nums[i]); // 大于 pivot 的放右边
    } else {
      left.push(nums[i]); // 小于等于 pivot 的放左边
    }
  }
  if (right.length >= k - 1) {
    // 第 k 大的数在右边
    return findKthLargest(right, k);
  } else if (right.length === k - 1) {
    // 第 k 大的数就是 pivot
    return pivot;
  } else {
    // 第 k 大的数在左边
    return findKthLargest(left, k - right.length - 1);
  }
}

// 测试
console.log(findKthLargest([3, 2, 1, 5, 6, 4], 2)); // 5
console.log(findKthLargest([3, 2, 3, 1, 2, 4, 5, 5, 6], 4)); // 4
```

1. **实现一个函数，输入一个字符串，判断字符串中是否包含重复字符。**

```javascript
function hasDuplicateChars(string) {
  const charMap = {};
  for (let char of string) {
    if (charMap[char]) {
      return true;
    }
    charMap[char] = true;
  }
  return false;
}

// 测试
console.log(hasDuplicateChars("abcdefg")); // false
console.log(hasDuplicateChars("abcdeafg")); // true
```

1. **实现一个函数，输入一个数组和一个数字 target，找出数组中两个数的和等于 target，并返回这两个数的下标。**

```JavaScript
function findTwoSum(nums, target) {
  const numMap = {};
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (numMap.hasOwnProperty(complement)) {
      return [numMap[complement], i];
    }
    numMap[nums[i]] = i;
  }
  return [];
}

// 测试
console.log(findTwoSum([2, 7, 11, 15], 9));  // [0, 1]
console.log(findTwoSum([3, 2, 4], 6));  // [1, 2]
console.log(findTwoSum([3, 3], 6));  // [0, 1]
```

1. **实现一个函数，输入一个字符串，将字符串中的单词全部反转。**

```JavaScript
function reverseWords(str) {
  // 使用空格将字符串分割成单词数组，然后将数组反转，最后用空格将单词数组连接起来
  return str.split(' ').reverse().join(' ');
}

// 测试
console.log(reverseWords("Hello World"));  // "World Hello"
console.log(reverseWords("I love JavaScript"));  // "JavaScript love I"
```

1. **实现一个函数，输入一个字符串，判断该字符串是否为回文串。**

```javascript
function isPalindrome(str) {
  // 将字符串转换为小写，并去除非字母和数字的字符
  const formattedStr = str.toLowerCase().replace(/[^a-z0-9]/g, "");

  let left = 0;
  let right = formattedStr.length - 1;

  while (left < right) {
    if (formattedStr[left] !== formattedStr[right]) {
      return false;
    }
    left++;
    right--;
  }

  return true;
}

// 测试
console.log(isPalindrome("A man, a plan, a canal: Panama")); // true
console.log(isPalindrome("race a car")); // false
```

1. **实现一个函数，输入一个数组，将数组中的所有元素按照奇偶性分组。奇数排在偶数的前面，原数组中的相对位置不变。**

```JavaScript
function groupOddEven(nums) {
  let left = 0;
  let right = nums.length - 1;

  while (left < right) {
    while (left < right && nums[left] % 2 !== 0) {
      left++;
    }
    while (left < right && nums[right] % 2 === 0) {
      right--;
    }
    if (left < right) {
      // 交换奇数和偶数的位置
      [nums[left], nums[right]] = [nums[right], nums[left]];
    }
  }

  return nums;
}

// 测试
console.log(groupOddEven([1, 2, 3, 4, 5, 6]));  // [1, 5, 3, 4, 2, 6]
console.log(groupOddEven([2, 4, 6, 1, 3, 5]));  // [1, 3, 5, 2, 4, 6]
```

1. **实现一个函数，输入一个字符串，将字符串中的每个单词的首字母变成大写字母，并返回新的字符串。**

```JavaScript
function capitalizeFirstLetter(str) {
  const words = str.split(' ');
  const capitalizedWords = words.map(word => {
    return word.charAt(0).toUpperCase() + word.slice(1);
  });
  return capitalizedWords.join(' ');
}

// 测试
console.log(capitalizeFirstLetter("hello world"));  // "Hello World"
console.log(capitalizeFirstLetter("i love javascript"));  // "I Love Javascript"

```

1. **实现一个函数，输入一个字符串，返回字符串中第一个不重复的字符。**

```JavaScript
function firstUniqueChar(str) {
  const charMap = {};

  for (let char of str) {
    if (charMap[char]) {
      charMap[char]++;
    } else {
      charMap[char] = 1;
    }
  }

  for (let char of str) {
    if (charMap[char] === 1) {
      return char;
    }
  }

  return "";
}

// 测试
console.log(firstUniqueChar("leetcode"));  // "l"
console.log(firstUniqueChar("loveleetcode"));  // "v"
console.log(firstUniqueChar("aabbcc"));  // ""

```

1. **实现一个函数，输入一个字符串，将字符串中的所有数字序列加 1，并返回新的字符串。例如："1a2b3c" => "2a3b4c"。**

```JavaScript
function incrementNumberSequence(str) {
  return str.replace(/\d+/g, function(match) {
    return String(Number(match) + 1);
  });
}

// 测试
console.log(incrementNumberSequence("1a2b3c"));  // "2a3b4c"
console.log(incrementNumberSequence("hello 123 world"));  // "hello 124 world"
console.log(incrementNumberSequence("abc"));  // "abc"

```

1. **实现一个函数，输入一个有序数组和一个目标值，使用二分查找算法在数组中查找该值的下标。**

```JavaScript
function binarySearch(nums, target) {
  let left = 0;
  let right = nums.length - 1;

  while (left <= right) {
    let mid = Math.floor((left + right) / 2);
    if (nums[mid] === target) {
      return mid;
    } else if (nums[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }

  return -1;
}

// 测试
console.log(binarySearch([1, 3, 5, 7, 9], 3));  // 1
console.log(binarySearch([1, 3, 5, 7, 9], 10));  // -1

```

1. **实现一个函数，输入一个字符串和一个模式串，判断字符串是否匹配模式串。其中模式串中可以包含"."和"** _"两个特殊字符，"."可以匹配任意一个字符，"_ **"可以匹配任意多个字符（包括 0 个字符）**

```JavaScript
function isMatch(str, pattern) {
  const m = str.length;
  const n = pattern.length;

  // 创建一个二维数组dp，dp[i][j]表示str前i个字符和pattern前j个字符是否匹配
  const dp = Array(m + 1).fill(false).map(() => Array(n + 1).fill(false));

  // 初始化dp[0][0]为true，表示空字符串和空模式串匹配
  dp[0][0] = true;

  // 初始化第一行，考虑pattern以*开头的情况
  for (let j = 1; j <= n; j++) {
    if (pattern[j - 1] === ' ') {
      dp[0][j] = dp[0][j - 2];
    }
  }

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (pattern[j - 1] === str[i - 1] || pattern[j - 1] === '.') {
        // 如果当前字符匹配，则取决于前面的字符是否匹配
        dp[i][j] = dp[i - 1][j - 1];
      } else if (pattern[j - 1] === ' ') {
        // 如果当前字符是*，则有两种情况：
        // 1. *匹配0个字符，即pattern前两个字符被忽略，dp[i][j-2]为true
        // 2. *匹配1个或多个字符，即str的当前字符和pattern的前一个字符匹配，且dp[i-1][j]为true
        dp[i][j] = dp[i][j - 2] || (dp[i - 1][j] && (str[i - 1] === pattern[j - 2] || pattern[j - 2] === '.'));
      }
    }
  }

  return dp[m][n];
}

// 测试
console.log(isMatch("aa", "a"));  // false
console.log(isMatch("aa", "a."));  // true
console.log(isMatch("ab", ".*"));  // true
console.log(isMatch("aab", "c*a*b"));  // true
console.log(isMatch("mississippi", "mis*is*p*."));  // false

```
