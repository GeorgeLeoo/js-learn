# 变量的定义

> js中定义变量有多种方法，可以使用var、let、const或者直接使用

### var
var是 ES6 之前使用的变量声明关键字，var是全局声明，没有像c语言这样的强类型语言的块级作用域。更多关于作用域请参考[作用域](scope)

> 未声明的变量直接使用会报错

``` javascript
console.log(a); //  Uncaught ReferenceError: a is not defined
```

> var 声明的变量存在变量提升

``` javascript
console.log(a); //  undefined

var a = 1;
```
这个例子中，我们发现 a 没有报错，而是打印输出 undefined。这和js引擎有关，js在执行文件之前会先扫描所有的代码，然后把用var声明的变量提升到整个文件的最上面，但是不会带上我们给该变量赋的值。我们可以将扫描后的代码看做是这样

``` javascript
var a;

console.log(a); // undefined

a=1;
```
对于未赋值的变量，它会自带一个初始化值，即undefined

> var 是全局声明，不存在局部变量，声明的变量不存在块级作用域，而且可以重复使用var关键字定义同一个变量

``` javascript
var a = 1;  // 全局变量a

if (true) {
    var a = 2;    // 全局变量a
    console.log(a); //  2
}
console.log(a); //  2
```
这里两个打印a的值都是2，这里的a是同一个a。而在c语言中，a的值为1，在c语言中if的大括号中声明的变量是局部变量。

### let
let是ES6中新增的变量声明关键字，let的存在让js的更加先进了，拥有块级作用域。
``` javascript
let a = 1;  // 全局变量 a

if (true) {
    let a = 2;   // 局部变量 a
    console.log(a); // 2
}
console.log(a); //  1
```
在这个例子中，我们发现 if 里面打印的是 2，外面打印的是 1，if 里面声明的变量 a 并没有对外面的变量 a 造成影响。这就是 let 所在的大括号生成块级作用域

> 代码块内声明的变量，外部访问不到

``` javascript
{
    let a=1;
    var b=2;
}
console.log(a); // Uncaught ReferenceError: a is not defined
console.log(b); // 2
```
在这个例子中，let声明的在外部并不能被访问，而使用var声明的b可以被外部访问到

> let 不许重复定义

```javascript
let a = 1;
let a = 2;  //  Uncaught SyntaxError: Identifier 'a' has already been declared
```
在使用var的时候，我们说过var可以重复定义同一个变量名，但是let却不被允许这样做，在这个例子中，我们用let重复定义了两个a，结果报错了，提示a已经被声明了。

### const
const 是 ES6 中新增的一个关键字，用来定义一个常量。常量是块级作用域，很像使用 let 语句定义的变量。常量的值不能通过重新赋值来改变，并且不能重新声明。const声明创建一个值的只读引用，但这并不意味着它所持有的值是不可变的，只是变量标识符不能重新分配。

> 常量不可以重新赋值

``` javascript
const a = 1;
a = 2;    //  VM4704:2 Uncaught TypeError: Assignment to constant variable.
```
当运行这个代码的时候，我们发现代码报错了

> 初始化时必须被赋值

``` javascript
const a;    // VM20113:1 Uncaught SyntaxError: Missing initializer in const declaration
```
我们发现const定义但不初始化值就报错了

>常量可以修改它所持有的值

``` javascript
const obj = { a: 1 };
obj.a = 11;
console.log(obj);   // { a: 11 }
```
我们声明了常量对象obj，我们修改了obj的a属性。那为何对象就可以修改呢，参考[对象Object](object)

### 暂时性死区
let或const所在的块级作用域会形成暂时性死区，这个区域不再受外部的影响，是一个相对独立的区域。

```javascript
var a = 1;  // 全局变量a
if(true) {
    a  = 2; 
    let a = 3;  // 局部变量  Uncaught ReferenceError: Cannot access 'a' before initialization
}
console.log(a);
```
这个例子中，存在全局变量a，但是块级作用域内let又声明了一个局部变量a，导致后者绑定这个块级作用域，形成一个相对独立的区域，所以在let声明变量前，对a赋值会报错。

### 是否会被挂载到全局对象上

> let和const不会被挂载到全局window上（NodeJs中是Global），而var会

``` javascript
var a = 1;
let b = 2;
const c = 3;
console.log(window.a);  // 1
console.log(window.b);  // undefined
console.log(window.c);  // undefined
```
