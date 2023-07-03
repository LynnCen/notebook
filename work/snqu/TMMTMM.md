# TMMTMM PC

技术栈：react+mobx+electron+node+dexie+ts+ws+antd ...

### @消息

1. 检测输入框最后一个符号是否为@ 

2. 唤起群成员列表框

3. 插入自定义@标签到输入框(类似插入表情)

4. 封装@消息 内容content参考后端文档

5. 页面根据消息的type字段进行区分不同类型的消息 

6. 拉去消息时 处理与我有关的@消息显示@图标并且消息提醒

7. 键盘上下选人

8. 复制，引用和翻译

### 删除账号

1. 更改用户的status

2. 收到账号删除的cmd消息，删除数据库并且退出登录

3. 删除账号后 捕获接口异常 并且更新入库

4. 账号删除后 其他用户的显示逻辑 在getNameweight中判断

5. contacts页面 好友申请 pyq 账号已删除用户的显示

      

### 文件另存和拖拽保存

**文件另存**

![1663826100942](C:\Users\SNQU\AppData\Roaming\Typora\typora-user-images\1663826100942.png)

**文件拖拽保存**

图片：给img设置属性  `draggable="true"` 

视频和文件 使用electron的 `startDrag` 

![dragFiles](D:\cen-sources\notebook\img\dragFiles.png)

### 群详情信息

群管理 群编辑 群简介 位于Tmmsessionboard下

### 群管理

### 会话静音，置顶和隐藏

session数据中对应字段 isMute isTop和hideInfo 多端同步

ui层根据静音和置顶字段来进行显示

隐藏只在本地做，根据最大index或者seq来进行判断 拉去会话时就会将hideinfo入库 逻辑层进行判断

### 群公告

将群公告的内容进行md5加密 和对本地内容进行判断是否是新公告

### 草稿

输入框 边输边入库 包括引用 发送消息或删除输入框的内容删除该条草稿

### 联系人和会话搜索

dexie并没有提供模糊搜索的api 并且indexDB是关系型数据库 通过建立索引来查询 加快查询效率

因此需要对整个用户表和会话表整个进行遍历 搜索优先级产品文档有规定

根据优先级将各个表的搜索数据进行求并集 返回给ui层显示

### 求并集算法

**搜索关键字**

![searchKey](D:\cen-sources\notebook\img\searchKey.PNG)

**求并集**

![union](D:\cen-sources\notebook\img\union.PNG)



![memberandgroup](D:\cen-sources\notebook\img\memberandgroup.PNG)

### win，mac自动更新

使用**electron-builder**和electron内部的**autoUpdater**

![autoUpdater](D:\cen-sources\notebook\img\autoUpdater.jpg)



**autoUpdater自动更新流程**

![electroon-update](D:\cen-sources\notebook\img\electroon-update.png)

### 断网拉取会话信息

```javascript
// 监听网络连接断开
        window.addEventListener("online", async () => {
            console.log("online--------------------");
            WSClient.reconnection(); // need websocket reconnection
            clearAwaitSendMessage();
            // pull session
            await initSessionConfig();
        });
```

### 最近表情

在**keyValue**表中 增加一条数据last_emoji  ps：只在本地

### 退出群聊

pc主动退出 删除会话 删除消息  多端同步

```javascript
 // delete current user;
        await db?.groupMembers
            ?.where("gid")
            .equals(gid)
            .and((item: GroupMembers) => item.uid === UserInfo._id)
            // ?.delete();
            .modify({ deleted: true, isAdmin: 0, isOwner: 0 });

        await db?.groupInfo
            ?.where("uid")
            .equals(UserInfo._id)
            .filter((item: GroupInfo) => item.id === gid)
            .modify({ kicked: true });
```



### 递归 带有表情String

```javascript
const recursionEmojiText = (text) => {
            const emojiMap = arrayToMap(emojiMapData, "code");
            // array  the match string is the  0 indexof array -> matchEmoji[0] = first match emoji string
            const matchEmoji = text.match(/\[.*?\]/);
            console.log(matchEmoji);
            if (!matchEmoji) return insertText(text);
            const emoji = emojiMap.get(matchEmoji[0]);
            console.log(emoji);
            const indexEmoji = text.indexOf(matchEmoji[0]);
            // slice emojiText such as this -> "preText (first match emoji string)  lastText"
            const preText = text.slice(0, indexEmoji);
            const lastText = text.slice(indexEmoji + matchEmoji[0].length, text.length);
            //insert the preText
            if (preText.trim()) insertText(preText);
            //insert the first match emoji string
            // judge is emoji or another match strs which such as （^(?!http)[A-Za-z\u4e00-\u9fa5]+[\u4E00-\u9FA5A-Za-z\:\_\.\-\d]*$）
            if (emoji) {
                selectEmojiWithImg(emoji);
            } else {
                insertText(matchEmoji[0]);
            }
            //judge the lastText wheather include the match emoji string
            // if true recursion call the recursionEmojiText
            // if false insertText lastText
            if (lastText.match(/\[.*?\]/gim)) {
                recursionEmojiText(lastText);
            } else {
                insertText(lastText);
            }
        };
```

### 文件下载卡顿

文件 图片 视频下载流程

![files](D:\cen-sources\notebook\img\files.jpg)

### 键盘上下选人

![keyboardUpandDown](D:\cen-sources\notebook\img\keyboardUpandDown.PNG)

### 校验Code码是否为我们平台合法code码

路由参数: t="..."&code="AWZiY2JmMzFlMDcyZDJkMGSg" 

① code码使用base64编码 base64会将 _下划线 替换为 /反斜杠 -减号替换为 +加号 ，需要我们反替换回去，在进行base64解码，如UrlDeCode方法；

② 将转为base64的str 转为对应的ascii数组 例如 ：

"AWZiY2JmMzFlMDcyZDJkMGSg"  -》  [1 102 98 99 98 102 51 49 101 48 55 50 100 50 100 48 100 160]

③ 第一位为版本号 最后一位为检验位 除最后一位 其余各位进行类似验证身份证的运算 `p = (p + v * dictionaryMap[i]) % 251;`  如果算出的p等于校验位 则为合法code
 思考？ 为什么不使用md5或者sha1进行加密 ？

```typescript
export function verifyCode(code: string): boolean {
  let base64 = UrlDeCode(code);
  const asciiArr = strToAscii(base64);
  const operationArr = asciiArr.slice(0, asciiArr.length - 1);
  const checkBit = asciiArr[asciiArr.length - 1];
  let p = 0;
  operationArr.forEach((v, i) => {
    p = (p + v * dictionaryMap[i]) % 251;
  });
  if (p == checkBit) return true;
  return false;
}

export function strToAscii(code: string): number[] {
  if (!code || code.length < 1) return [];
  const codeArr = code.split('');
  let ascii: number[] = [];
  codeArr.forEach((item, index) => {
    let asciiCode = item.charCodeAt(0);
    ascii.push(asciiCode);
  });
  return ascii;
}

export function UrlDeCode(url: string) {
  if (!url) return '';
  let strDecode = url.replaceAll('/', '_').replaceAll('+', '-');
  return atob(strDecode);
}
```

**知识补充：**

##### base64编码

**概述**

 Base64编码 是一种基于 64 个可打印字符来表示二进制数据的方法。目前 Base64 已经成为网络上常见的传输 8 位二进制字节代码的编码方式之一。 

**编码过程**

将待转换的字符串每三个字节分为一组，每个字节占 8 个二进制位，那么共有 24 个二进制位。
将第（1）步得到的每 24 个二进制位分为每 6 个一组，则每 3 个字节可分为 4 组。
在每组前面添加两个 0 ，每组由 6 个二进制位变为 8 个二进制位，总共 32 个二进制位，即四个字节。

挺多人会误以为 Base64 是加密算法，喜欢在数据传输不安全的时候来一句：使用 Base64 加密一下呗。

我们先来看一下加密的定义：在密码学中，加密（英语：Encryption）是将明文信息改变为难以读取的密文内容，使之不可读的过程。只有拥有解密方法的对象，经由解密过程，才能将密文还原为正常可读的内容。

所谓加密算法就是加密的方法。

如果你使用 Base64 进行 “加密” ，第三方拿到数据可以瞬间解密，根本无法实现你想要的 加密 效果。因此，Base64 不能说是 加密算法，只能说是 编码。




### 



# TMMTMM官网

技术栈：react+ts+next+node+antd

faq和用户手册 国际化🌐  ssr seo 自适应移动端



# TMMTMM后台管理

技术栈：umi+react+ts+dva+antd design Pro

数据统计，权限管理，包管理，日程安排，版本详情等



# TMMTMM h5

技术栈：umi+react+ts+antd+redux

下载页面，版本更新页面，支付页面，人机验证页面，跳转app页面































