---
title: JavaScript 笔记
date: 2021-02-19 11:30:41
tags:
---
## 对代码行进行折行
您可以在文本字符串中使用反斜杠对代码行进行换行。下面的例子会正确地显示：
```
document.write("你好 \
世界!");
```
## JavaScript数字
极大或极小的数字可以通过科学（指数）计数法来书写：
```
var y=123e5;      // 12300000
var z=123e-5;     // 0.00123
```
## Undefined 和 Null
Undefined 和 Null
Undefined 这个值表示变量不含有值。
可以通过将变量的值设置为 null 来清空变量。
```
var person;
var car="Volvo";
document.write(person + "<br>"); // undefined
document.write(car + "<br>"); // Volvo
var car=null
document.write(car + "<br>"); // null
```
## JavaScript 全局变量
变量在函数外定义，即为全局变量。
如果变量在函数内没有声明（没有使用 var 关键字），该变量为全局变量。
以下实例中 carName 在函数内，但是为全局变量。

```
// 此处可调用 carName 变量
 
function myFunction() {
    carName = "Volvo";
    // 此处可调用 carName 变量
}
```
## undefined 和 null 的区别
null 和 undefined 的值相等，但类型不等：
```
typeof undefined             // undefined
typeof null                  // object
null === undefined           // false
null == undefined            // true
```
## 注意：对象数据类型
NaN 的数据类型是 number
数组(Array)的数据类型是 object
日期(Date)的数据类型为 object
null 的数据类型是 object
未定义变量的数据类型为 undefined
如果对象是 JavaScript Array 或 JavaScript Date ，我们就无法通过 typeof 来判断他们的类型，因为都是 返回 object。
## constructor 属性
constructor 属性返回所有 JavaScript 变量的构造函数。
```
"John".constructor                 // 返回函数 String()  { [native code] }
(3.14).constructor                 // 返回函数 Number()  { [native code] }
false.constructor                  // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor              // 返回函数 Array()   { [native code] }
{name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
new Date().constructor             // 返回函数 Date()    { [native code] }
function () {}.constructor         // 返回函数 Function(){ [native code] }
```
你可以使用 constructor 属性来查看对象是否为数组 (包含字符串 "Array"):
```
function isArray(myArray) {
    return myArray.constructor.toString().indexOf("Array") > -1;
}
```
你可以使用 constructor 属性来查看对象是否为日期 (包含字符串 "Date"):
```
function isDate(myDate) {
    return myDate.constructor.toString().indexOf("Date") > -1;
}
```
## 数字转字符串
1、String(123)
2、Number 方法 toString() 也是有同样的效果。(100 + 23).toString()
3、在 Number 方法 章节中，你可以找到更多数字转换为字符串的方法：
toFixed()	把数字转换为字符串，结果的小数点后有指定位数的数字。
## 字符串转为数字
1、Number("3.14")    // 返回 3.14
2、Number方法中：
parseFloat()	解析一个字符串，并返回一个浮点数。
parseInt()	解析一个字符串，并返回一个整数。
## 将日期转换为数字
1、全局方法 Number() 可将日期转换为数字。
d = new Date();
Number(d)          // 返回 1404568027739
2、日期方法 getTime() 也有相同的效果。
d = new Date();
d.getTime()        // 返回 1404568027739
## 正则表达式
字符串方法
search() 方法 用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，并返回子串的起始位置。
replace() 方法 用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。
```
var str = "Visit Runoob microsoft!"; 
var n = str.search(/Runoob/i);
var txt = str.replace(/microsoft/i,"Runoob"); // Visit Runoob Runoob!
```
使用 RegExp 对象
在 JavaScript 中，RegExp 对象是一个预定义了属性和方法的正则表达式对象。
test() 方法用于检测一个字符串是否匹配某个模式，如果字符串中含有匹配的文本，则返回 true，否则返回 false
```
var patt = /e/;
patt.test("The best things in life are free!"); // true
```
exec() 方法用于检索字符串中的正则表达式的匹配。
该函数返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。
```
/e/.exec("The best things in life are free!"); // e
```
