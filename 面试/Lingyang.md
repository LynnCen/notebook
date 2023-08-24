

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

import React, { useState, useEffect } from 'react';

function WindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });

  useEffect(() => {
    function handleResize() {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    }

    window.addEventListener('resize', handleResize);

    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return (
    <div>
      <h1>Window Size: {windowSize.width} x {windowSize.height}</h1>
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
    const observer = new IntersectionObserver((entries) => {
      const image = entries[0];

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


6.**用react 实现一个input组件能将输入包含 逗号(半角,全角)、空格、 换行符分隔的字符串转换为数组进行onChange输出， 输入框内展示为以半角逗号分隔的字符串。**请考虑用户体验， 不能影响用户输入****


```javascript
import React, { useState } from 'react';

function Input() {
  const [inputValue, setInputValue] = useState('');
  const [outputValue, setOutputValue] = useState([]);

  const handleInputChange = (event) => {
    const inputValue = event.target.value;
    setInputValue(inputValue);

    const parsedValue = parseInputValue(inputValue);
    setOutputValue(parsedValue);
  };

  const parseInputValue = (value) => {
    const separators = [',', '，', ' ', '\n'];
    const parsedValue = value
      .split(new RegExp(separators.join('|')))
      .filter((val) => val !== '')
      .map((val) => val.trim());

    return parsedValue;
  };

  return (
    <div>
      <textarea value={inputValue} onChange={handleInputChange} rows={3} />
      <div>Output: {outputValue.join(', ')}</div>
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
2. **实现一个函数，输入一个数组和一个数字target，找出数组中两个数的和等于target，并返回这两个数的下标。**
3. **实现一个函数，输入一个字符串，将字符串中的单词全部反转。**
4. **实现一个函数，输入一个字符串，判断该字符串是否为回文串。**
5. **实现一个函数，输入一个数组，将数组中的所有元素按照奇偶性分组。奇数排在偶数的前面，原数组中的相对位置不变。**
6. **实现一个函数，输入一个字符串，将字符串中的每个单词的首字母变成大写字母，并返回新的字符串。**
7. **实现一个函数，输入一个字符串，返回字符串中第一个不重复的字符。**
8. **实现一个函数，输入一个字符串，将字符串中的所有数字序列加1，并返回新的字符串。例如："1a2b3c" => "2a3b4c"。**
9. **实现一个函数，输入一个有序数组和一个目标值，使用二分查找算法在数组中查找该值的下标。**
10. **实现一个函数，输入一个字符串和一个模式串，判断字符串是否匹配模式串。其中模式串中可以包含"."和"** *"两个特殊字符，"."可以匹配任意一个字符，"* **"可以匹配任意多个字符（包括0个字符）**
