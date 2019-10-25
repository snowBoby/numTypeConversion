# numTypeConversion
数值类型转换
强制转换：强制转换主要指使用Number、String和Boolean三个函数，手动将各种类型的值，分布转换成数字、字符串或者布尔值。  

一、数值转换：有3个函数可以把非数值转换为数值：Number()、parseInt()、parseFloat()，Number()可以转换任何数据类型，parseInt()和parseFloat()专门用于字符转转化成数值。Number()和parseInt()都可以转化十六进制，parseFloat()只能转换十进制数，所以无第二个参数，parseInt()有第二个参数。  

1、Number() 函数的转换规则:  
  如果 Boolean 值，true 和 false 将分别被转换为 1 和 0。  
  如果是数字值，只是简单的传人和返回。  
  如果是 null 值，返回 0。  
  如果是 undefined，返回 NaN。  
  如果是字符串，遵循下列规则：  
    如果字符串中只包含数字（包括前面带加号或头号的情况），则将其转换为十进制数值，即 “1” 会变成 1，“123” 会变成 123，而 “011” 会变成 11（注意：前     导的零被忽略了）；
    如果字符串中包含有效的浮点格式，如 “1.1”，则将其转换为对应的浮点数值（同样，也会忽略前导零）；
    如果字符串值包含有效的十六进制格式，例如 “0xf”，则将其转换为相同大小的十进制整数值；
    如果字符串是空的（不包含任何字符），则将其转换为 0；
    如果字符串中包含除上述格式之外的字符，则将其转换为 NaN。
  如果是对象，遵循下列规则：
    第一步，调用对象自身的valueOf方法。如果返回原始类型的值，则直接对该值使用Number函数，不再进行后续步骤。
    第二步，如果valueOf方法返回的还是对象，则改为调用对象自身的toString方法。如果toString方法返回原始类型的值，则对该值使用Number函数，不再进行后     续步骤，否则返回NaN。
    
    Number({a: 1}) // NaN
    Number([1, 2, 3]) // NaN
    Number([5]) // 5

2、parseInt()函数的转换规则:  
  更多的是看其是否符合数值模式。它会忽略字符串前面的空格，直至找到第一个非空格字符。如果第一个字符不是数字字符或者负号，parseInt() 函数就会返回 NaN；
  
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
  
3、parseFloat()函数的转换规则:  
  与parseInt()规则类似，但是不能转化十六进制数，只解析十进制数，因此没有第二个参数。
  
二、Boolean() 函数的转换规则:

     数据类型    转换为true的值   转换为false的值
     Boolean     true            false
     String      非空字符          空字符
     Number      非0数字          0和NaN
     Object      任何对象          null
     Undefined    无             undefined
  
三、String() 函数的转换规则:  
    如果值有toString()方法，则调用该方法并返回相应的结果。  
    如果值是null，则返回"null"  
    如果值是undefined，则返回"undefined"  
    
    注意：除了null和undefined，都有toString()方法，toString()可以传递一个参数：输出数值的基数。输出几进制格式的字符串
    var num = 10;
    num.toString()   //"10"
    num.toString(2)   //"1010"
    num.toString(8)   //"12"
    num.toString(10)   //"10"
    num.toString(16)   //"a"
    
