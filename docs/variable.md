# 变量的定义

> js中定义变量有多种方法，可以使用var、let、const或者不声明关键字而直接使用

### var
var是 ES6 之前使用的变量声明关键字，var是全局声明，没有像c语言这样的强类型语言的块级作用域

> 未声明的变量直接使用会报错

``` javascript
console.log(a); //  Uncaught ReferenceError: a is not defined
```

> var 声明的变量存在变量提升

``` javascript
console.log(a); //  undefined

var a = 1;
```
这个例子中，我们发现 a 没有报错，而是打印输出 undefined。这和js引擎有关，js在执行文件之前会先扫描所有的代码，然后把用var声明的变量提升到整个文件的最上面，但是不会带上我们给该变量赋的值。扫描后的代码就像这样

``` javascript
var a;

console.log(a);

a=1;
```
对于未赋值的变量，它会自带一个初始化值，即undefined

> var 是全局声明，不存在局部变量，声明的变量不存在块级作用域，而且可以重复使用var关键字定义同一个变量

``` javascript
var a = 1;  // 全局变量a

if(true){
    var a=2;    // 全局变量a
}
console.log(a); //  2
```
在js中最后打印a的值是2，这里的a是同一个a，全局变量。而在c语言中，a的值为1，在c语言中if的大括号中声明的变量是局部变量。

### let
let是ES6中新增的变量，let的存在让js的更加先进了，拥有块级作用域。
``` javascript
let a = 1;  // 全局变量 a

if(true){
    let a=2;   // 局部变量 a
    console.log(a); // 2
}
console.log(a); //  1
```
在这个例子中，我们发现 if 里面打印的是 2，外面打印的是 1，if 里面声明的变量 a 并没有对外面的变量 a 造成影响。这就是 let 所在的大括号生成块级作用域



