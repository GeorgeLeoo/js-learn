# 数值Number

js 中的数值可以表示整数也可以表示小数。js 采用的是 IEEE 745 标准定义的 64 位浮点格式表示数字，由于使用该标准的问题，导致 0.1 + 0.2 不等于 0.3。

### Number()函数
Number() 函数可以将对象的值转换为数字

> 语法 

``` javascript
Number(object)
```
其中 object 是必选参数。

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

Number 方法

> parseInt()    将字符串转化为整数

``` javascript
Number.parseInt('11');       // 11
Number.parseInt(' 11aaa');    // 11
Number.parseInt(' 1 1aaa');    // 1
Number.parseInt('aaa11');    // NaN
``` 
parseInt 在解析时，会忽略字符串开头的空格，总是从第一个字符开始解析，如果第一个字符是数字，就继续下一个字符，直到不是数字字符为止

> parseFloat(string)  将字符串转化为浮点数

``` javascript
Number.parseFloat('11.1');       // 11.1
Number.parseFloat(' 11.1aaa');    // 11.1
Number.parseFloat(' 1 1.1aaa');    // 1
Number.parseFloat('aaa11.1');    // NaN
``` 
parseFloat在解析时，会忽略字符串开头的空格，总是从第一个字符开始解析，如果第一个字符是数字或小数点，就继续下一个字符，直到遇到第二个小数点或非数字字符为止。

> isNaN() 判断一个值是否是 NaN

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

> isInteger() 判断一个值是否为整数

```javascript
Number.isInteger(25) // true
Number.isInteger(25.0) // true
Number.isInteger(25.1) // false
Number.isInteger("15") // false
Number.isInteger(true) // false
```
在 js 中，整数和浮点数是同样的储存方法，所以3和3.0被视为同一个值。


