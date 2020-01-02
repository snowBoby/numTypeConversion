# numTypeConversion
数值类型转换，包含ｊｓ数据类型、强制转换和隐式类型转换。
## 一.ｊｓ内置类型
js中有7种内置类型（数据类型），可以分为两类：原始（基本）类型、引用类型。  　　
* 基础类型(原始值)：`undefined、 null、 string、 number、 boolean、 symbol (es6新出的，本文不讨论这种类型)`-》typeof判断数据类型
* 引用类型(对象值)： `object（子类型：String, Number, Boolean, Date, Array, RegExp, Math, Function，JSON）`-》instanceof判断数据类型  
`注意：null是基本类型中唯一的一个“假值”类型，(!a && typeof a === "object") //用它来判断null值`  
      &nbsp;&nbsp;`typeof function a(){} === "function" 但是function不是内置类型，只是object一个子类型`

## 二.强制转换
&nbsp;&nbsp;&nbsp;&nbsp;强制转换主要指使用Number、String和Boolean三个函数，手动将各种类型的值，分布转换成数值、字符串或者布尔值。 
### 1.1 数值转换
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
  
### 1.2布尔值转换
#### Boolean() 函数的转换规则:

     数据类型    转换为true的值   转换为false的值
     Boolean     true            false
     String      非空字符          空字符
     Number      非0数字          0和NaN
     Object      任何对象          null
     Undefined    无             undefined
### 1.3 字符串强制转换

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
