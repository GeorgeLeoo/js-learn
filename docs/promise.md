# Promise

在 js 中，所有代码都是单线程的。要想实现异步操作就必须使用回调函数
```javascript
function callback(){
    console.log("callback");
}
console.log("before setTimeout");
setTimeout(callback, 1000);
console.log("after setTimeout");
```
最后输出的是
```
before setTimeout
after setTimeout
callback    // 2s后输出
```

但是使用回调函数会有一个问题，就是会形成地狱回调，回调会嵌套很深，这样代码就非常的不优雅，所以就有了 promise。

### Promise概念 
Promise 时候 ES6 发布的，是为了解决异步编程的地狱回调问题，自己的方法有 all、reject、resolve，
原型上有 then、catch 方法

### Promise两个特点
1. 对象的状态不受外界影响。Promise对象代表一个异步操作，
有三种状态：
    - pending（进行中）
    - fulfilled（已成功）
    - rejected（已失败）

2. 一旦状态发生，任何其他操作都无法改变这个状态，
状态发生变化只有两种可能，
    - 从 pending 变为 fulfilled
    - 从 pending 变为 rejected

```javascript
function foo(){
    return new Promise((resolve, reject) => {
        if(condition){
            resolve("success");
        }else{
            reject("fail");
        }
    });
}
foo().then((res,err)=>{
    
})
```
