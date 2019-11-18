# Ajax

> Ajax 是 Asynchronous JavaScript And XML 的缩写，中文翻译是异步 JavaScript 和 XML。 Ajax 用 XMLHttpRequest 对象访问服务器。 Ajax 可以发送和接各种格式的信息，比如JSON、XML、HTML、txt等。由于Ajax异步的原因，使用 Ajax 和服务器交换数据的时候，页面不用刷新。

### 两大特性
1. 请求的时候不会刷新页面
2. 可以接收并处理来自服务器的数据

### 如何使用Ajax

```javascript
// 根据不同的浏览器版本创建HttpRequest对象
let httpRequest;
if(window.XMLHttpRequest) {
    // 现代浏览器以及IE7以上
    httpRequest = new XMLHttpRequest();
}else if(window.ActiveXObject){
    // IE6及其以下版本
    httpRequest = new ActiveXObject("Microsoft.XMLHTTP");
}
// 用过 get 方法打开 http://api.example.com/users，异步方法
// 第一个参数是请求方法，如get，post，put等
// 第二个参数是请求地址
// 第三个参数是启用异步还是同步，默认位true
// true：异步，推荐使用true参数，使用false的用户体验不是很好
// false：同步
httpRequest.open("GET","http://api.example.com/users",true);
// 设置请求头，必须在 open 和 send 方法之间调用
httpRequest.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
// 发送请求
// send的参数可以是任何类型的数据，如果是get以外的请求，要设置请求头
httpRequest.send();
// 处理服务器的响应
// 第一步：这个函数检查请求的状态，如果状态的值是XMLHttpRequest.DONE，即 readyState 为 4 时，
// 表示服务器接收到了这个请求，可以进行下一步了
// 第二步：
httpRequest.onreadystatechange = function() {
    if(httpRequest.readyState === XMLHttpRequest.DONE) {
        // 服务器接收到了请求
        // 判断 http 请求的 status 的值，更多 status 值参考：https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html
        if(httpRequest.status === 200) {
            // 响应成功
            // 获取数据的两种方法
            // httpRequest.responseText：以字符串的形式返回服务器返回的数据，一般情况使用这种方法
            // httpRequest.responseXML：以 XML 文档的形式返回服务器返回的数据
            let responseText = httpRequest.responseText;
        } else {
            // 响应失败
        }
    } else {
        // 还没有准备好
    }
};

```
> readyState的值

- 0： 请求还未初始化
- 1： 客户端已和服务器建立连接
- 2： 服务器已接收到请求
- 3： 处理请求
- 4： 请求完成并响应

### Ajax 的封装
为了更好的使用 Ajax，就必须对 Ajax 进行封装

```javascript
function ajax({url, method = "get", data, headers = [], isAsync = true}){

    method = method.toUpperCase();

    if(method === "GET" && data){
        let keys = Object.keys(data);
        let url_suffix = "?";
        for(let i = 0; i < keys.length; i++) {
            url_suffix += keys[i] + "=" + JSON.stringify(data[keys[i]]);
            if(i !== keys.length-1) {
                url_suffix +="&";
            }
        }
        url+=url_suffix;
    }

    function createHttpRequest(){
        if(window.XMLHttpRequest) {
            return httpRequest = new XMLHttpRequest();
        }else if(window.ActiveXObject){
            return httpRequest = new ActiveXObject("Microsoft.XMLHTTP");
        }
    }

    function setRequestHeaders(httpRequest, headers){
        let keys = Object.keys(headers);
        for(let i = 0; i < keys.length; i++){
            httpRequest.setRequestHeader(keys[i], headers[keys[i]]);
        }
    }

    function send(httpRequest, method, data){
        if(method !== "GET" && data){
            httpRequest.send(data);
        }else{
            httpRequest.send();
        }
    }

    function handleReadyStateChange(httpRequest,resolve,reject){
        if(httpRequest.readyState === XMLHttpRequest.DONE) {
            if(httpRequest.status === 200) {
                let response = httpRequest.response;
                resolve(response);
            } else {
                reject();
            }
        } else {
            reject();
        }   
    }

    return new Promise((resolve, reject) => {
        let httpRequest = createHttpRequest();
        
        httpRequest.open(method.toUpperCase(), url, isAsync);

        setRequestHeaders(httpRequest, headers);

        send(httpRequest, method, data);

        httpRequest.onreadystatechange = handleReadyStateChange(httpRequest,resolve,reject);
    });
}
```