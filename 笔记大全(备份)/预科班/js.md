# 0729.js的输入输出和书写位置

## 01.js初相识

##### 如何在html里面开辟js环境

<script>
    XXXXXX
</script>


##### js简介

JavaScript是一种面向对象的脚本语言

创始人:布兰登.艾奇

js标准名:ECMAScript



##### js能做什么

**表单动态校验**(密码强度检测)(JS产生最初的目的)

**网页特效**

**桌面程序**(Electron)

**服务端开发**(Node.js)

**App**(Cordova)

控制硬件-**物联网**(Ruff)

**游戏开发**(cocos2d-js)

我们熟知的**微信小程序、插件扩展、游戏特效、刷浏览量**……很多都是可以利用JS开发使用的



##### 浏览器执行JS

**浏览器由多个部分组成其中就有：渲染引擎和JS引擎**

渲染引擎：用来**解析html和css**，所称内核

JS引擎：也称为JS解释器。用来读**取网页中的JavaScript代码**，对其处理后运行



##### Js的组成

![img](https://cdn.nlark.com/yuque/0/2025/png/27167233/1744095056764-e00dfd25-41ff-4637-9a60-a130f2ae6ffb.png)

（1）、ECMAScript （ JavaScript的核心 是规范标准）**(ES)**

描述了语言的**基本语法**(var、for、if、array等)和数据类型(数字、字符串、布尔、函数、对象(obj、[]、{}、null)、未定义)。

只学习基础语法，做不了常用的网页交互效果，为后面的内容打基础，做铺垫

（2）、DOM （Document Object Model文档对象模型，可以去操作网页）

Document(文档)

Object(对象)

Model（模型）

（3）、BOM 浏览器对象模型，操作浏览器





## 02.js输入输出语句

##### 注释:

单行注释   //    ctrl+/

多行注释    /*   */       shift+alt+A





##### 输出语句1:警告框弹出   alert

alert('hello');

alert(123)

依次弹出hello和123

**不能识别标签**

alert("<h1>你好</h1>")

**弹出  <h1>你好</h1>**



##### 输出语句2:页面输出  document.write

向body中输出一个内容

document.write(123);

document.write("hello");

网页主体显示123hello

**页面输出可以识别标签**

document.write("<h1>你好</h1>");

**网页显示h1级标题的你好**



##### 输出语句3;控制台输出   console.log

console.log(123);

console.log("hello");

console.log(666)

控制台依次输出123,hello,666



##### 输入语句1:用户输入数据

prompt("请输入你的名字")

仅仅是输入,什么效果都没有

输入后输出

alter(prompt("请输入你的名字"))    #输入名字后弹出警告框输出名字

document.write(prompt("你今年多大了？"));    #输入年龄后网页输出年龄

console.log(prompt("你叫什么名字"));     #输入名字后控制台输出名字

##### 输入语句2:确认输入

confirm('你确定要出去玩吗？')

***确认框***

确认:true

取消:false



## 03.js的书写位置

##### 1.书写位置1：body内，直接script开辟空间



##### 2.书写位置2：head内，直接script开辟空间



##### 3.书写位置3：在标签的事件属性,属性值的位置书写js语句

事件属性:跟交互行为相关的属性 ,这类的属性,都是以on开头,属性值写js语句

常用的事件属性:跟交互行为相关的属性 ,这类的属性,都是以on开头

   onclick : 当鼠标点击时

   ondblclick :当鼠标双击时

   onmouseenter:当鼠标移入

   onmouseleave:当鼠标移出

   onmousemove:当鼠标移动时

   oninput : 当用户输入时

   onchange :当内容改变时

   onfocus : 当输入框聚焦时

   onblur : 当输入框失焦时

**例如:**

/<div class="box" onclick="console.log('你点我干嘛')"></div>    #每点一次div,控制台输出一次你点我干嘛

<input type="text" onfocus="document.write('你输入的是啥')" />  #每聚焦一次输入框,控制台输出一次你输入的是啥

/  

**总结:**和css的内联样式一样,标签(html)和行为(js)耦合了,不好维护,不方便复用,不好修改



##### 4.书写位置4：外部书写js文件,通过script标签的src属性引入

优势：可以在不同的页面中同时引用，也可以利用到浏览器的缓存机制，推荐使用方式

  如果这个script的标签就是为了引入js文件,那就不能在其标签体的位置,

  书写其他的js语句,也不会执行的,例如下面的console.log不会执行

<script src="./03.js">
		console.log('哈哈哈哈');
</script>

## 04.js基础语法

（1）js中严格区分大小写

（2）js中每一条语句以分号（;）结尾 

​    -如果不写分号，浏览器会自动添加，但会消耗一些系统资源,大部分情况下，可以不加分号，

​    一些特殊情况，必须要加分号，如果你不加，浏览器默认添加的，可能会加错

（3）js中会自动忽略多个空格和换行，所以我们可以利用空格和换行对代码进行格式化 



# 0730.变量和变量的声明

## 01.字面量&变量

##### 字面量(不就是常量吗)

固定量,不会变的量

console.log(1)   //对数据的操作,永远只有这一种情况

console.log("hello")



##### 变量

可变化的量,不会固定的量

var x = 1

console.log(x);  *//对数据操作*

在代码的世界，我们更加喜欢用变量，不直接使用字面

   申请变量的本质：定义1个变量，相对于在内存中申请1个空间，变量名就是空间名，用来存放**字面量**



## 02声明变量(3种方式)

##### 声明变量：**var**

   设 x = 1

###### 1.1  **声明变量，变量名为x，变量值为1**

将字面量1赋值给变量x

​    **var** x = 1;

更新变量,直接写就行,不需要var了

​	       x = 2;

console.log(x);    #控制台输出:2

###### 1.2  仅声明变量但不赋值

​     **var** y;

console.log(y);   #控制台获取到的是：**undefined**

变量赋值,因为上面没有赋值,所以这里是复制而不是更新变量

​    y = "hello";



​    **var** a = 1;

​    **var** b = 2;

​    **var** c = 3;

###### 1.3  同时定义多个变量  a,b,c

​    **var** a = 1,b = 2,c = 3;





#####    声明变量：**let**

​    **let** x = 1;   #声明变量并赋值

​    x = 2;     #更新变量



​    **let** y;    #声明变量不赋值

​    y = 2;   #变量赋值



​    **let** a=1,b=2,c=3;   #声明多个变量并赋值





#####    声明变量：**const**

​    **const** x = 1;   #声明变量并赋值

​    x = 2;    #**const 声明的变量，不允许更新**



​    **const** y;     #**const 不允许 声明变量不赋值**

​    console.log(x);



​    **const** a = 1, b = 2,c = 3;   #声明多个变量并赋值

​    console.log(a, b, c);

每声明一次开辟一个内存





## 03.var 和 let&const 的区别

**var** 和 let、const 的区别

#####    1、**var** 是es5的写法，let/const 是es6的新语法

记下就行

#####    2、**var** 可以重复声明同一个变量,**let/const** 不可以

​    **var** x = 1;

​    **var** x = 2;

​    **var** x = 3;

​    console.log(x);   #输出3

***下面这样,直接报错***

​    **let** x = 1;

​    **let** x = 2;

​    **const** y = 1;

​    **const** y = 2;



#####    3、var没有作用域，全局作用域(全局有效)，**let**/const 有块级作用域

​    这真不是全局变量和局部变量?

​    作用域：某个变量起作用的范围

​    {       **#花括号内是个作用域**

​     **var** a = 1;

​     **let** b = 2;

​         console.log(a);

​         console.log(b);    **#控制台中a,b都输出,两个都有效**

​    }

​    console.log(a);

​    console.log(b);  **#控制台中只输出a,而b在作用域外,不输出**

但这时候再用let声明b,我们刚才说let不能重复声明变量,不过这里是没问题的,因为作用域变化了**,解决变量污染的问题**

**let** b=3

console.log(b);-----输出3



#####    4、**var** 有变量提升，可以先使用,返回的是undefined，再定义，**let** **/const** 不允许

​    console.log(a);   #控制台显示undefined

​    **var** a = 1;  #再定义

​    下面这样会报错

​    console.log(b); 

​    **let** b = 1;



#####    5、**var** 与顶级对象window挂钩，而let /const 没有

​	简单来说:用`var`在**全局作用域**声明变量，会自动变成`window`的属性

​    **var** 声明的变量，都会在window对象的下方，都可以通过window.xx的方式获取

   只不过我们一般省略window.



​    **var** a = 1;

​    console.log(a);

​    等价于console.log(window.a);    #既是a,也是window.a,不管是声明还是调用的时候



​    **let** a = 1;

​    console.log(a);     #1

​    console.log(window.a);    #undefined,a并没有在window的下方







## 04.let和const的区别

**let&const** 的区别

#####     相同点

**let、const** 都是es6语法，不可以重复声明，有块级作用域，不与顶级对象挂钩，先定义再使用

##### 	不同点

   1、**let** 定义的变量允许修改，const不允许

   2、**let** 允许先声明再赋值，const不允许,const声明的时候必须赋值

##### 	注意

   1.后期就使用let，const定义变量，不使用var，

   2.一些不希望别人修改的值，就用const



## 05.变量的命名规范(神了,废话)

变量的命名规范

##### 3个规则（必须要遵守的）

   变量名必须由字母、数字、$、_ 构成，不允许有其他字符

   不能以数字开头

   不能是js底层的关键字（js自己已经使用了的名字）

#####    3个规范（建议你要遵守的，可以不遵守）

   见名知义

   不建议使用汉字

   多个单词时，用小驼峰或者_连接



# 0731.数据类型(简单数据类型)

## 01.变量的类型

声明变量a,把数值1赋值给a

   设 变量名 = 变量值

   变量的类型  变量值的类型

   js是一种弱类型的语言，所以js中的变量本身是没有类型，

   **变量会看变量值是什么类型，变量就是什么类型**

​    **let** a = 1;    a现在是数字类型

​    a = "hello";   a现在是字符串类型

   c,java等语言，属于强类型语言，定义变量时，必须要先确定变量的类型，

​    变量值必须符合这个类型

   **let** a:string='hello'    限定a是str类型

​    **let** b = 123;     **#变量b是数值类型**

​    **let** c = "hi";      **#变量c是字符串类型**

 

## 02.number类型(数字型)

##### 1、定义整数&浮点数

​    **let** a = 1;

​    **let** b = 1.1;

##### 2、检测某个变量的类型语法：  typeof   数据

​    检测变量a的类型,赋值给变量res1

​    **let** res1 = typeof a;

​    console.log(res1);     #number

​    console.log(typeof a);    **#也可以直接写**

​    console.log(typeof b);    #控制台输出number

##### 3、数值是由范围的，有最大值，有最小值

并不是无穷无尽的    

console.log(Number.MAX_VALUE); *//1.7976931348623157e+308*

​    console.log(Number.MIN_VALUE); *//5e-324*

##### 4、整数可以正常运算，浮点数在js中，一般不参与运算

​    **let** a = 211;

​    **let** b = 121;

​	console.log(a + b);   #结果是332

​		更新变量

​    a = 0.1;

​    b = 0.2;

​    console.log(a + b);   #结果是0.30000000000000004,不精确,所以不参与运算

##### 5、特殊数值

###### 5.1、NaN   (not a number) 非数值

​    **let** a = NaN;

​     console.log(typeof a);    **#虽然叫非数值,但类型还是number**

**如何检测a到底是不是NaN:**  用isNaN(变量)，如果是NaN就返回true，不是则返回false

​    a = 1;

​    **let** res = isNaN(a);

​    console.log(res)/////console.log(isNaN(a))   #结果:true

###### 5.2、Infinity 无限数

**let** b = Infinity;

console.log(typeof b);   #返回number



## 03.Boolean类型(布尔值)

#####  1、定义布尔类型

   Boolean布尔值类型,就2个值，true，false

   一般结合js语句使用，if语句，for语句

   **let** a = true;

   **let** b = false;

​    console.log(typeof a); *//boolean*

​    console.log(typeof b); *//boolean*

#####     2、布尔类型的运算

   console.log(true + 1);   #结果是2

   console.log(false + 1);   #结果是1

 

## 04.Undefined类型(未定义值)

Undefined :  未定义,值就一个，就是undefined

#####     定义undefined值：方式一

   **let** a;     **#只定义了变量,未赋值**

   console.log(a);    #返回的是undefined值

   console.log(typeof a);     #返回的是undefined类型



#####     定义undefined值：方式二

   **let**  b = undefined;   **#直接定义为undefined**

   console.log(b);    

   console.log(typeof b); *//undefined类型*



## 05.Null类型(空值)

Null:空，值就一个，就是null

#####    定义null方式

   **let** a = null;

   console.log(typeof a); 检测null类型，竟然得到**object**，这是typeof的错误

   ***检测null的类型，不能用typeof，会得到一个错误的结果***



#####    **通用的检测数据类型的方式(什么类型都可以)**

   a = NaN;

   a = 123;

   a = true;

   a = undefined;

   **let** res = Object.prototype.toString.call(a);

   console.log(res);   #分别返回NaN,number,boolean,undefined



***//null相关的面试题***.扫码了

   *//1、为什么typeof检测null，结果确为object*

Javascript在存储数据的时候，不同的数据在底层存储都使用二进制的方式存储，在Javascript中如果二进制的前三位都为0的话，就会被判断为object，null是js后期添加的数据类型，它的二进制存储表示形式为全是0，自然前三位也是0，因此执行typeof时会返回”object”，这本身也是这门语言的BUG。

   *//2、null和undefined有什么区别*

定义：null:需要定义并赋值为null，表示为空，undefined：要么定义不赋值，要么赋值为undefined

作用：null独有使用场景

检测方式：typeof可以检测undefined，null无法用typeof检测,需要用通用的检测方式



   //3、null的使用场景有哪些

- 初始化引用类型变量（占位）
-  显式清空引用类型值（释放内存 / 重置状态/间接可以解绑事件）
- API / 函数返回合法空结果（ **逻辑上允许返回空值**）
- DOM 操作判断： 找不到元素时，**原生返回** `**null**`
- TS 类型标记：“变量可以是某类型，也可以是 null”



## 06.String类型

#####    1、定义符号(3种)

定义一个字符串，''  ""  ``(反引号)，任意字符放在前面三种引号内，都是一个字符串

   '' ""内字符串不可以换行，``可以换行，识别（承认）空格，换行

   **let** str1='hello'

   **let** str2="hi"

   **let** str3= `  你好   `

​    **let** str4="null"   #字符串,不是null类型

​    console.log(typeof str1);

​    console.log(typeof str2);

​    console.log(typeof str3);



#####    2、任意类型的数据和字符串相加，都得到一个新的字符串,叫拼串

​    **let** a = 1;

​    **let** b = "2";

​    **let** res = a + b;

​    console.log(res);    #返回  12

​    console.log(typeof res);  #类型是string

​    **let** a = True;

​    **let** b = "2";

​    **let** res = a + b;

​    console.log(res);    #返回  True2

​    console.log(typeof res);  #类型是string

​     undefined和null同理



#####     3、字符串和变量的拼接---引号引号中间加加,中间变量

   输出：我叫xxx,我今年xx岁了，很高兴认识大家

​    **let** name = prompt("请输入你的名字");   **#用户输入,并赋值给name**

​    **let** age = prompt("请输入你的年龄");  **#同理**



###### **拼接用加号+**,因为我们声明str还是字符串,所以外面要加引号

​    //  let str = "我叫" + name + ",我今年" + age + "岁了，很高兴认识大家";

​    // let str='我叫*'+name+',我今年'+age+'岁了，很高兴认识大家'*

​    //  let str = `我叫` + name + `,我今年` + age + `岁了，很高兴认识大家`;

###### 如果是反引号也可以用${}中间放变量

​    **let** str = `我叫${name},我今年${age}岁了，很高兴认识大家`;



#####    4 、获取字符串的长度

​    **let** str = "hello";

​    console.log(str.length);   #5



#####     5、字符串中的转义符 \ (告诉浏览器，\后的就是普普通通的字面符号)

​    **let** str=" 孔子曰：\\\"学而时习之\\\" "

   **let** str = "2026\\\7\\\31";

   console.log(str);

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260803000923258.png" alt="image-20260803000923258" style="zoom:50%;" />



# 0803类型的转换

## 01.其他数据类型的转换 

##### 为什么要进行类型转换

   **let** age = prompt("请输入你的年龄");

​    alert(age + 1);

​    alert(typeof age);   **#结果是191,拼串了**

   **prompt获取到的用户输入，类型是string，string和任意类型相加，都会拼串，**

​    我们希望获取到的是一个number，所以这里需要将string类型转成number

如何操作:

age=Number(age)

alert(age + 1);

alert(typeof age);   **#结果是20**





   Number，Boolean，String,Null,Undefined

   2、其他类型转成Number

   3、其他类型转成Boolean

   4、其他类型转成String

   

#####    其他类型不能转成Null，其他类型不能转成Undefined，

原因

null只能定义变量并赋值为null

undefined只有定义变量不赋值或者赋值为undefined







## 02.其他类型转换成Number类型

##### 其他类型转成Number后的结果的情况(un字符变na,nu空格变成0,别的变数字)

###### Boolean型

   如果是true，转成数值为1，如果是false，转成数值为0

   **let** oVal = true;

​    oVal = false;



###### string型

   **如果是纯数值的String类型，可以直接转成对应数值的Number类型**

​    oVal = "123";



​    **如果是除数值外有其他字符的String类型，转成数值NaN**

​    oVal = "hello";

​    oVal = "123a";



​    **如果字符串是空串或者是全是空格的字符串，都会转成数值为0**

​    oVal = "";

​    oVal = "     ";



###### null型

​    *//null类型换成数值为0*

​    oVal = null;



###### undefined型

​    *//undefined类型转成数值为NaN*

​    oVal = undefined;



#####    方式一：Number()  正规军,js底层给我们封装的方法，可以直接使用）

​	oVal在上面定义了

​    **let** nVal = **Number(oVal);**



#####    方式二：隐式转化 （利用了js底层的规则，帮助我们转换）

######    2.1、除加法以外的运算的方式，-0, *1, /1 

(你要进行运算，必须要先转成number类型才能运算,js底层帮我们转换了)

​    **let** nVal = oVal - 0;

​    nVal = oVal * 1;

   nVal = oVal / 1;

JavaScript 在做**减法（`-`）**运算时，会优先将两边的值转为**数字类型**,再计算

null-2  >>>>   -2

######    2.2、正号 （在任意类型数据的前面加'+'，js都会将它转成number）

​    **let** nVal = +oVal;



#####    方式三：特殊方式，专门用来处理以数字开头的字符串，

######     3.1、用来处理整数+其他字符的字符串---parseInt

   oVal = "123a";

   **let** nVal = **parseInt**(oVal);   #取整.**结果是123**

######    3.2、用来处理浮点数+其他字符的字符串---parseFloat

   oVal = "1.23a";

   nVal = **parseFloat**(oVal);  #取浮点数,**结果是1.23**



***赋值类型***

   ***let res1 = Object.prototype.toString.call(oVal);***

   ***let res2 = Object.prototype.toString.call(nVal);***

***输出值和类型***

   ***console.log(`原值：${oVal},原值得类型${res1}`);***

   ***console.log(`新值：${nVal},新值得类型${res2}`);***









## 03.其他类型转换成String类型

##### 其他转成String的结果(原来是什么就是什么,只是变成了str)

   **let** oVal = true;-----转成true

​         oVal = false;-----转成false

   	  oVal = null;-----转成null
   	
   	  oVal = undefined;-----转成undefined
   	
   	  oVal = 123;-----转成123

​    	 oVal = 1.23;-----转成1.23

​    	 oVal = NaN;-----转成NaN



##### 方式一：String() 正规军，js底层给我们封装的方法，可以直接使用

​    **let** nVal = String(oVal);



##### 方式二：toString()变量:    变量名.toString(),注意，null和undefined不能使用

​    **let** nVal = oVal.toString();

​	**null和undefined不能使用**



##### 方式三：隐式转换  + ''   （js底层因为某种规则，自己去转的）

   **let** nVal = oVal + "";  **#加空字符串的方式**



***赋值类型***

   ***let res1 = Object.prototype.toString.call(oVal);***

   ***let res2 = Object.prototype.toString.call(nVal);***

***输出值和类型***

   ***console.log(`原值：${oVal},原值得类型${res1}`);***

   ***console.log(`新值：${nVal},新值得类型${res2}`);***







## 04.其他类型转换成Boolean类型

##### 其他字符转成Boolean后的情况(只有正常数字和正常字符串是True)

###### 	string   

除了空串，其他任意字符串，转成boolean都为true，空串为false

   **let** oVal = "hello";

​    	 oVal = "123";

  	   oVal = "1.234a";
  	
  	  oVal = "      ";   #这些都是True

​	     oVal = "";         #只有空串是false



######     number

​    除了0，NaN，其他任意数值，转成boolean都为true，0，NaN为false

​    oVal = 123;

​    oVal = 1.23;

​    oVal = Infinity;     #这些都是True



​    oVal = 0;

​    oVal = NaN;          #这两个是false





######     null和undefined转成boolean为false

​    oVal = null;

​    oVal = undefined;

本身为false

​    oVal = false;



#####    面试题：js中为false的6种情况

布尔值:false

数字:0

数字:NaN

字符串:空串:""

未定义:undefined

空:null



#####    方式一：Boolean() ==>正规军，js底层给我们封装好的，可以直接使用的方法

​    **let** nVal = Boolean(oVal);



#####    方式二：隐式转换  !! 双非  ==>单个非，js底层会将这个这类类型转成布尔类型，并取反，

   再加!，将取反后的值，再取反，还原原布尔类型

   oVal = true;

   oVal = "hello";

   oVal = "";

   **let** nVal = !!oVal; *//取反 **,两次取反，返回原值类型***



***赋值类型***

   ***let res1 = Object.prototype.toString.call(oVal);***

   ***let res2 = Object.prototype.toString.call(nVal);***

***输出值和类型***

   ***console.log(`原值：${oVal},原值得类型${res1}`);***

   ***console.log(`新值：${nVal},新值得类型${res2}`);***







# 0804运算符

## 01.运算符初相识

   运算符：可以对1个值或多个值进行运算，最终有结果

   表达式:**由运算符和变量或者常量组成的式子，最终有一个结果 **    ,     这个结果，我们叫返回值

​	typeof     #就是一个运算符

   **let** res = typeof 1;   #表达式

   **let** res2 = 1 + 1;





## 02.算数运算符

   \+  -  *  /  %

#####    1、如果是数值运算，跟数学运算是一样的



#####    2、非数值运算

######    2.1、字符串和任意类型相加

​			结果都是拼串，得到一个新的字符串,这个优先级最高(因为＋号不能用来隐式转化)

######    2.2、如果是非数值和数值进行运算

​			会将非数值转成数值，再进行运算（除了上面的+字符串）

######    2.3、NaN和任意值进行运算

​			结果都是NaN（除了上面的+字符串）

   **let** a = 1;

   a = true;

   a = "123";

   a = null;                 #null转数值为0

   a = undefined;     #undefined转数值NaN

   a = "   ";                 #空格串转成数值，结果0

   **let** b = 2;

   **let** res = a + b;

   console.log(res);

   

   **let** result = 1 + 2 + "3";    #33   能计算,计算后再拼串

   result = "1" + 2 + 3;          #123   拼串后继续串







## 03.一元运算符+-

   \+ - 相同点：都会对非数值类型转成数值类型

   \+ - 不同点：+不会对数字有任何影响，-会对数字取负数

   **let** res1 = -"123"; *//-123*

   **let** res2 = +"123"; *//123*



   **var** a = 123;

   a = +a; 

   a = -a; 

   **var** result = 1 + "2" + 3;    #123   拼串

   result = 1 + +"2" + 3;      #6   变number了,再相加

   result = 1 + +true;     #2  同理,变number了,再相加



   console.log(typeof +true); *//number*

   console.log("result=" + result); *//2*





## 04.递增递减运算符++a,a++

#####    递增运算符

   **++ 递增运算符：如果你要对一个数字反复的+1，此时就可以用到递增**

###### 前置和后置的区别

   相同点：无论是前置递增，还是后置递增，**对于变量a本身来说，结果都会加1**

   区别：前置递增++a，后置递增a++，**这个整体的值不一样**

   前置递增++a,    会先给a加1，再返回这个整体值,相当于返回a+1

   后置递增a++， 会先返回a，再给a加1

​    **let** a = 1;

###### 后置递增  a++

​    **let** res = a++;    #res=1,a=2

​    console.log(res);

###### 前置递增 ++a

​     **let** res = ++a;   #res=2,a=2 

​     console.log(res);

**例题**

​    **let** b = 11;

​    **let** res = ++b + b++ + b;

​    *//  ++b:12   b:12*

​    *//   b++:12   b:13*

​    *//  13*

​    console.log(res);  #37



#####    递减运算符(同理)

   无论是前置递减，还是后置递减，对于变量c来说，都会递减1

   前置递减，后置递减，他们自己本身的值，前置递减会先减再返回，后置递减会先返回，再减

   **let** c = 10;

   **let** res1 = c--;  #res1=10   c=9

   **let** res2 = --c;  #res2=8     c=8

   console.log(c);*//8*

   console.log(res1);*//10*

   console.log(res2);*//8*

   





## 05.逻辑运算符

##### 非  !

######    1.1、非会对布尔值进行取反

######    1.2、如果对一个非布尔值取反,会把这个值转成布尔值类型，再取反，

   所以我们常用!!来将一个类型转成布尔类型

​    **let** res = true;

​    res = "123";

​    console.log(!res);  #false



##### 与  &&   (找false)

######    1、对于布尔值进行与运算： 只要有1个false，返回的就是false

######    2、对于非布尔值进行运算，会先将值转成布尔值，进行运算，最终返回对应的原值

​    **let** res1 = true;

​    **let** res2 = false;

​    **let** res = res1 && res2;

​    console.log(res);    **#false**



###### 短路与

&&运算是一个找false的过程，一旦找到false就不往下找了，这个过程，我们叫**短路与**

**而且返回的不只是false,而是对应false的原值**

比如a=null,b=1,a&&b   找到false了(a),返回null,而不是false

比如a=1,b="casd",  a&&b   找不到false了,返回casd,而不是True(因为找到了最后,取最后一个)



##### 或  ||  (找True)

######    1、对于布尔值进行或运算： 只要有1个true，返回的就是true

######    2、对于非布尔值进行或运算，会先将值转成布尔值，运算之后，再返回原值

   或运算，是一个找true的过程，一旦找到true，它就不往下找了，这个过程，我们叫**短路或**

​    **let** res1 = true;

​    **let** res2 = false;

​    **let** res = res1 || res2;

​    console.log(res);    **#true**



###### 短路或

或运算，是一个找true的过程，一旦找到true，它就不往下找了，这个过程，我们叫**短路或**

比如a=1,b=null,a||b   找到true了(1),返回1,而不是true

比如a=null,b="undefined",  a||b   找不到True了,返回undefined,而不是false(因为找到了最后,取最后一个)



###### 练习

​    **var** result = 1 && 2;    **2**

​    result = 0 && 2;			**0**

​    result = NaN && 0;     **NaN**

​    result = 1 || 2;          **1**

​    result = 2 || NaN;     **2**

​    result = NaN || 1;    **1**

​    result = "" || "hello";   **hello**

​    result = -1 || "你好";     -**1**



   **let** str = "hello";

   str.length && alert("123");   **#弹出123,因为找不到false**





# 0805运算符

## 06.赋值运算符

   **let** a = 1;

​    	 a **+=** 1; 	*//a=a+1*

​    	 a **-=** 1;	 *//a=a-1*

​    	 a ***=** 2;

​    	 a **/=** 2;

​    	 a **%=** 2; 	*//a=a%2*

​    	 a **||=** 2; 	*//a=a||2*

​    	 a **&&=** 2; 	*//a=a&&2;*





## 07.关系运算符

**\>  >=  <  <=** ,用于比较2个值之间的关系，关系成立，则返回true，不成立则返回false

#####    1、如果是2个数值之间的比较，就是数学之间的比较

   **let** a = 10;

#####    2、如果是非数值运算，则会先转成数值，再运算

   a = True;---->1

   b=null; ------>0 

#####    3、NaN不和任何值进行比较,返回的都是false

   a = NaN;

   a='123a'

   a = undefined;

   **let** b = -2;

   b = null;

   b = 0;

#####    4、比较的2个值，都是字符串(空也是)，不会转成数值，会按照unicode编码表比较

   此时，需要我们手动的将某个字符串转成数值，再进行比较,   **一位一位的比**

  a='14' 

  b = "131";   1=1   4>3 ,a大,就算有子母也是这样比

   console.log(a >= +b);     True



   b = "131";

   a = "a"   a>1   65 97

   console.log(a <= +b);     false



## 08.相等运算符

== 相等      = = =全等       != 不相等       !== 不全等

   最终关系成立，返回true，否则返回false

   **let** a = "1";      #**相等运算符，会进行类型转换，只比较值，不比较类型**	

   **let** b = 1; 		#**全等运算符，不会进行类型转换，比较的是值和类型**

​    console.log(a == b);    T

​    console.log(a != b);     F

​    console.log(a === b);   F

​    console.log(a !== b);    T

​    特殊情况：

#####     **1、undefined == null  ==>true**

​		  ***undefined是null衍生出来的,所以值相同,但是类型不同***

​          undefined === null  ==>false

#####    2、NaN和任意值做相等运算，结果都是false，包括它自己

​    a = NaN;

​    b = null;

​    b = NaN;

#####    3、null和0 做相等运算，结果是false，null跟0做比较时，并没有将null转成数值0

   a = null;

   b = 0;

   console.log(a == b);  ffFFF

​    console.log(a === b);  fFFF

##### 面试题：

== 与=＝=的区别是什么？
都可以做值的相等运算，== 会做类型转换，= = =不会，===要求值和类型完全相等







## 09.三元运算符/条件表达式

三元表达式:只适用于简单的语句结构，不可以写多个语句

​    **let** a = 1;

​    **let** b = 2;

   语法结构:     **条件表达式？语句1:语句2**     true执行语句1,false执行语句2

​    a < b ? alert("hello") : alert("hi");     >>>>>hello

   用户输入年龄，如果年龄>18岁，可以去网吧，否则回家好好学习

​    **let** age = prompt("请输入你的年龄");

​    age >= 18 ? alert("去网咖") : alert("回家好好学习");



   **用三元运算符比较三个数的大小**

   **let** a = 100;

   **let** b = 200;

   **let** c = 30;

   *//比较a和b的值，将较大值返回，赋值给max*

   **let** max = a > b ? a : b;

   *//再比较max和c的值，将较大值返回，更新max变量*

   max = max > c ? max : c;

   console.log(max);

   **嵌套**

​    **let** age = prompt("请输入你的年龄");

​    age>=18?(age>30?alter("回家"):alert("去网咖")):alert("回家好好学习");



## 10.优先级

   *// 如果你的运算非常的复杂，在写的时候，就加上()*

​    **let** res=1+2*3%(2-(10||2)&&1)

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260805112525157.png" alt="image-20260805112525157" style="zoom:50%;" />







# 0806流程控制

## 01.顺序结构

它**没有特定的语法结构**，程序会按照代码的先后顺序，依次执行

#### 代码块

代码块 {}，代码块内的代码，要不然一起执行，要不然都不执行

   块级作用域，es5的语法是没有块级作用域的，es6是有块级作用域的

   {

​    console.log(1);

​    console.log(1);

​    console.log(1);

​    **var** a = 1;

​    console.log(a);

​    **let** b = 1;

​    console.log(b);

   }

   console.log(a);

   console.log(b);*//报错*



## 02.分支结构

#### 	三元表达式

​	用户输入数字，如果数字小于10，则在前面补0，比如01，如果数字大于10，则不需要补，如20

   **let** num = prompt("请输入数字");

   num = num >= 10 ? num : "0" + num;

   alert(num)



#### 	if语句

条件表达式结果为true，则执行紧跟其后的代码块，为false，则不执行



​    **if (条件表达式) {**

​     **代码块;**

​    **}**



​    **let** str = "hello";

​    if (str.length < 6) {

​     console.log(1);

​     console.log(2);

​     console.log(3);

​    }





#### 	if_else语句



if（条件表达式）{

 		执行语句1....

}else{

 		执行语句2....

 }



#### 	if_else if_else语句

if(条件表达式){

 		执行语句....

}else if(条件表达式){

 		执行语句....

}else{

 		上述都不成立，执行语句

 }

else不写条件

#### 	switch_case语句

switch专门用来做特定值的比较，而且语法中有break，可以提高代码性能

   特定值的多分支结构

**switch_case做的,if  else_if也可以做, 但是特定值的判断用S_C性能更好,范围用if  else_if**

switch会将条件表达式和case后的值进行**全等比较**

switch(条件表达式){

 		case 表达式:
 	
 			执行语句....；

​		 		   break;

 *//break,一旦比对成功，对应语句执行完毕，代码就中断，不会继续往下运行*

​			 case 表达式:

 			执行语句....
 	
 					break;

​			 ······(无数case)

 			default:
 	
 			执行语句...；

​			 }



## 03.循环结构

### 	for循环

for循环就是for循环

记个蛋笔记呢

有啥好记的

你就是个废物



**for(初始化表达式;条件表达式;更新表达式){**

 **循环体....**

 **}**

****



### 双重for循环

单层for循环

   for(**let** i=0;i<5;i++){

​     console.log('外层循环',i);

   }

   双重for循环

   嵌套在内部的循环是外部循环的循环体

   外层循环，循环一次，内存循环，循环一轮

​    for (**let** i = 0; i < 5; i++) {

​     console.log("外层循环", i);

​     for (**let** j = 0; j < 5; j++) {

​      console.log("内存循环", j);

​     }

​    }



### while循环

while循环一定要有终止条件，否则会死循环

​    **let** i = 0; *//初始表达式*

​    while (i < 5) {

​     *//条件表达式*

​     console.log("执行while循环");

​     i++; *//更新表达式*

​    }

​    for(**let** j=0;j<5;j++){

​     console.log('执行for循环');

​    }

   当你不知道循环次数时，可以用while循环

   需求：用户必须输入5，才停止循环，否则一直让用户输入循环

​    **let** num = prompt("请输入数字");

​    while (num != 5) {

​     num = prompt("请输入数字");

​    }

   强制表白，用户必须要输入，我爱你，退出循环，否则，一直用用户输入

   **let** str = prompt("你爱我吗？");

   while (str !== "我爱你") {

​    str = prompt("你爱我吗？");

   }





### break和continue的使用

​    **let** str = prompt("你爱我吗？");

​    while (true) {

​     *// 加终止条件*

​     if (str == "我爱你") {

​      break; *//退出循环*

​     }

​     str = prompt("你爱我吗？");

​    }

​    for (**let** i = 0; i < 5; i++) {

​     if (i == 2) {

​      *//  break; //退出循环*

​      continue; *//跳过本次循环，继续下一次循环*

​     }

​     console.log(i);

​    }

​    输出1-100之间的数，除了10的倍数

​    for (**let** i = 1; i <= 100; i++) {

​     if (i % 10 == 0) {

​      continue;

​     }

​     console.log(i);

​    }

   break结束指定循环

   名字:for循环

   outer: for (**let** i = 1; i <= 5; i++) {

​    console.log("外层循环", i);

​    for (**let** j = 1; j <= 5; j++) {

​     if (j == 2) {

​      *// break ; //结束的是离它最近的for循环*

​      break outer; *//结束指定的for循环*

​     }

​     console.log("内层循环", j);

​    }

   }







### do_while循环

先做再循环

while循环和do_while循环的区别，while可能不执行，do_while循环至少执行一次

​    **let** str = prompt("你爱我吗？");

​    *//当首次str=='我爱你'时，while就不执行了*

​    while (str !== "我爱你") {

​     str = prompt("你爱我吗？");

​    }

​    do {} while (true);

​    **let** str;

​    do {

​     str = prompt("你爱我吗？");

​    } while (str !== "我爱你");

   要求用书输入0-5之间的数字，只有当用户输入5的时候，才退出循环

   **let** num;

   do {

​    num = prompt("请输入数字");

​    if (num == 5) {

​     break;

​    }

   } while (true);

















  
