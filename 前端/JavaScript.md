# JavaScript

JavaScript 作者只用了10天就开出来

## 行为层（JavaScript）

JavaScript是一门弱类型的脚本语言，其源代码在发往客户端之前不需要经过编译，而是将文本格式的字符代码发送给浏览器，由浏览器解释运行

**Native 原生 JS 开发**

原生 JS 开发，也就是让我们按照【ECMAScript】标准开发方式，简称 ES ，特点是所有浏览器都支持。

**TypeScript 微软标准**

TypeScript 是一种由微软开发和开源的编程语言，他是JavaScript 的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。



## UI框架

- Ant-Design：阿里出品，基于React的UI框架
- ElementUI. iview、ice：饿了么出品，基于Vue的UI框架
- BootStrap：Twitter推出的一个用于前端开发的开源工具包
- AmazeUI：又叫”妹子UI“，一款HTML5跨屏的前端框架



## JavaScript 构建工具

- Babel：JS编译工具，主要用于浏览器不支持的ES新特性
- WebPack：模块打包器，主要作用是打包、压缩、合并和按序加载



## 1、什么是JavaScript

### 1.1、概述

JavaScript是一门世界上最流行的脚本语言

一个合格的后端人员，必须要精通javaScript



### 1.2、历史

**ECMAScript**可以理解为是JavaScript的一个标准。

最新版本已经到了ES6版本

但是大部分浏览器还只停留在支持ES5代码上

开发环境--先上环境，版本不一致



## 2、快速入门

### 2.1、引入JavaScript

**script标签内不用显示定义type，也默认就是JavaScript**

#### 2.1.1、内部标签

```html
<script>
	alert('hello world');
</script>	
```

#### 2.1.2、外部引入

abs.js

```javascript
alert('hello world');
```

test.html

```html
 <!--注意script必须成对出现-->
<script src="abc.js"></script>
```

测试代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <!--script标签内写JavaScript代码-->
    <!--<script>-->
    <!--    alert('hello world');-->
    <!--</script>-->

    <!--外部引入-->
    <!--注意script必须成对出现，如果<script>标签中使用src属性，那么该标签中封装的  		javascript代码不会被执行。-->
    <script src="js/qj.js"></script>

    <!--不用显示定义type，也默认就是JavaScript-->
    <script type="javascript">

    </script>

</head>
<body>

<!--这里也可以存放-->
</body>
</html>
```



### 2.2、基本语法入门

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <!--javaScript严格区分大小写-->
    <script>
        // 1.定义变量
        var score=70;

        // 2.条件控制
        if (score>60&&score<70){
            alert("60~70");
        }else if(score>70&&score<80){
            alert("70~80")
        }else{
            alert("other");
        }

        //console.log(score) 在浏览器的控制台打印变量，相当于System.out.println()
        /*
        * dasdasda
        */
    </script>
    
</head>
<body>

</body>
</html>
```

浏览器必备调试须知：

![image-20200607174918253](https://gitee.com/xudongyin/img/raw/master/img/20200719183703.png)

![image-20200628235243288](https://gitee.com/xudongyin/img/raw/master/img/20200719183657.png)

### 2.3、数据类型

数值、文本、图形、音频、视频...

> 变量

```javascript
var 变量名='变量值';
```

不能以数字开头！

> number

js不区分小数跟整数，统一用number

```javascript
123		//整数
123.1	//浮点数
1.23e3	//科学技术法
-99		//负数
NaN		//not a number
Infinity	//表示无限大
```

> 字符串

```javascript
'abc'
"abc"
```

> 布尔值

```javascript
true
false
```

> 算术运算符

~~~javas
+ 、- 、* 、/ 、% 、++ 、--
~~~

> 赋值运算符

~~~JavaScript
= 、 += 、-= 、 *= 、 /= 、 %= 
~~~

> 逻辑运算符

```javascript
&&	//两个都为真，结果为真
||	//一个为真，结果为真
!	//真即假，假即真
```

> 比较运算符

```javascript
> 、<、 >=、 <=、 !=
=	//赋值
==	//等于（类型不一样，值一样，也会判断为true）
===	 //绝对等于（类型一样，值一样，结果为true）   
```

**这是一个js缺陷，坚持不要使用==**

须知：

- **NaN===NaN,这个与所有的数值都不相等，包括自己**
- **只能通过isNaN(NaN)来判断这个数值是否为NaN**

> 位运算符

~~~javascript
& 、 | 、 ^ 、 >> 、 <<  、 >>>
 /*
    左移几位就是该数据乘以2的几次方（左移运算符<<）（右移相反>>:除以，高位空缺的位高位是什么就补什么）；
无符号右移>>>：数据进行右移时，高位出现空缺，无论高位是什么都补0。
 */
~~~

> 三元运算符

~~~JavaScript
   ? :
~~~

> 浮点数问题：

```javascript
console.log((1/3)===(1-2/3));
//输出 false
```

尽量避免使用浮点数进行计算，存在精度问题

```javascript
console.log(Math.abs(1/3-(1-2/3))<0.000000001);
//输出 true
```

> null和undefined

- null 空
- undefined 未定义，其实它就是一个常量。

~~~JavaScript
var xx;
alert(xx);  //  undefined
alert(xx==undefined);  //true
~~~

> 其他一些注意点

~~~JavaScript
alert("12"-1);//11
alert("12"+1);//121
alert(true+1);//2 ，因为在js中false就是0，或者null，非0、非null就是true。默认用1表示。
alert("a="+a/1000*1000);//a=3710;进行除运算不会进行取整
~~~

> 数组

java的数组必须是一系列相同类型的对象，js中不需要这样

```JavaScript
 //保证代码的可读性，尽量使用[]
var arr = [1, 2, 3, 4, 5, "hello", null, true];

new Array(1, 2, 3, 4, 5, "hello");
```

取数组下标，如果越界了，就会undefined

> 对象

**对象是{}，数组是[]**

每个属性之间使用逗号隔开，最后一个不需要

```JavaScript
var person={
    name:'lisi',
    age:3,
    tags:['js','java','web']
}
```

取对象的值

![image-20200607211950848](https://gitee.com/xudongyin/img/raw/master/img/20200719183715.png)



### 2.4、严格检查模式

![image-20200607214007334](https://gitee.com/xudongyin/img/raw/master/img/20200719183736.png)

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <!--前提：IDEA需要设置支持使用ES6语法
        'use strict';严格检查模式，预防JavaScript的随意性导致产生的一些问题，必须写在JavaScript的第一行
        局部变量建议使用let定义
    -->
    <script>
        //严格检查模式
        'use strict';
        //全局变量
        var i=1;
        //ES6 中局部变量使用let定义
        let j=1;
    </script>
</head>
<body>

</body>
</html>
```



## 3、数据类型

### 3.1、字符串

> 1、正常字符串使用单引号或者双引号包裹

> 2、注意转义字符 \

```
\'
\n
\t
\u4e2d	\u####  Unicode字符
\x41	Ascll字符
```

> 3、多行字符串编写

```javascript
//tab 上面的那个piao~
var msg=`
    helloworld
    你好
    你好呀
`;
console.log(msg);
```

> 4、模板字符串

```JavaScript
var name="lisi";
var age=3;
var msg=`你好，${name}`;
console.log(msg);
```

> 5、字符串长度

```javascript
str.length
```

> 6、字符串的可变性,不可变

![image-20200608115349387](https://gitee.com/xudongyin/img/raw/master/img/20200719183751.png)

> 7、大小写转换

```javascript
//转大写，注意这里是方法，不是属性
str.toUpperCase();

//转小写
str.toLowerCase();
```

> 8、输出指定字符的位置

```javascript
str.indexOf("t");
```

> 9、substring从第x个字符截取到第y个字符，包含第一个，不包含最后一个

```javascript
str.substring(x,y);
```



### 3.2、数组

**Array可以包含任意类型的数据**

```javascript
var arr=[1,2,3,4,5,6];//通过下标取值和赋值
arr[0];
arr[0]=1;
```

> 1、长度

```javascript
arr.length;
```

注意：**假如给arr.length赋值，数组大小就会发生变化，如果赋值过小，元素就会丢失**

> 2、indexOf，通过元素获得下表索引

```javascript
arr.indexOf(2);
```

字符串的1和数字1是不同的

> 3、slice()	截取Array的一部分，返回一个新的数组，类似于String中的subString

> 4、push()、pop()

```javascript
push:压入到尾部
pop:弹出尾部的一个元素
```

> 5、unshift()，shift()

```javascript
unshift:压入到头部
shift:弹出头部的一个元素
```

> 6、排序用sort()

![image-20200608131540848](https://gitee.com/xudongyin/img/raw/master/img/20200719183803.png)

> 7、元素反转 reverse()

![image-20200608131651751](https://gitee.com/xudongyin/img/raw/master/img/20200719183850.png)

> 8、concat()

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719183845.png)

注意：**concat()并没有修改数组，只是返回了一个新的数组**

> 9、连接符 join

打印拼接数组，使用特定的字符串连接

```javascript
>> arr.join("-");
<- "c-b-a"
```

> 10、多维数组

```javascript
>> arr=[[1,2],[3,4],[5,6]];
>> arr[1][1]
<- 4
```

数组：存储数据（如何存，如何取，方法都可以自己实现！）

js中的数组定义的两种方式：

~~~JavaScript
// 1、直接定义
var arr = []; 
var arr = [3,1,5,8];
// 2、使用了javascript中的Array对象来完成的定义。 
var arr = new Array(); //var arr = [];
var arr1 = new Array(5); //数组定义并长度是5.
var arr2 = new Array(5,6,7); //定义一个数组，元素是5,6,7 

alert(typeof(arr));//对象类型是Object
~~~



### 3.3、对象

若干个键值对

```JavaScript
var 对象名 = {
    属性名 : 属性值,
    	.... ,
    属性名 : 属性值
};

 //定义了一个person对象，他有四个属性
var person = {
    name: 'lisi',
    age: 3,
    email: '123456@qq.com',
    score: 0
};
```

js中的对象，{...}表示一个对象，键值对描述属性xxx:xxx，多个属性之间使用逗号隔开，最后一个属性不加逗号。

**JavaScript中所有的键都是字符串，值是任意对象！**

> 1、对象赋值问题

```javascript
>> person.name='wangwu';
<- "wangwu"
>> person
<- Object { name: "wangwu", age: 3, email: "123456@qq.com", score: 0 }
```

> 2、使用一个不存在的对象属性不会报错  undefined

```javascript
>> person.haha
<- undefined
```

> 3、动态的删减属性，通过delete来删除对象的属性

```javascript
>> delete person.name
<- true
>> person
<- Object { age: 3, email: "123456@qq.com", score: 0 }
```

> 4、动态的添加属性，直接给新的属性添加值即可

```javascript
>> person.haha='hahah'
<- "hahah"
>> person
<- Object { age: 3, email: "123456@qq.com", score: 0, haha: "hahah" }
```

> 5、判断属性值是否在这个对象中，xxx in xxx

```javascript
>> 'age' in person
<- true
//继承
>> 'toString' in person
<- true
```

> 6、判断一个属性是否是自身拥有的	hasOwnProperty

```javascript
>> person.hasOwnProperty('toString');
<- false
>> person.hasOwnProperty('age');
<- true
```



### 3.4、流程控制

> if判断

```javascript
let age=3;
if(age<=3){
    alert('haha');
}else if(age>3&&age<5){
    alert('kua');
}else{
    alert('lili');
}
```

> 循环

while循环，尽量避免程序死循环

```javascript
while(age<100){
    age=age+1;
    console.log(age);
}

do{
    age=age+1;
    console.log(age);
}while(age<100)
```

for循环

```JavaScript
var age=[12,35,26,45,38];

for (let i=0;i<age.length;i++){
    console.log(age[i]);
}

for (let number of age) {
    console.log(number);
}
```

forEach循环

> ES5.1引入的

```JavaScript
var age=[12,35,26,45,38];
//函数
age.forEach(function (value) {
    console.log(value)
})
```

for...in

```javascript
var age=[12,35,26,45,38];
//索引    for(let index in object){}
for (let number in age) {
    console.log(age[number]);
}
```



### 3.5、Map和Set

> ES6的新特性

Map:

```javascript
//SE6
//学生的成绩，学生的名字
let map=new Map([['tom',100],['jack',90],['haha',88]]);
let name = map.get('tom');//通过key获得value
map.set('admin',60);	//新增或修改
map.delete('tom');		//删除
console.log(map);
```

Set:无序不重复的集合

```JavaScript
let set=new Set([3,1,1,1,1]);//set可以去重
set.add(2);			//添加
set.delete(3);		//删除
console.log(set.has(1));//判断是否包含
console.log(set);
```



### 3.6、iterator

> ES6的新特性

遍历迭代Map和Set

遍历数组

```JavaScript
let arr=[9,6,7,2];
for (let number of arr) {
    console.log(number);
}
```

遍历Map

```JavaScript
let map=new Map([['tom',100],['jack',90],['haha',88]]);
for (let mapElement of map) {
    console.log(mapElement);
}
```

遍历Set

```javascript
let set=new Set([3,1,1,2,6]);//set可以去重
for (let number of set) {
    console.log(number);
}
```



## 4、函数及面向对象

### 4.1、定义函数

> 定义方式一

绝对值函数

```javascript
function abs(x){
    if(x>=0){
        return x;
    }else{
        return -x;
    }
}
```

一旦执行return，代表函数结束，返回结果

**如果没有执行return，函数执行完也会返回结果，结果为undefined**。

> 定义方式二

```javascript
var abs=function(x){
    if(x>=0){
        return x;
    }else{
        return -x;
    }
}
```

function(x){...}这是一个匿名函数，但是可以把结果赋值给abs，通过abs就可以调用

两种方法等价

> 调用函数

```javascript
abs(10)	//10
abs(-10)	//10
var sum = getSum; //getSum本身是一个函数名，而函数本身在js中就是一个对象。getSum就是这个函数对象的引用.将getSum这个引用的地址赋值给sum。这时sum也指向了这个函数对象。相当于这个函数对象有两个函数名称。
```

参数问题：

JavaScript可以传任意个参数，也可以不传参数

参数进来是否存在的问题？假设不存在参数，如何规避

```JavaScript
var abs=function(x){
    //手动抛出异常
    if(typeof x!=='number'){
        throw 'Not a number';
    }
    if(x>=0){
        return x;
    }else{
        return -x;
    }
}
```

> arguments

`arguments`是JS免费赠送的**关键字**

代表传进来的所有的参数是一个数组

```JavaScript
var abs=function(x){
    console.log("x==>",x);
    for (let i = 0; i < arguments.length; i++) {
        console.log(arguments[i]);
    }
    if(x>=0){
        return x;
    }else{
        return -x;
    }
}
```

问题：**arguments包含所有的参数，有时候想用多余的参数来进行附加操作，需要排除已有参数**

> rest

以前：

```javascript
if(arguments.length>2){
    for(let i=2;i<arguments.length;i++){
		//...
    }
}
```

ES6引入的新特性，获取除了已经定义的参数之外的所有参数 ...rest

```javascript
function a(a,b,...rest){
    console.log("a===>",a);
    console.log("b===>",b);
    console.log(rest);
}
```

**rest参数只能写在最后面，必须用...标识**



### 4.2、变量作用域

在JavaScript中，var定义的变量实际是有作用域的

假设在函数体中声明，则在函数体外不可以使用（非要实现，可以使用闭包）

```JavaScript
`use strict`;
function f() {
    var x=1;
    x=x+1;
}
x=x+2;//ReferenceError: x is not defined
```

如果两个函数使用了相同的变量名，只要在函数内部，不冲突

```javascript
`use strict`;
function f() {
    var x=1;
    x=x+1;
}
function f1() {
    var x=2;
}
```

**内部函数可以访问外部函数的成员，反之，则不行**

```javascript
function f() {
    var x=1;
    //内部函数可以访问外部函数的成员，反之，则不行
    function f1() {
        var y=x+1;
    }
    var z=y+1;//ReferenceError: y is not defined
}
```

假设内部函数变量和外部函数变量重名，

```JavaScript
function f() {
    var x=1;
    //内部函数可以访问外部函数的成员，反之，则不行
    function f1() {
        var x='A';
        console.log('inner==>'+x);//inner==>A
    }
    console.log('outer==>'+x);//outer==>1
    f1();
}
f();
```

在JavaScript中，**函数查找变量从自身函数开始，由“内”向“外”查找**，假设外部存在这个同名的函数变量，则内部函数会屏蔽外部函数的变量。

> 提升变量的作用域

```JavaScript
function f() {
    var x='x'+y;
    console.log(x);
    var y='y';
}
```

结果：xundefined     输出x和undefined拼接

说明**js执行引擎自动提升了y的声明，不会提升y的赋值**，等价于

```JavaScript
function f2() {
    var y;
    var x='x'+y;
    console.log(x);
    y='y';
}
```

> 全局函数

```JavaScript
//全局变量
x=1;
function f() {
    console.log(x);
}
f();
console.log(x);
```

全局对象window

```JavaScript
var x='xxx';
alert(x);
alert(window.x);//默认所有的全局变量都会自动绑定在window对象下
```

alert()这个函数本身也是一个`window`变量

```JavaScript
var x='xxx';

window.alert(x);
var old_alert=window.alert;
//old_alert(x);

 window.alert=function () {

 };

 //发现alert失效了
 window.alert(123);

 //恢复
 window.alert=old_alert;
 window.alert(456);
```

JavaScript实际上只有一个全局作用域，任何变量（函数也可视为变量），假设没有在函数作用范围内找到，就会向外查找，如果在全局作用域都没有找到，就会报`ReferenceError`

> 规范

由于所有的全局变量都会绑定到Windows上，如果不同的js使用了相同的全局变量，冲突->如何减少冲突？ 

```JavaScript
//唯一全局变量，用来装自己定义的全局变量
 var Mapp={};
 
 //定义全局变量
 Mapp.name='lisi';
 Mapp.add=function(a,b){
     return a+b;
 }
```

把自己的代码全部放入自己定义的唯一空间名字中，降低全局命名冲突问题

> 局部作用域	`let`

使用`var`定义

```JavaScript
function aaa() {
    for (var i = 0; i < 100; i++) {
        console.log(i);
    }
    console.log(i+1);//i出了作用域还可以使用
}
```

ES6使用`let`关键字，解决局部作用域冲突问题

```JavaScript
function aaa() {
    for (let i = 0; i < 100; i++) {
        console.log(i);
    }
    console.log(i+1);//ReferenceError: i is not defined
}
```

建议大家都使用`let`去定义局部作用域的变量

> ​	常量`const`

在ES6之前定义常量：**只要用全部大写字母命名的变量就是常量，建议不要修改这样的值**

```javascript
var PI=3.14;
console.log(PI);
PI='213';//可以改变这个值
console.log(PI);
```

在ES6引入了常量关键字`const`

```JavaScript
const PI=3.14;//只读变量
console.log(PI);
PI=354;// TypeError: invalid assignment to const `PI' ，失败
```



### 4.3、方法

> 定义方法

方法就是把函数放在对象里面，队形只有两个东西：属性跟方法

```JavaScript
var a = {
    name: 'lisi',
    birth: 2000,
    //方法
    age: function () {
        //今年-出生年月
        let year = new Date().getFullYear();
        return year - this.birth;
    }
}

//属性
a.name

//方法，一定要带（）
a.age()
```

this代表什么？拆开上面的代码看看

```JavaScript
function getAge() {
    //今年-出生年月
    let year = new Date().getFullYear();
    return year - this.birth;
}
var a = {
    name: 'lisi',
    birth: 2000,
    //方法
    age: getAge
};

//a.age()   ok
//getAge()  NaN		window
```

**这个this是无法指向的，默认指向调用他的那个对象**

> apply

在js中可以控制this的指向

```JavaScript
function getAge() {
    //今年-出生年月
    let year = new Date().getFullYear();
    return year - this.birth;
}
var a2 = {
    name: 'lisi',
    birth: 2000,
    //方法
    age: getAge
};

getAge.apply(a2,[]);//this指向了a2这个对象，参数为空 想要获取谁就指向谁
```



## 5、内部对象

> 标准对象

```javascript
>> typeof 123
<- "number"
>> typeof '123'
<- "string"
>> typeof true
<- "boolean"
>> typeof NaN
<- "number"
>> typeof []
<- "object"
>> typeof {}
<- "object"
>> typeof Math.abs
<- "function"
>> typeof undefined
<- "undefined"
```



### 5.1、Date

> 基本使用

```JavaScript
let date = new Date();//Date Tue Jun 09 2020 10:49:38 GMT+0800 (中国标准时间)
console.log(date);
date.getFullYear();//年
date.getMonth();//月 0-11 代表月
date.getDate();//日
date.getDay();//星期
date.getHours();//时
date.getMinutes();//分
date.getSeconds();//秒
date.getMilliseconds();//毫秒

date.getTime();//时间戳 全世界统一  1970.1.1 00:00:00 毫秒数
console.log(new Date(1591671532754));//时间戳转时间
```

> 转换

```javascript
>> var now=new Date(1591671532754);
>> console.log(now);
<- Date Tue Jun 09 2020 10:58:52 GMT+0800 (中国标准时间)
>> now.toLocaleString()//注意调用是一个方法，不是属性
<- "2020/6/9 上午10:58:52"
>> now.toGMTString()
<- "Tue, 09 Jun 2020 02:58:52 GMT"
```



### 5.2、Math

~~~JavaScript
    /*
	* 演示Math对象。该对象的中的方法都是静态的。不需要new，直接Math调用即   可。
	*/
var num1 = Math.ceil(12.34);//返回大于等于指定参数的最小整数。
var num2 = Math.floor(12.34);//返回小于等于指定数据的最大整数。
var num3 = Math.round(12.54);//四舍五入
var num4 = Math.pow(10,2);  //平方
~~~



### 5.3、Json

> Json是什么

早期，所有的数据传输习惯使用xml文件

- JSON(JavaScript Object Notation, JS 对象简谱) 是一种轻量级的数据交换格式。
- 简洁和清晰的**层次结构**使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地**提升网络传输效率**。

在JavaScript中，一切皆为对象，任何js支持的类型都可以用 JSON 来表示

格式：

- 对象都用 {}
- 数组都用 []
- 所有的键值对都使用 key:value

> JSON 字符串和 JavaScript 对象的转换

```javascript
var user={
    name:'lisi',
    age:3,
    sex:'男'
};
//对象转换为JSON
let jsonUser = JSON.stringify(user);//{"name":"lisi","age":3,"sex":"男"}

//JSON字符串转换为对象,参数为JSON字符串
let object = JSON.parse(jsonUser);//Object { name: "lisi", age: 3, sex: "男" }
```

很多人搞不清楚，JSON 和 JS 对象的区别

```javascript
var object={a:'hello',b:'hellob'};
var json='{"a":"hello","b":"hellob"}';
```



### 5.4、Ajax

- 原生的js写法：xhr 异步请求
- jQuery封装好的方法 $("#name").ajax("")
- axios 请求



## 6、面向对象编程

> 原型对象

JavaScript、java、c#...面向对象；JavaScript有一些区别

类：模板	原型

对象：具体的实例

在JavaScript中，这个需要换一下思维方式

原型：

```JavaScript
var Student={
    name:'lisi',
    age:3,
    sex:'男',
    run:function () {
        console.log(this.name+' run...');
    }
};

var xiaoming={
    name:'xiaoming'
};

//xiaoming的原型是Student
xiaoming.__proto__=Student;

var bird={
    fly:function(){
        console.log(this.name+' fly...');
    }
};

//xiaoming的原型是bird
xiaoming.__proto__=bird;
```

> class 继承

以前：

```JavaScript
function student(name) {
    this.name=name;
}

//给student新增一个方法
student.prototype.hello=function () {
    alert('hello');
};
```

`class`关键字是在ES6引入的

1、定义一个类，属性，方法

```JavaScript
//ES6之后
//定义一个类
class Student{
    constructor(name) {
        this.name=name;
    }
    hello(){
        alert('hello');
    }
}

var xiaoming=new Student("小明");
var xiaohong=new Student("小红");

xiaoming.hello();
```

2、继承

```JavaScript
class Student{
    constructor(name) {
        this.name=name;
    }
    hello(){
        alert('hello');
    }
}

class XiaoStudent extends Student{
    constructor(name,grade) {
        super(name);
        this.grade=grade;
    }
    myGrade(){
        alert('我是一个小学生');
    }
}

var xiaoming=new Student("小明");
var xiaohong=new XiaoStudent("小红",1);
```

本质：查看对向原型

![image-20200609120533024](https://gitee.com/xudongyin/img/raw/master/img/20200719183926.png)

> 原型链

__ proto __:

![image-20200609172547313](https://gitee.com/xudongyin/img/raw/master/img/20200719183931.png)



## 7、操作Bom元素

**BOM：Browser Object Model（浏览器对象模型）**

> 浏览器介绍

JavaScript和浏览器关系：JavaScript诞生就是为了让他在浏览器中运行

浏览器内核：

- IE 6-11
- Chrome
- Safari
- FireFox

三方

- QQ浏览器
- 360浏览器



### 7.1、window（重要）

> window代表浏览器窗口

```javascript
>> window.innerHeight
<- 244
>> window.innerWidth
<- 1536
>> window.outerHeight
<- 848
>> window.outerWidth
<- 1550
//可以调整浏览器窗口大小试试
```



### 7.2、Navigator

> Navigator封装了浏览器的信息

```javascript
>> navigator.appName
<- "Netscape"
>> navigator.appVersion
<- "5.0 (Windows)"
>> navigator.userAgent
<- "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:76.0) Gecko/20100101 Firefox/76.0"
>> navigator.platform
<- "Win32
```

大多数时候不会使用`navigator`对象，因为会被人为修改，建议使用这些属性来判断和编写代码



### 7.3、screen

> 代表屏幕的尺寸（像素px）

```javascript
>> screen.width
<- 1536
>> screen.height
<- 864
```



### 7.4、location（重要）

> 代表当前页面的URL信息

```javascript
host: "www.baidu.com"				//主机
href: "https://www.baidu.com/"		//地址
protocol: "https:"					//协议
reload: function reload()			//重新加载
//设置新的地址
>> location.assign("https://www.csdn.net/")
```



### 7.5、Document

> Document代表当前页面	HTML DOM文档树

```javascript
>> document.title
<- "百度一下，你就知道"
>> document.title="hahah"
<- "hahah"
```

> 获取具体的文档树节点

```HTML
<dl id="app">
    <dt>Java</dt>
    <dd>JavaEE</dd>
    <dd>JavaSE</dd>
</dl>

<script>
    let dl = document.getElementById('app');
    console.log(dl);
</script>
```

> 获取cookie

```javascript
>> document.cookie
<- "BAIDUID=4ABFE152F04D1B1C0AA7208BF816637D:FG=1; BIDUPSID=4ABFE152F04D1B1C0AA7208BF816637D; PSTM=1589766046; BDORZ=FFFB88E999055A3F8A630C64834BD6D0; BD_HOME=1; H_PS_PSSID=31724_1461_31325_21082_31780_31271_30823_31848_22160; BD_UPN=13314752; ORIGIN=1; ISSW=1; ISSW=1"
```

> 劫持cookie原理

```html
<script src="aaa.js"></script>
<!--恶意人员获取用户的cookie，上传到他的服务器-->
```

服务器端可以设置cookie：httpOnly，防止 cookie 泄露



### 7.6、History（不建议使用）

> 代表浏览器的历史记录

```javascript
history.forward();//前进
history.back();//后退
```



### 7.7、Window 对象的一些方法

~~~JavaScript
<script type="text/javascript">
    var timeid；//全局变量，如果写在某个方法的话别的方法就不能使用
    function windowMethodDemo(){
        var b = confirm("你真的确定要点击吗？");  //这里的方法都省略了window.
        alert("b="+b);  //上面confirm方法会弹出一个窗口，点击按钮会返回值

        setTimeout("alert('time out run')",4000); //设置多少毫秒弹出
        timeid = setInterval("alert('interval run')",3000); //返回的Integer对象clearInterval方法用，该方法的作用是每多少毫秒就弹出
    }

function stopTime(){
    clearInterval(timeid); //取消setInterval方法的调用
}

function windowMove(){
    //	moveBy(10,10);  窗口移动多少像素
    //	moveTo(40,40);  窗口移动到哪个坐标
    for (var x = 0; x < 700; x++){  //该代码段就是抖动窗口的功能
        moveBy(20, 0);
        moveBy(0, 20);
        moveBy(-20,0);
        moveBy(0,-20);
    } 
}

function windowOpen(){				 open("ad.html","_blank","height=400,width=400,status（状态栏）=no,toolbar（工具栏）=no,menubar（菜单栏）=no,location（地址栏）=no");
//第一个参数是打开的链接，第二个参数窗口target属性，第三个参数设置窗口的其他属性
  //		  close(); 弹出窗口后关闭该窗口
}
</script>

<input type="button" value="演示window对象的方法" onclick="windowOpen()"/>
<input type="button" value="停止" onclick="stopTime()"/>
~~~

### 7.8、Window 对象的一些事件

~~~JavaScript
<script type="text/javascript">
             /*
			 onunload = function(){  //窗口关闭后弹出
				alert("onunload run");
			} 

			onload = function(){  //窗口加载后弹出
				alert("onload run");
			}

			onbeforeunload = function(){  //窗口关闭时弹出
				alert("onbeforeunload run");
			}  已过时？
			*/
    onload = function(){
    window.status = "啊！，加载完毕啦";  //设置窗口加载后状态栏的信息
}
</script>
~~~



## 8、操作Dom对象（重点）

**DOM：Document Object Model（文档对象模型）**

DOM用来将标记型文档封装成对象，并将标记型文档中的所有的内容(标签，文本，属性等)都封装成对象。封装成对象的目的是为了更为方便的操作这些文档以及文档中的所有内容。因为对象的出现就可以有属性和行为被调用。
文档对象模型
		文档：标记型文档。
		对象：封装了属性和行为的实例，可以被直接调用。
		模型：所有标记型文档都具备一些共性特征的一个体现。
标记型文档(标签，属性，标签中封装的数据)，只要是标记型文档，DOM这种技术都可以对其进行操作。常见的标记型文档：html  xml

DOM技术的解析方式：将标记型文档解析一棵DOM树，并将树中的内容都封装成节点对象。
注意：这个DOM解析方式的好处：可以对树中的节点进行任意操作，比如：增删改查。
弊端：这种解析需要将整个标记型文档加载进内存。意味着如果标记型文档的体积很大，较为浪费内存空间。

简介另一种解析方式：SAX：是由一些组织定义的一种民间常用的解析方式，并不是w3c标准，而DOM是W3C的标准。
SAX解析的方式：基于事件驱动的解析。获取数据的速度很快，但是不能对标记进行增删改。

DOM模型有三种：
		DOM level 1：将html文档封装成对象。
		DOM level 2：在leve 1基础上加入了新功能，比如解析名称空间。
 	   DOM level 3：将xml文档封装成了对象。

DHTML:动态的HTML。不是一门语言：是多项技术综合体的简称.其中包含了HTML,CSS,DOM,JavaScript。

这四个技术在动态html页面效果定义时，都处于什么样角色呢？负责什么样的职责呢？

HTML:负责提供标签，对数据进行封装，目的是便于对该标签中的数据进行操作。简单说：用标签封装数据。		

CSS:负责提供样式属性，对标签中的数据进行样式的定义。 简单说：对数据进行样式定义

DOM:负责将标签型文档以及文档中的所有内容进行解析，并封装成对象，在对象中定义了更多的属性和行为，便于对对象操作。简单说：将文档和标签以及其他内容变成对象。 

JS:负责提供程序设计语言，对页面中的对象进行逻辑操作。简单说：负责页面的行为定义。就是页面的动态效果。

所以javascript是动态效果的主力编程语言。

DHTML+XMLhttpRequest = AJAX

### 8.1、核心

浏览器网页就是一个Dom树形结构

- 更新：更新Dom节点
- 遍历：得到Dom节点
- 删除：删除Dom节点
- 添加：添加一个新的Dom节点

要操作一个Dom节点，就要先获得这个Dom节点



### 8.2、获得Dom节点

```JavaScript
/*
获取节点的方法：
		1，getElementById():通过标签的id属性值获取该标签节点。返回该标签节点。一般容器型节点都有ID属性，所以就用这个获取。
		2，getElementsByName(): 通过标签的name属性获取节点，因为name有相同，所以返回的一个数组。
		3，getElementsByTagName(): 通过标签名获取节点。因为标签名会重复，所以返回的是一个数组。
*/
//对应CSS选择器
let byTagName = document.getElementsByTagName("h1");
console.log(byTagName);
let byId = document.getElementById('p1');
console.log(byId);
let byClassName = document.getElementsByClassName('p2');
console.log(byClassName);
let father = document.getElementById('father');
let children = father.children;//获取父节点下的所有子节点
let child=father.children[index];//获取第index个子节点
let firstChild = father.firstChild;//第一个子节点
let lastChild = father.lastChild;//最后一个子节点
```

这是原生代码，之后我们都尽量使用jQuery



### 8.3、更新Dom

```html
<div id="id1"></div>

<script>
    let id1 = document.getElementById('id1');
</script>
```

操作文本

- `id1.innerText='456';`修改文本的值
- `id1.innerHTML='123';`可以解析HTML文本标签

操作css

```javascript
>> id1.style.color='red'		//属性使用字符串
<- "red"
>> id1.style.fontSize='100px'	//下划线转驼峰命名问题
<- "100px"
>> id1.style.padding='2em'
<- "2em"
```



### 8.4、删除Dom

删除节点的步骤：**先获取父节点，再通过父节点删除自己**

```html
<div id="father">
    <h1>标题1</h1>
    <p id="p1">p1</p>
    <p class="p2">p2</p>
</div>

<script>
    let self = document.getElementById('p1');
    let father = self.parentElement;
    father.removeChild(self);
    
    //删除是一个动态的过程
    father.removeChildren(father.children[0]);
</script>
```

注意：**删除多个节点的时候，children的位置是时刻变化的。**



### 8.5、插入Dom

当获得了某个Dom节点，假设这个Dom节点是空的，通过innerHTML就可以增加一个元素，但是如果这个Dom节点已经存在元素了，就不能这么干，会覆盖。

> 将已有的标签追加到后面

```html
<p id="js">JavaScript</p>
<div id="list">
    <p id="me">JavaME</p>
    <p id="ee">JavaEE</p>
    <p id="se">JavaSE</p>
</div>

<script>
    let js = document.getElementById('js');
    let list = document.getElementById('list');
    list.append(js);//追加到后面
</script>
```

效果：

![image-20200609214501342](https://gitee.com/xudongyin/img/raw/master/img/20200719183946.png)

> 创建一个新的标签，实现插入

```html
<p id="js">JavaScript</p>
<div id="list">
    <p id="me">JavaME</p>
    <p id="ee">JavaEE</p>
    <p id="se">JavaSE</p>
</div>

<script>
    let js = document.getElementById('js');//已存在的节点
    let list = document.getElementById('list');
    list.append(js);

    //通过js创建一个节点
    let newP = document.createElement('p');//创建一个p标签
    newP.id='newP';
    newP.innerText='hello';
    list.append(newP);

    //创建一个标签节点
    let myScript = document.createElement('script');
    myScript.setAttribute('type','text/javascript');
    list.appendChild(myScript);

    //可以创建一个style标签
    let myStyle = document.createElement('style');//创建了一个空style标签
    myStyle.setAttribute("type",'text/css');//设置type为text/css
    myStyle.innerHTML='body{background: antiquewhite;}';//设置标签内容
    let head = document.getElementsByTagName('head');//将style追加到头部
    head[0].appendChild(myStyle);//head的第0个才是head

</script>
```

效果：

![image-20200609221608486](https://gitee.com/xudongyin/img/raw/master/img/20200719184029.png)

> 插入到前面	insertBefore

```JavaScript
//insertBefore(newNode,targetNode)
let js = document.getElementById('js');//新的节点
let list = document.getElementById('list');//包含的父节点
let ee = document.getElementById('ee');//目标节点
list.insertBefore(js,ee);
```



## 9、操作表单（验证）

### 9.1、表单是什么

- 文本框	text
- 下拉框 < select >
- 单选框 radio
- 多选框 checkbox
- 隐藏域 hidden
- 密码框 password
- ....

表单的目的：提交信息



### 9.2、获得要提交的信息

```HTML
<form action="" method="post">
    <span>用户名</span>
    <input type="text" id="username">
    <!--多选框的值就是value的值-->
    <p>
        <span>性别</span>
        <input type="radio" name="sex" value="man" id="boy">男
        <input type="radio" name="sex" value="women" id="girl">女
    </p>

</form>


<script>
    let username = document.getElementById('username');
    let boy = document.getElementById('boy');
    let girl = document.getElementById('girl');
    //得到输入框的值
    username.value;
    //修改输入框的值
    username.value = '123';

    //对于单选框，多选框等固定的值，用.value只能取到当前的值
    boy.value;
    boy.checked;//查看返回的结果是否为true，如果为true，则为选中
    boy.checked=true;//赋值

</script>
```



### 9.3、提交表单

> md5加密密码

md5在线cdn：https://www.bootcdn.cn/blueimp-md5/

```html
<!--表单绑定提交事件
    onsubmit 绑定一个提交检测函数 true false
    将这个结果返回表单，使用onsubmit接收
    onsubmit="return check()"-->
<form action="https://www.baidu.com/" method="post" onsubmit="return check()">
    <p>
        <span>用户名</span>
        <input type="text" id="username" name="username">
    </p>
   <p>
       <span>密&nbsp;&nbsp;&nbsp;&nbsp;码</span>
       <input type="password" id="input-password">
       
       <!--隐藏域作用:如果直接把input-password加密，在提交表单的一瞬间密码会变长，因			为进行了加密，加隐藏域可以保持密码长度不变，服务器获取的数据也应该是id为
		md5-password的数据，id为input-password只是过渡作用-->
       <input type="hidden" id="md5-password" name="password">
   </p>

    <!--绑定事件 onclick 被点击-->
    <button type="submit">提交</button>

</form>


<script>
    function check() {
        let username = document.getElementById('username');
        let input_password = document.getElementById('input-password');
        let md5_password = document.getElementById('md5-password');
        // MD5算法
        md5_password.value=md5(input_password.value);

        //可以检验表单内容，true通过，false阻止
        return true;

    }
</script>
```



## 10、jQuery

JavaScript

jQuery库，里面存在大量的JavaScript函数

> 引入jQuery

（官网：https://jquery.com/）

（文档：https://jquery.cuishifeng.cn/index.html）

（在线cdn：https://www.bootcdn.cn/jquery/）



### 10.1、初识jQuery

公式：$(选择器).事件函数()

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--cdn引入-->
    <!--<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.js"></script>-->

    <script src="lib/jquery-3.5.1.js"></script>
</head>
<body>

    <!--
        公式：$(selector).action()
    -->
    <a href="" id="test-jquery">点我</a>

    <script>
        //选择器就是css选择器
        $('#test-jquery').click(function () {
            alert('hello jquery');
        })
    </script>

</body>
</html>
```



### 10.2、选择器

文档工具站：https://jquery.cuishifeng.cn/index.html

```JavaScript
//原生js，选择器少，麻烦不好记
//标签
document.getElementsByTagName();
//id
document.getElementById();
//类
document.getElementsByClassName();

//jquery    css中的选择器，他都能用
$('p').click();//标签选择器
$('#id1').click();//id选择器
$('.class1').click();//类选择器
```



### 10.3、事件

鼠标事件，键盘事件，其他事件

![image-20200610105953573](https://gitee.com/xudongyin/img/raw/master/img/20200719184038.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="lib/jquery-3.5.1.js"></script>
    <style>
        #divMove{
            width: 500px;
            height: 500px;
            border: red solid 1px;
        }
    </style>
</head>
<body>

	<!--获取鼠标当前的坐标-->
	mouse：<span id="mouseMove"></span>
    <div id="divMove">
        在这里移动鼠标试试
    </div>

    <script>
        //当网页元素加载完毕之后响应事件
        $(function () {
            $('#divMove').mousemove(function (e) {
                $('#mouseMove').text('x:'+e.pageX+"  y:"+e.pageY);
            })
        })
    </script>
</body>
</html>
```



### 10.4、操作DOM

> 文本节点操作

```html
<ul id="test-ul">
    <li class="js">javascript</li>
    <li name="python">python</li>
</ul>

<script>
    $('#test-ul li[name=python]').text();//获得值
    $('#test-ul li[name=python]').text("123");//设置值
    $('#test-ul').html();//获得值
	$('#test-ul').html("<strong>123</strong>");//设置值
</script>
```

> css操作

```JavaScript
$('#test-ul li[name=python]').css({'color':'red','fontSize':'30px'});
```

> 元素的显示和隐藏

本质：`display:none`

```JavaScript
$('#test-ul li[name=python]').hide();//隐藏
$('#test-ul li[name=python]').show();//显示
$('#test-ul li[name=python]').toggle();//隐藏显示轮换，调用一次换一次
```

> 娱乐测试

```JavaScript
$(window).width();//浏览器窗口
$(window).height();
```

> 小技巧

1、如何巩固 JS

- 看jQuery框架源码
- 看游戏源码

2、巩固HTML、CSS

- 扒网站，全部down下来，然后对应修改，看效果