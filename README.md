# numTypeConversion
数值类型转换，包含ｊｓ数据类型、强制转换和隐式类型转换。
## 一. ｊｓ内置类型
js中有7种内置类型（数据类型），可以分为两类：**原始（基本）类型和引用类型：** 　　
* 基础类型(原始值)：`undefined、 null、 string、 number、 boolean、 symbol (es6新出的，本文不讨论这种类型)`-》typeof判断数据类型
* 引用类型(对象值)： `object（子类型：String, Number, Boolean, Array, Object, Function，Symbol, RegExp, Date, Error, Math, JSON）`-》instanceof判断数据类型   
` 注意：简单标量基本类型值通过值复制来赋值/传递，而复合值（对象等）通过引用复制来赋值/传递。JavaScript中的引用和其他语言中的引用/指针不同，它们不能指向别的变量/引用，只能指向值。如果通过值复制的方式来传递复合值（如数组），就需要为其创建一个复本(eg: foo(a.slice()) )，这样传递的就不再是原始值。 `

**typeof注意事项：**
* JS中的变量是没有类型的，只有值才有类型。所以对变量执行typeof操作时，得到的结果并不是该变量的类型，而是该变量持有值的类型。
* typeof null === "object"，null是基本类型中唯一的一个“假值”类型，(!a && typeof a === "object") //用它来判断null值
* typeof function a(){} === "function" 但是function不是内置类型，只是object一个子类型
* typeof Undeclared/Undefined === 'undefined' (undefined是值的一种，undeclared则表示变量还没有被声明过)，变量有没有被声明，都没有报错，这是因为typeof有一个特殊的安全防范机制(全局对象也可以用window.__也是安全防范机制)。

### 1.1  数组
* 数组：数组通过数字进行索引，但是数组也是对象，所以也可以包含字符串键值和属性（但这些并不计算在数组长度内），除非字符串键值能够被强制类型转换为十进制数字的话，它就会被当作数字索引来处理。
* 类数组：包括 DOM查询操作返回的DOM元素列表、arguments

| 操作 | 语法 |
| --- | --- |
| 类数组转换成数组 | 1、var arr = Array.prototype.slice.call( arguments ); //slice(..)不带参数会返回当前数组的一个浅复本  2、Array.from(arguments)|
| 字符串转换成数组 | 1、"foo".split('').reverse().join('')对于包含复杂字符(Unicode，如星号、多字节字符)的字符串并不适用，这时则需要功能更加完备、能够处理Unicode的工具库，可参考Esrever |
| length = 0 | 清空数组 |
   
### 1.2  字符串
* 字符串和数组的确很相似，他们都是类数组，都有length属性以及indexOf(..)和concat(..)方法。
* JavaScript中字符串是不可变的，而数组是可变的。字符串不可变是指字符串的成员函数不会改变其原始值，而是创建并返回一个新的字符串。而数组的成员函数都是在其原始值上进行操作。例子如下
* 许多数组函数来处理字符串很方便。虽然字符串没有这些函数，但是可以通过“借用”数组（Array.prototype.xxx.call）的非变更方法(操作数组原始值的成员函数eg：reverse() )来处理字符串。例子如下
```
var a = "foo"; 
var b = ["f","o","o"];

a[1] = "O"; //a[1]在JavaScript中并非总是合法语法，在老版本的IE中就不被允许（现在可以了）。正确的方法应该是a.charAt(1)。
b[1] = "O"; 

a; // "foo" 
b; // ["f","O","o"]

//字符串不可变，数组可变，体现在如下：
c = a.toUpperCase(); 
a === c;    // false 
a;          // "foo" 
c;          // "FOO" 

b.push( "!" ); 
b;          // ["f","O","o","!"]

//可以通过“借用”数组的非变更方法来处理字符串
a.join;         // undefined 
a.map;          // undefined 

var c = Array.prototype.join.call( a, "-" ); 
var d = Array.prototype.map.call( a, function(v){     return v.toUpperCase() + "."; } ).join( "" ); 

c;              // "f-o-o" 
d;              // "F.O.O."

//修改原始值的数组成员函数，字符串不能“借用”
a.reverse;      // undefined 
b.reverse();    // ["!","o","O","f"] 
b;              // ["f","O","o","!"]
Array.prototype.reverse.call( a ); //报错，我们无法“借用”数组的可变更成员函数，因为字符串是不可变的。

*注意：如果需要经常以字符数组的方式来处理字符串的话，倒不如直接使用数组。这样就不用在字符串和数组之间来回折腾。可以在需要时使用join("")将字符数组转换为字符串
```
### 1.3  数字
Js适用的是“双精度”格式（即64位二进制），` a  |  0可以将变量a中的数值转换为32位有符号整数，因为数位运算符|只适用于32位整数（它只关心32位以内的值，其他的数位将被忽略）。因此与0进行操作即可截取a中的32位数位。`  
* 0x/0X表示十六进制数，0o/0O表示八进制数，0b/0B表示二进制数（尽量使用小写）  
* void __ 返回undefined    
* 对负零进行字符串化会返回"0"，反过来将其从字符串转换为数字，得到的结果是准确的

| Number类型成员函数 | 描述 | 例子针对var a = 42.59; |
| --- | --- | --- |
| toFixed( 位数 ) | 指定小数保留几位数 | a.toFixed( 0 ); // "43" |
| toExponential() | 转换成指数形式 | a.toExponential() //"4.259e+1" |
| toPrecision(位数) | 指定有效数位的显示位数 |a.toPrecision( 1 ); // "4e+1"  a.toPrecision( 3 ); // "42.6" |

| Number类属性 | 描述 |
| --- | --- |
| Number.EPSILON | 机器精度，通常是2^-52 |
| Number.MAX_VALUE | 最大浮点数，大约是1.798e+308 |
| Number.MIN_VALUE | 最小浮点数，大约是5e-324，它不是负数，但无限接近于0 |
| Number.MAX_SAFE_INTEGER | “安全”呈现的最大整数是2^53-1，即9007199254740991 |
| Number.MIN_SAFE_INTEGER | 最小整数是-9007199254740991 |
| Number.NEGATIVE_INfiNITY | -Infinity，JavaScript的运算结果有可能溢出，此时结果为Infinity或者-Infinity |
| Number.isInteger(..) | 检测一个值是否是整数 |
| Number.isSafeInteger(..) | 检测一个值是否是安全的整数 |
| Number.isNaN(..) | 检测是一个数值并且是否为NaN，由于全局isNaN()有缺陷，不能检测是否为一个数值，由此es6引出这个 |
```

//二进制浮点数会出现问题 
0.1 + 0.2 === 0.3; // false 简单来说，二进制浮点数中的0.1和0.2并不是十分精确，所以在处理带有小数的数字时需要特别注意。

//那么应该怎样来判断0.1 + 0.2和0.3是否相等呢？
//最常见的方法是设置一个误差范围值，通常称为“机器精度”，对JavaScript的数字来说，这个值通常是2^-52 (2.220446049250313e-16)，es6这个值存在于Number.EPSILON中。
if (!Number.EPSILON) {                     
   Number.EPSILON = Math.pow(2,-52); 
}
function numbersCloseEnoughToEqual(n1,n2) {     
   return Math.abs( n1 - n2 ) < Number.EPSILON; 
} 
var a = 0.1 + 0.2; 
var b = 0.3; 
numbersCloseEnoughToEqual( a, b );                  // true 
numbersCloseEnoughToEqual( 0.0000001, 0.0000002 );  // false

//ES6之前的浏览器isNaN的polyfill，两种方式：
if (!Number.isNaN) {     
   Number.isNaN = function(n) {         
   return (             
      typeof n === "number" &&             
      window.isNaN( n )        
      );      
   }; 
}
if (!Number.isNaN) {     
   Number.isNaN = function(n) {         
   return n !== n;     //NaN是JavaScript中唯一一个不等于自身的值
   }; 
}

//JavaScript的运算结果有可能溢出，此时结果为Infinity或者-Infinity。规范规定，如果数学运算（如加法）的结果超出处理范围，则由IEEE 754规范中的“就近取整”模式来决定最后的结果。例如，相对于Infinity，Number.MAX_VALUE  +  Math.pow(2,  969)与Number.MAX_VALUE更为接近，因此它被“向下取整”；而Number.MAX_VALUE + Math.pow(2, 970)与Infinity更为接近，所以它被“向上取整”
//计算结果一旦溢出为无穷数（infinity）就无法再得到有穷数。
var a = Number.MAX_VALUE;   // 1.7976931348623157e+308 
a + a;                      // Infinity 
a + Math.pow( 2, 970 );     // Infinity 
a + Math.pow( 2, 969 );     // 1.7976931348623157e+308

//解决-0 === 0 的问题：或者通过Object.is(a,b)，能使用==和===时就尽量不要使用Object.is(..)，因为前者效率更高、更为通用。Object.is(..)主要用来处理那些特殊的相等比较。
function isNegZero(n) {     
   n = Number( n );     
   return (n === 0) && (1 / n === -Infinity); 
}
if (!Object.is) {     
   Object.is = function(v1, v2) {         
      // 判断是否是-0         
      if (v1 === 0 && v2 === 0) {             
         return 1 / v1 === 1 / v2; 
      }         
      // 判断是否是NaN         
      if (v1 !== v1) {             
         return v2 !== v2;         
      }         
      // 其他情况        
      return v1 === v2;     
   }; 
}
```
## 二.强制转换
&nbsp;&nbsp;&nbsp;&nbsp;强制转换主要指使用Number、String和Boolean三个函数，手动将各种类型的值，分布转换成数值、字符串或者布尔值。 
### 2.1 数值转换
&nbsp;&nbsp;&nbsp;&nbsp;有3个函数可以把非数值转换为数值：Number()、parseInt()、parseFloat()，Number()可以转换任何数据类型，parseInt()和parseFloat()专门用于字符串转化成数值。Number()和parseInt()都可以转化十六进制，parseFloat()只能转换十进制数，所以无第二个参数，parseInt()有第二个参数。  

#### Number() 函数的转换规则:  

* 如果 Boolean 值，true 和 false 将分别被转换为 1和0。  
* 如果是数字值，只是简单的传人和返回。  
* 如果是 null 值，返回 0。  
* 如果是 undefined，返回 NaN。  
* 如果是字符串，遵循下列规则：  
    * 如果字符串中只包含数字（包括前面带加号或头号的情况），则将其转换为十进制数值，即 “1” 会变成 1，“123” 会变成 123，而 “011” 会变成 11（注意：前     导的零被忽略了）；
    * 如果字符串中包含有效的浮点格式，如 “1.1”，则将其转换为对应的浮点数值（同样，也会忽略前导零）；
    * 如果字符串值包含有效的十六进制格式，例如 “0xf”，则将其转换为相同大小的十进制整数值；
    * 如果字符串是空的（不包含任何字符），则将其转换为 0；
    * 如果字符串中包含除上述格式之外的字符，则将其转换为 NaN。
* 如果是对象，遵循下列规则：

    * 第一步，调用对象自身的valueOf方法。如果返回原始类型的值，则直接对该值使用Number函数，不再进行后续步骤。
    * 第二步，如果valueOf方法返回的还是对象，则改为调用对象自身的toString方法。如果toString方法返回原始类型的值，则对该值使用Number函数，不再进行后续步骤，否则返回NaN。


```
    Number({a: 1}) // NaN
    Number([1, 2, 3]) // NaN
    Number([5]) // 5
```

#### parseInt()函数的转换规则:  

&nbsp;&nbsp;&nbsp;&nbsp;更多的是看其是否符合数值模式。它会忽略字符串前面的空格，直至找到第一个非空格字符。如果第一个字符不是数字字符或者负号，parseInt() 函数就会返回 NaN；
  
    parseInt("1234blue") //1234
    parseInt("")         //NaN
    parseInt("0xA")      //10(十六进制数)
    parseInt("070")      //70(十进制数)
    parseInt("22.5")     //22
    parseInt("0xAF",16)  //175(按十六进制进行解析)
    parseInt("AF",16)    //175(按十六进制进行解析)
    parseInt("AF")       //NaN
    parseInt("10",2)     //2(按二进制进行解析)
    parseInt("10",8)     //8(按八进制进行解析)
    parseInt("10",10)    //10(按十进制进行解析)
    parseInt("10",16)    //16(按十六进制进行解析)
  

#### parseFloat()函数的转换规则:  

&nbsp;&nbsp;&nbsp;&nbsp;与parseInt()规则类似，但是不能转化十六进制数，只解析十进制数，因此没有第二个参数。
  
### 2.2布尔值转换
#### Boolean() 函数的转换规则:

     数据类型    转换为true的值   转换为false的值
     Boolean     true            false
     String      非空字符          空字符
     Number      非0数字          0和NaN
     Object      任何对象          null
     Undefined    无             undefined
### 2.3 字符串强制转换

#### String() 函数的转换规则:  
*     如果值有toString()方法，则调用该方法并返回相应的结果。  
*     如果值是null，则返回"null"  
*     如果值是undefined，则返回"undefined"  
`注意：除了null和undefined，都有toString()方法，toString()可以传递一个参数：输出数值的基数。输出几进制格式的字符串`

    
```

    var num = 10;
    num.toString()   //"10"
    num.toString(2)   //"1010"
    num.toString(8)   //"12"
    num.toString(10)   //"10"
    num.toString(16)   //"a"
```
    
## 三.隐式类型转换
[参考](https://juejin.im/post/5a7172d9f265da3e3245cbca)
```
const a = {
  i: 1,
  toString: function () {
    return a.i++;
  }
}
if (a == 1 && a == 2 && a == 3) {
  console.log('hello world!');
}
```
隐式转换中主要涉及到三种转换：

1、将值转为原始值，ToPrimitive()。

2、将值转为数字，ToNumber()。

3、将值转为字符串，ToString()。

隐式转换的场景[参考](https://juejin.im/post/5a558d0051882573291450b6)：

1、转换为Number。++, --，
   * +, - 运算符
   * ++, -- 运算符
   * +, -, *, /, % 计算
   需要注意的是，当 + 计算有字符串参与计算时，会转换为字符串。
   * &lt;, &gt;, >=, <= 比较运算符， 操作数都不是String类型时
   * ==, != 操作数中只有String类型和Number类型，或者其中有一个是Boolean
2、转换为Boolean。
   * && ! || 运算
   * 条件运算
   * if, while, do-while, for
3、转换为String
   * + 号运算，其中一个操作符为字符串
   * 比较运算，其中一个操作符为字符串
### 3.1、通过ToPrimitive将值转换为原始值
js引擎内部的抽象操作ToPrimitive(input,PreferredType?)input是要转换的值，PreferredType是可选参数，可以是Number或String类型。他只是一个转换标志，**转换结果一定是一个原始值（或者报错）**。既然PreferredType是可选参数，那么如果没有这个参数时，PreferredType的值会按照这样的规则来自动设置：**该对象为Date类型，则PreferredType被设置为String，否则，PreferredType被设置为Number**
#### 3.1.1、如果PreferredType被标记为Number，则会进行下面的操作流程来转换输入的值。
```
1、如果输入的值已经是一个原始值，则直接返回它
2、否则，如果输入的值是一个对象，则调用该对象的valueOf()方法，如果valueOf()方法的返回值是一个原始值，则返回这个原始值。
3、否则，调用这个对象的toString()方法，如果toString()方法返回的是一个原始值，则返回这个原始值。
4、否则，抛出TypeError异常。
```
#### 3.1.2、如果PreferredType被标记为String，则会进行下面的操作流程来转换输入的值。
```
1、如果输入的值已经是一个原始值，则直接返回它
2、否则，调用这个对象的toString()方法，如果toString()方法返回的是一个原始值，则返回这个原始值。
3、否则，如果输入的值是一个对象，则调用该对象的valueOf()方法，如果valueOf()方法的返回值是一个原始值，则返回这个原始值。
4、否则，抛出TypeError异常。
```
#### 3.1.3、valueOf方法和toString方法解析
对象一定有valueOf和toString两个方法。在控制台输出Object.prototype，你会发现其中就有valueOf和toString方法，而Object.prototype是所有对象原型链顶层原型，所有对象都会继承该原型的方法，故任何对象都会有valueOf和toString方法

1. 先看看对象的valueOf函数，其转换结果是什么？对于js的常见内置对象：Date, Array, Math, Number, Boolean, String, Array, RegExp, Function
    * Number、Boolean、String这三种构造函数生成的基础值的对象形式，通过valueOf转换后会变成相应的原始值。
    * Date这种特殊的对象，其原型Date.prototype上内置的valueOf函数将日期转换为日期的毫秒的形式的数值。
    * 除此之外返回的都为this，即对象本身。

    ```
    var num = new Number('123');
    num.valueOf(); // 123

    var str = new String('12df');
    str.valueOf(); // '12df'

    var bool = new Boolean('fd');
    bool.valueOf(); // true
    
    //针对第二条
    var a = new Date();
    a.valueOf(); // 1515143895500
    
    //以下针对第三条
    var a = new Array();
    a.valueOf() === a; // true

    var b = new Object({});
    b.valueOf() === b; // true
    ```
2.再来看看toString函数，其转换结果是什么？对于js的常见内置对象：Date, Array, Math, Number, Boolean, String, Array, RegExp, Function。

* Number、Boolean、String、Array、Date、RegExp、Function这几种构造函数生成的对象，通过toString转换后会变成相应的字符串的形式，因为这些构造函数上封装了自己的toString方法
* 除这些对象及其实例化对象之外，其他对象返回的都是该对象的类型，(有问题欢迎告知)，都是继承的Object.prototype.toString方法。

```
Number.prototype.hasOwnProperty('toString'); // true
Boolean.prototype.hasOwnProperty('toString'); // true
String.prototype.hasOwnProperty('toString'); // true
Array.prototype.hasOwnProperty('toString'); // true
Date.prototype.hasOwnProperty('toString'); // true
RegExp.prototype.hasOwnProperty('toString'); // true
Function.prototype.hasOwnProperty('toString'); // true

var num = new Number('123sd');
num.toString(); // 'NaN'

var str = new String('12df');
str.toString(); // '12df'

var bool = new Boolean('fd');
bool.toString(); // 'true'

var arr = new Array(1,2);
arr.toString(); // '1,2'

var d = new Date();
d.toString(); // "Wed Oct 11 2017 08:00:00 GMT+0800 (中国标准时间)"

var func = function () {}
func.toString(); // "function () {}"

//以下针对第二条
var obj = new Object({});
obj.toString(); // "[object Object]"

Math.toString(); // "[object Math]"
```

### 3.2、通过ToNumber将值转换为数字
根据参数类型进行下面转换：
| 参数 |结果  |
| --- | --- |
| undefined | NaN |
| null | +0 |
| 布尔值 | true转换1，false转换为+0 |
| 数字 | 无须转换 |
| 字符串 | 有字符串解析为数字，例如：‘324’转换为324，‘qwer’转换为NaN |
| 对象(obj) | 先进行 ToPrimitive(obj, Number)转换得到原始值，在进行ToNumber转换为数字 |

### 3.3、通过ToString将值转换为字符串
根据参数类型进行下面转换：

| 参数 |结果  |
| --- | --- |
| undefined | 'undefined' |
| null | 'null' |
| 布尔值 | 转换为'true' 或 'false' |
| 数字 | 数字转换字符串，比如：1.765转为'1.765' |
| 字符串 | 无须转换 |
| 对象(obj) | 先进行 ToPrimitive(obj, String)转换得到原始值，在进行ToString转换为字符串 |

```
例子：
({} + {}) = ?
两个对象的值进行+运算符，肯定要先进行隐式转换为原始类型才能进行计算。
1、进行ToPrimitive转换，由于没有指定PreferredType类型，{}会使默认值为Number，进行ToPrimitive(input, Number)运算。
2、所以会执行valueOf方法，({}).valueOf(),返回的还是{}对象，不是原始值。
3、继续执行toString方法，({}).toString(),返回"[object Object]"，是原始值。
故得到最终的结果，"[object Object]" + "[object Object]" = "[object Object][object Object]"
```
```
2 * {} = ?
1、首先*运算符只能对number类型进行运算，故第一步就是对{}进行ToNumber类型转换。
2、由于{}是对象类型，故先进行原始类型转换，ToPrimitive(input, Number)运算。
3、所以会执行valueOf方法，({}).valueOf(),返回的还是{}对象，不是原始值。
4、继续执行toString方法，({}).toString(),返回"[object Object]"，是原始值。
5、转换为原始值后再进行ToNumber运算，"[object Object]"就转换为NaN。
故最终的结果为 2 * NaN = NaN
```
### 3.4、== 运算符隐式转换
```
比较运算 x==y, 其中 x 和 y 是值，返回 true 或者 false。这样的比较按如下方式进行：

1、若 Type(x) 与 Type(y) 相同， 则

    1* 若 Type(x) 为 Undefined， 返回 true。
    2* 若 Type(x) 为 Null， 返回 true。
    3* 若 Type(x) 为 Number， 则
  
        (1)、若 x 为 NaN， 返回 false。
        (2)、若 y 为 NaN， 返回 false。
        (3)、若 x 与 y 为相等数值， 返回 true。
        (4)、若 x 为 +0 且 y 为 −0， 返回 true。
        (5)、若 x 为 −0 且 y 为 +0， 返回 true。
        (6)、返回 false。
        
    4* 若 Type(x) 为 String, 则当 x 和 y 为完全相同的字符序列（长度相等且相同字符在相同位置）时返回 true。 否则， 返回 false。
    5* 若 Type(x) 为 Boolean, 当 x 和 y 为同为 true 或者同为 false 时返回 true。 否则， 返回 false。
    6*  当 x 和 y 为引用同一对象时返回 true。否则，返回 false。
  
2、若 x 为 null 且 y 为 undefined， 返回 true。
3、若 x 为 undefined 且 y 为 null， 返回 true。
4、若 Type(x) 为 Number 且 Type(y) 为 String，返回比较 x == ToNumber(y) 的结果。
5、若 Type(x) 为 String 且 Type(y) 为 Number，返回比较 ToNumber(x) == y 的结果。
6、若 Type(x) 为 Boolean， 返回比较 ToNumber(x) == y 的结果。
7、若 Type(y) 为 Boolean， 返回比较 x == ToNumber(y) 的结果。
8、若 Type(x) 为 String 或 Number，且 Type(y) 为 Object，返回比较 x == ToPrimitive(y) 的结果。
9、若 Type(x) 为 Object 且 Type(y) 为 String 或 Number， 返回比较 ToPrimitive(x) == y 的结果。
10、返回 false。
```
```
var a = {
  valueOf: function () {
     return 1;
  },
  toString: function () {
     return '123'
  }
}
true == a // true;
首先，x与y类型不同，x为boolean类型，则进行ToNumber转换为1,为number类型。
接着，x为number，y为object类型，对y进行原始转换，ToPrimitive(a, ?),没有指定转换类型，默认number类型。
而后，ToPrimitive(a, Number)首先调用valueOf方法，返回1，得到原始类型1。
最后 1 == 1， 返回true。
```
```
[] == !{}
1、! 运算符优先级高于==，故先进行！运算。
2、!{}运算结果为false，结果变成 [] == false比较。
3、根据上面第7条，等式右边y = ToNumber(false) = 0。结果变成 [] == 0。
4、按照上面第9条，比较变成ToPrimitive([]) == 0。
    按照上面规则进行原始值转换，[]会先调用valueOf函数，返回this。
   不是原始值，继续调用toString方法，x = [].toString() = ''。
   故结果为 '' == 0比较。
5、根据上面第5条，等式左边x = ToNumber('') = 0。
   所以结果变为： 0 == 0，返回true，比较结束。
```
