##  按位运算

下面是各种按位运算符的说明：

1. 按位与（&）：对两个数值的二进制位进行与运算，只有两位都是 1 时，结果才是 1，否则为 0。

2. 按位或（|）：对两个数值的二进制位进行或运算，只要两位中有任意一位是 1，结果就是 1，否则为 0。

3. 按位异或（^）：对两个数值的二进制位进行异或运算，如果两位不同，则结果为 1，否则为 0。

4. 左移位（<<）：将一个数值的二进制位向左移动指定的位数，空出的位用 0 填充。

5. 有符号右移位（>>）：将一个数值的二进制位向右移动指定的位数，空出的位用符号位（即最高位）填充。

6. 无符号右移位（>>>）：将一个数值的二进制位向右移动指定的位数，空出的位用 0 填充。

``` js
const a = 5; // 二进制表示为 101
const b = 3; // 二进制表示为 011

const c = a & b; // 按位与运算，结果为 001，即 1
const d = a | b; // 按位或运算，结果为 111，即 7
const e = a ^ b; // 按位异或运算，结果为 110，即 6
const f = a << 2; // 左移 2 位，结果为 10100，即 20
const g = a >> 1; // 有符号右移 1 位，结果为 10，即 2
const h = a >>> 1; // 无符号右移 1 位，结果为 10，即 2
```

需要注意的是，按位运算符只能用于整数，如果用于非整数，则会将非整数转换为整数进行计算。另外，按位运算符的操作数是数值类型，如果操作数是字符串类型，则会将其转换为数值类型进行计算。



## 如何确定函数的执行上下文，以及堆栈信息
思考以下代码 
前置：初始resultSheetFile为CustomizeFile，handleUploadConfirm接受UploadFile并更改resultSheetFile
打印的pre和current的resultSheetFile是否相等
``` ts

interface CustomizeFile {
  name: string;
  downloadSrc: string;
}
 const [state, setState] = useSetState<StateType>(initialValues);
  const {
    type,
    downloadTemplateSrc,
    projectSheetFile,
    resultSheetFile,
    projectSheetFileStatus,
    resultSheetFileStatus,} = state;
  const handleUploadConfirm = (file: UploadFile) => {
    // file {file:xxx,uid,filename.......}
    setuploadVisible(false);
    const key = type === UpLoaderType.ProjectSheet ? 'projectSheetFile' : 'resultSheetFile';
    if (isResultSheet) queryGenerateSearchItems(file);
    const fileStatus = 
      type === UpLoaderType.ProjectSheet ? 'projectSheetFileStatus' : 'resultSheetFileStatus';
    console.log('pre', resultSheetFile); // pre {name: 'x',downloadSrc:'sss' }
    setState(
      {
        [key]: file,
        [fileStatus]: {
          status: 'success',
          errorMsg: null,
        },
      },
      () => {

        console.log('current', resultSheetFile);// pre {name: 'x',downloadSrc:'sss' }
      }
    );
  };
```
**现象：**
pre和current打印的值相等
  ![1](./img/log1.png)、
此时handleUploadConfirm的堆栈信息
 ![2](./img/socpe1.png)、
setState的回掉函数：
 ![3](./img/scope2.png)、
执行setState的回调函数，发现此时的state已经改变了
 ![4](./img/setState之后的scope.png)、

疑问：该如何确定一个函数的执行上下文、作用域链