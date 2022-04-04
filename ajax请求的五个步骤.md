# ajax请求的五个步骤

首先，创建一个XMLHttpRequest异步对象

`var xhr;`

`if (window.XMLHttpRequest)` 

 `{// code for IE7+, Firefox, Chrome, Opera, Safari`  

`xhr=new XMLHttpRequest();`  

`}`

 `else`  

`{// code for IE6, IE5`  

`xhr=new ActiveXObject("Microsoft.XMLHTTP");` 

 `}`

```javascript
var xhr = new XMLHttpRequest();
```

然后，设置请求方式和请求地址

```javascript
// get请求如果有参数就需要在url后面拼接参数，
// post如果有参数，就在请求体中传递 
xhr.open("get","validate.php?username="+name)
xhr.open("post","validate.php");
```

第三步：设置请求（GET方式忽略此步骤）头:setRequestHeader()

```javascript
// 1.get不需要设置
// 2.post需要设置请求头：Content-Type:application/x-www-form-urlencoded
xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
```

第四步：设置请求体 send()

```javascript
// 1.get的参数在url拼接了，所以不需要在这个函数中设置
// 2.post的参数在这个函数中设置(如果有参数)
xhr.send(null) xhr.send("username="+name);
```

第五步：让异步对象接收服务器的响应数据

// 一个成功的响应有两个条件：1.服务器成功响应了 2.异步对象的响应状态为4(数据解析完毕可以使用了)

```
xhr.onreadystatechange = function(){ 
if(xhr.status == 200 && xhr.readyState == 4){ 
 console.log(xhr.responseText);
 }
```

#### ajax-get方式请求案例：

```javascript
var xhr = new XMLHttpRequest();
xhr.open("get","validate.php?username="+name);
xhr.send(null);
xhr.onreadystatechange = function(){
if(xhr.status == 200 && xhr.readyState == 4){ console.log(xhr.responseText); document.querySelector(".showmsg").innerHTML = xhr.responseText;;
 }
 }
```

#### ajax-post方式请求案例：

```javascript
var xhr = new XMLHttpRequest();
xhr.open("post","validate.php");
xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
xhr.send("username="+name);
xhr.onreadystatechange = function(){
// 判断服务器是否响应，判断异步对象的响应状态
if(xhr.status == 200 && xhr.readyState == 4){
 document.querySelector(".showmsg").innerHTML = xhr.responseText;
 }
 }
```

