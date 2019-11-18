# 数值Number

js 中的数值可以表示整数也可以表示小数。js 采用的是 IEEE 745 标准定义的 64 位浮点格式表示数字，由于使用该标准的问题，导致 0.1 + 0.2 不等于 0.3。

### 创建数值的方法

``` javascript
// 1. 通过new Number对象
var a = new Number('123'); // a === 123 is false

// 2. 通过全局对象上的Number方法
var b = Number('123'); // b === 123 is true

```

> 两种创建数值方法的比较

```javascript
a === b;    // false
a == b; // true

a instanceof Number; // true
b instanceof Number; // false

typeof a;    // object
typeof b;    // number
```

a是通过new一个Number对象创建的，所以a是一个对象的实例，b是通过Number方法创建的，返回的是一个数，两者的类型不一样，这就是为什么a不全等于b的原因

> 返回值

``` javascript
Number(1);              // 1
Number('2');            // 2
Number('3a');           // NaN
Number('4 2');          // NaN
Number('4 b');          // NaN
Number('');             // 0
Number(true);           // 1
Number(false);          // 0
Number({});             // NaN
Number(new Date());     // 1573881400986
Number(null);           // 0
Number(undefined);      // NaN
Number([]);             // 0
Number(['22']);         // 22
Number(['22', '33']);   // NaN
Number(function(){});   // NaN
```

### Number 属性

```
Number.MAX_SAFE_INTEGER: 最大安全整数
Number.MIN_SAFE_INTEGER: 最小安全整数
Number.MAX_VALUE: 最大数
Number.MIN_VALUE: 最小数
Number.POSITIVE_INFINITY: 正无穷
Number.NEGATIVE_INFINITY: 负无穷
```

### Number 方法

#### Number.parseInt(value)    
> 将字符串转化为整数（ES6）

``` javascript
Number.parseInt('11');       // 11
Number.parseInt(' 11aaa');    // 11
Number.parseInt(' 1 1aaa');    // 1
Number.parseInt('aaa11');    // NaN
``` 
parseInt 在解析时，会忽略字符串开头的空格，总是从第一个字符开始解析，如果第一个字符是数字，就继续下一个字符，直到不是数字字符为止
Number上的parseInt方法和全局对象上的parseInt方法一样，ES6中之所以这样是为了模块化，下面的parseFloat也是一样

> polyfill

```javascript
Number.parseInt = Number.parseInt || function(value){
    return typeof value === "number" && window.parseInt(value);
}
```

#### Number.parseFloat(value)  
> 将字符串转化为浮点数（ES6）

``` javascript
Number.parseFloat('11.1');       // 11.1
Number.parseFloat(' 11.1aaa');    // 11.1
Number.parseFloat(' 1 1.1aaa');    // 1
Number.parseFloat('aaa11.1');    // NaN
``` 
parseFloat在解析时，会忽略字符串开头的空格，总是从第一个字符开始解析，如果第一个字符是数字或小数点，就继续下一个字符，直到遇到第二个小数点或非数字字符为止。

> polyfill

```javascript
Number.parseFloat = Number.parseFloat || function(value){
    return typeof value === "number" && window.parseFloat(value);
}
```

#### Number.isNaN(value) 
> 判断一个值是否是 NaN（ES6）

```javascript
Number.isNaN(NaN);        // true
Number.isNaN(Number.NaN); // true
Number.isNaN(0 / 0)       // true

Number.isNaN("NaN");      // false，字符串 "NaN" 不会被隐式转换成数字 NaN。
Number.isNaN(undefined);  // false
Number.isNaN({});         // false
Number.isNaN("blabla");   // false

// 下面的都返回 false
Number.isNaN(true);
Number.isNaN(null);
Number.isNaN(37);
Number.isNaN("37");
Number.isNaN("37.37");
Number.isNaN("");
Number.isNaN(" ");
```

> polyfill

```javascript
Number.isNaN = Number.isNaN || function(value){
    return typeof number === "number" && isNaN(value);
}
```

#### Number.isInteger(value) 
> 判断一个值是否为整数（ES6）

```javascript
Number.isInteger(25) // true
Number.isInteger(25.0) // true
Number.isInteger(25.1) // false
Number.isInteger("15") // false
Number.isInteger(true) // false
```

> polyfill

``` javascript
// 判断Number有没有isInteger方法，有的话直接使用Number.isInteger，
// 没有的话创建一个函数
Number.isInteger = Number.isInteger || function(value) {
    // 返回
    // 判断传来的value是否是number类型
    return typeof value === "number" && 
            // 同时value是否是有穷数值
           isFinite(value) && 
           // Math.floor(value)返回小于等于value的最大整数
           // 判断返回小于等于value的最大整数是否等于value
           Math.floor(value) === value;
};
```

####  Number.isFinite(value) 
>判断一个值是否为有穷数（ES6）

```javascript
Number.isFinite(Infinity);  // false
Number.isFinite(NaN);       // false
Number.isFinite(-Infinity); // false

Number.isFinite(0);         // true
Number.isFinite(2e64);      // true

Number.isFinite('0');       // false, 全局函数 isFinite('0') 会返回 true
```

> polyfill

```javascript
//判断Number有没有isFinite，有就直接使用，没有就创建一个函数
Number.isFinite = Number.isFinite || function(value) {
    // 由于全局对象上的isFinite()是可以隐式转换的，
    // 而Number上的isFinite()是不带隐式转换的，
    // 所以首先要判断value是否是number，
    // 然后在使用全局对象上的isFinite()
    return typeof value === "number" && isFinite(value);
}
```

#### Number.isSafeInteger(value) 
> 判断传来的数是否是一个安全整数（ES6）

```javascript
Number.isSafeInteger(3);                    // true
Number.isSafeInteger(Math.pow(2, 53))       // false
Number.isSafeInteger(Math.pow(2, 53) - 1)   // true
Number.isSafeInteger(NaN);                  // false
Number.isSafeInteger(Infinity);             // false
Number.isSafeInteger("3");                  // false
Number.isSafeInteger(3.1);                  // false
Number.isSafeInteger(3.0);                  // true
```
在 js 中，整数和浮点数是同样的储存方法，所以3和3.0被视为同一个值。

####  Number.prototype.toFixed(n) 
> 保留n位小数

```javascript
var numObj = 12345.6789;

numObj.toFixed();         // 返回 "12346"：进行四舍五入，不包括小数部分，相当于传了参数0，保留0位小数
numObj.toFixed(1);        // 返回 "12345.7"：进行四舍五入，保留1位小数

numObj.toFixed(6);        // 返回 "12345.678900"：用0填充，保留6位小数

(1.23e+20).toFixed(2);    // 返回 "123000000000000000000.00"，保留2位小数

(1.23e-10).toFixed(2);    // 返回 "0.00"，保留2位小数

2.34.toFixed(1);          // 返回 "2.3"，保留1位小数

-2.34.toFixed(1);         // 返回 -2.3 （由于操作符优先级，负数不会返回字符串），保留1位小数

(-2.34).toFixed(1);       // 返回 "-2.3" （若用括号提高优先级，则返回字符串），保留1位小数
```
