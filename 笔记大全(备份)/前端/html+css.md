## 准备工作

### vscode--安装插件

```
chinese(Simplified)  设置简体中文

chinese lorem : 输入jw,按enter,默认生成128随机假文
			       jw+数字+enter,生成对应数字假文
			      lorem+数字+enter,生成对应数字数量的英文单词

live server:开临时服务器,在服务器上运行网页,端口5050

preview  on web server:开临时服务器,在服务器上运行网页,端口8000(双重保险)

auto close tag:自动闭合标签

auto rename tag:重新修改名字(同步修改)

code running:
```



### 设置和快捷键:

```html
Editor: Font Size/ 代码大小

Auto Save---afterDelay 自动保存代码

Editor: Format On Save 保存时自动格式化代码

!+tab生成框架

ctrl + / 注释(同python):生成注释框

shift+alt+上下:复制整行整列

shift+alt+f:代码自动复位
```

```html
html语言：超文本标记语言，主要是用来搭建网页的结构、框架 ==> 盖毛坯房
css语言：层叠样式表，主要是用来对网页结构进行装饰 ==>装修
js语言：网页的交互（我给你一个行为，你给我一个反馈）==>智能家具
```

# HTML基础

## 语法介绍

### 标签语法:

​	单标签(自结束标签)    :   <标签名   属性1="属性值",属性="属性值">

​	双标签     :<开始标签名></结束标签名>

## 框架介绍

```html
<html></html>根标签/所有标签的祖先标签

<!-- 后面写的所有代码，都要在根标签内，一个页面只能有一个根标签
lang属性=“en属性值”>>>语言=“英文”，默认就是英文，所以其实lang属性可以省略 -->
```



```html
<!--head标签-->
<head></head>   头标签

<!--网页标题:title标签是html语言中，语义（作用/意义）最重的一个标签，可以帮助浏览器检索页面-->
<title> 超哥的网页</title>

<!--设置网页编码-->
<meta charset="UTF-8">

<!--配置移动端视口-->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!--link标签可以加标题图片-->
<link href="//www.jd.com/favicon.ico" rel="icon"></link>
```



```html
<!-- body标签-->
<body></body> 网页主体,主要书写内容
```



## 标签之间的关系

仅存在父子,祖先后代,兄弟关系

#### 父子关系:直接包裹和被包裹的关系

​	父标签（/父元素）：直接包裹子标签的元素
​	子标签（/子元素）：直接被父标签包裹的元素

例如,html根标签是head,body的父标签,head是title的父标签

#### 祖先后代关系：直接或间接包裹和被包裹的关系

​	祖先标签（/祖先元素）：间接包裹后代标签的元素
​	后代标签（/后代元素）：间接被祖先标签包裹的元素

例如,html是title的祖先标签,只要包括在内就是祖先后代关系,父子关系也是祖先后代关系

#### 兄弟关系：拥有共同的父元素的标签

​	兄元素、弟元素

代码从上到下解析,上面的是兄





## 标签的分类

1、形式分：**对于开发来说，没有什么作用**

​	单标签：如meta
​	双标签：如title

2、特点分

### 	块标签：主要用来对网页划分区块（/框架/区域）

​	例如:**div**

特点 1、**独占一行**,就是每个div的内容在网页内独占一行,不设置宽度会铺满整行
        2、可以设置宽高
        3、如果不设置宽度，宽度默认是父元素的100%，如果不设置高度，高度会被内容撑开,就是高度由内容决定

常用的块标签：div,header,main,footer,aside,section,(布局与结构标签)
					     ul,li,ol,dl,dt,dd,(列表标签)
						 p,h1-h6,center,hgroup······(文本与标题标签)

```html
<!-- 设置大小，可以通过css去设置，给开始标签/自结束标签.添加一个style属性，在style属性值的位置
  可以写css 
  css的语法结构
  样式名:样式值;
  width: 400px; 设置宽度
  height: 100px; 设置高度
  background-color: red;设置背景  -->

<div style="width: 400px; height: 100px; background-color: red">
      Lorem ipsum dolor sit amet.
</div> 
```



### 	行内标签：主要用来包裹具体内容

用来修饰局部文字

​	例如:**span**

 特点1、不会独占一行
         2、不可以设置宽高
         3、宽高是被内容撑开的
 常用的行内标签：span,em,strong,del,i,u,b,a······ 





### 	行内块标签：替换元素，每个标签都有自己独特的作用

​	例如:**img**

特点1、兼具行内标签和块标签的特点
       2、可以设置宽高，不会独占一行
       3、标签与标签之间会有3像素左右的空白间距  
常用的行内块标签：img,input,button·····







## 块标签

### 01.结构类块标签

**学习标签从两个角度:1.语义  2.默认样式,尽量选择合适的有语义的标签**

##### div

​	**语义**:没有特殊语义,只是用来画框架(划分区块)

​	div虽好，但不要滥用,如果有更合适的块标签,优先用合适的

​	**默认样式**:没有默认样式 

*在写html时,用header,main,footer搭建网页主要结构框架比div更准确,性能更好*

***header,main,footer每个网页只能用一个***

##### header(头)

​	**语义**:网页的*头部*

​	**默认样式**:没有默认样式

##### main(身体)

​	**语义**:网页的*主体*

​	**默认样式**:没有默认样式

##### footer(腿)

​	**语义**:网页的*底部*

​	**默认样式**:没有默认样式

##### aside(侧边栏)

​	**语义**:跟主体相关侧边的内容

​	**默认样式**:没有默认样式

##### section(新标签)---同div

​	**语义**:同div

​	**默认样式**:同div

##### article(文章类)

​	**语义**:;所有文章类的区块都可以用他

​	**默认样式**:没有默认样式



### 02.其他常用的块标签

##### h1-h6(标题标签))

​	**语义**:表示标题，所有有标题的结构，都用标题标签包裹

​    语义从h1到h6逐步降低的，也就是说h1的语义最重，h6的语义最轻

​    常用的是h1-h3,

​    h1，它是html标签中，紧次于title的语义

​    由于h1的语义比较重，所以一个页面只用一次，对于网站的主页来说，

​    ***h1一般给页面的logo使用***

​	**默认样式**:字体有加粗效果、字体大小有变化，h1到h6,字体逐步变小，上下有约20像素左右的间距



##### p标签(段落标签)

​	**语义**:段落，所有段落的部分都可以用P标签

​	**默认样式**:上下有16像素的间距（外边距）

​	注意:p标签一般只能用来包裹文本，包裹图片，不能再继续放块标签,如果放了,浏览器解析后会变成兄弟标签,因为帮你补齐帮你补齐p标签两边

##### center

​	**语义**:居中块标签，所有包裹在center内的内容都会居网页中显示 

​	**默认样式**:没有默认样式

##### hgroup

​	**语义**:给标题分组,其实和div什么的差不多,只是为了实现代码更加优雅

​	**默认样式**:没有默认样式





##### 列表标签:ul,li,ol,dl,dt,dd

列表使用：创建列表，在列表内，写**列表项(li标签)**

使用场景：在一个大区块内，分若干个小区块，这小区块它的结构、样式、语义雷同  

**列表默认样式**：列表上下有16像素的外边距，列表左侧会有40px的内边距,列表项前会添加项目符号，有序列表，默认是：1，2，3。无序列表，默认是黑心小圆点

​	有序列表(ol):有顺序的列表

​	无序列表(ul):没有顺序要求的列表

​	定义列表(dl):下定义(dt)与解释(dd)的列表

​        定义列表的默认样式：上下有16px的外边距，dd左侧会有40px的外边距

```html
    <h3>起床三件事</h3>
    <ol>
      <li>拿手机</li>
      <li>
        <ol>
          <li>
            <p>给锅放水</p>
          </li>
          <li>
            <div>尝词圣梵瞠罪，司反。</div>
          </li>
          <li>拿鸡蛋</li>
          <li>清洗鸡蛋</li>
          <li>放入锅中</li>
          <li>开火</li>
        </ol>
      </li>
      <li>刷牙</li>
      <li>洗脸</li>
    </ol>
    <h3>超市买东西</h3>
    <ul>
      <li>牙膏牙刷</li>
      <li>水果</li>
      <li>纸巾</li>
    </ul>
    <h3>课程介绍</h3>
    <dl>
      <dt>下定义</dt>
      <dd>内容解释1</dd>
      <dd>内容解释2</dd>
      <dd>内容解释3</dd>

      <dt>AIGC课程</dt>
      <dd>文生图</dd>
      <dd>图生图</dd>
      <dd>文生视频</dd>
      <dd>图生视频</dd>
      <dd>融生视频</dd>

      <dt>AI大模型</dt>
      <dd>python</dd>
      <dd>各种框架</dd>
    </dl>
```

**列表使用注意：**

​        1、列表的默认样式，一般我们要先去除，再使用，哪怕列表的默认样式要用到，我们也是先去除，再自己设 ,去除方式:style="padding: 0;margin: 0;list-style: none;",去除了,数字和点会消失

​        2、列表可以嵌套使用

​        3、最常用的是有序列表和无序列表，但在实际开发过程中，

​         一般不做具体区分，两种列表都能用

4、三种列表都有固定搭配，不要在列表中添加其他的直接子元素  
		ul li，
	    ol  li
	    dl dt  dd





## 行内标签

### 01.常用的行内标签          

##### span标签

​	**语义**:无具体语义,只用来包裹具体文本内容,让你可以选中这部分文本

​	**默认样式**:无



##### strong标签

​	**语义**:加粗强调，强调内容

​	**默认样式:加粗(b  标签也是,但是b有语义)**



##### em标签

​	**语义**:斜体强调，强调语音语调，情绪的强调

​	**默认样式:斜体(i  标签也是,但是i没有语义)**

```html
<em>9999</em>
```



##### hr标签

​	分割线,单标签

```html
<hr>
```




##### del标签

​	语义:表示划掉的语句

​	默认样式:文本中间会有删除线

```html
<del>9999</del>
```



##### u标签

默认样式:在文本下方添加下划线

```html
<u>99好多天9</u>
```



##### br标签

文本强制换行,单标签



### 02.超链接a标签

a标签语义:特殊的行内标签，里面可以包裹任意内容,可以是文本，可以是段落，可以是图片，可以是区块,不可以是自己

**超链接的功能：**

   1、从一个页面跳转到另一个页面

   2、当前页面不同位置跳转

   3、点击连接服务器，实现下载



**超链接的属性**

   1、href属性：  连接的是超链接跳转的路径地址

​     绝对路径:https://www.douyin.com

​     相对路径：./  ../

   2、target属性：控制新页面的打开方式

​    _self : 当前页打开新页面

​    _blank : 打开一个新的页面  

```html
<!--语法-->
<a href="" target=" "></a>

<a href="https://www.baidu.com/" target="_blank">百度</a>
```

**空链接：还没想好，具体跳转到什么位置，就先写空链接**

```html
<!--写法一：打#占位，但这种写会刷新页面，滚动条回到顶部-->
<a href="#">111</a>

<!--写法二：<a href="javascript:;"></a>,不会刷新页面-->
<a href="javascript:;">111</a>
```



### 03.a标签锚点功能

锚点功能（楼梯导航）：

```html
<!--步骤1：给你要去的位置打一个id属性，起一个id属性值，作为标记-->  
如:   
<p id="center">

<!--步骤2：在超链接的href属性值的位置：#id属性值-->   
<a href="#center">点击跳转到中间</a>

<!--如果直接写#，默认会刷新页面，重新开始，所以滚动条回到最顶部,实现跳转-->	    
<a href="#">点击回到顶部</a
```









## 行内块标签

### 01.常用的行内块标签

#### img标签

img标签，给页面引入图片

```html
<img src="./img/02.智能锁.webp" alt="智能锁" width="200/>
```

##### src：指向图片的路径

```
绝对路径：不会变的地址，固定的地址，无论你在哪个html文件中引入图片，都是这个地址

相对路径：不固定的地址，在不同的html文件中，图片的地址可能不一样

只有两种符号
./  : 当html文件和你要引入的图片，在同一个文件目录下， 则用./开头
默认是./开头，所以./也可以省略

../ : 当html文件和你要引入的图片，不在同一个文件目录下，且需要跳出当前文件夹，

来到上一级文件下,如果上一级目录下找不到，可以再../跳出，直到找到目标元素或者目标元素所在的文件夹,写法就是../../
```



#####    alt：给图片添加文本描述

```
关键字，一般情况下，alt属性值是看不到，
   当图片引入不成功的时候显示
   作用1：给用户良好的提示，图片没引入成功，图片是什么
   作用2：帮助浏览器检索图片，提高网站的性能
```



#####    width ：设置图片的宽度

#####    height：设置图片的高度

```
一般我们不会同时设置图片的宽度和高度，防止图片变形
只需要设置宽度或高度，另一边浏览器会同比例调整
```



#### audio标签

​	**语义**:引入音频

​	**属性**：

```
src位置:绝对地址,相对地址      
controls:控制音视频是否可以被播放，默认是false，音频如果是false，默认是不显示音频(controls后不跟属性值，如果需要这个属性就添加，不需要，就去除，没有属性值)
loop  循环播放,无属性值  
autoplay 已经被废弃了（自动播放）
```

```html
<audio src="./source/达拉崩吧.mp3" controls loop></audio>
```



#### video标签

​	**语义**:引入视频

​	**属性**:同audio

```html
<video
      src="./source/绝地逢生.mp4"
      style="width: 200px"
      controls
      loop
    ></video>
```





### 02.按钮标签

##### 	button(双标签)

###### 		属性

```
type属性:确定按钮的类型
属性值可选:
submit  提交  默认值
reset   重置
button  普通按钮，没有语义
```

###### 		实例

```html
<button type="submit" style="width: 100px;height: 50px;">提交按钮</button>
<button type="reset">重置按钮</button>
<button type="button" >普通按钮</button>
```



##### 	input(单标签)

###### 		属性:	

```html
同button,但是有个value属性是在按钮上写字,因为这是单标签
```

###### 		实例

```html
<input type="submit" value="提交按钮">
<input type="reset" value="重置按钮">
<input type="button" value="普通按钮">
```



## 表单标签

### form标签(块标签)

```apl
form标签是块标签,在浏览器里会独占一行，宽度默认撑满父容器，非常适合用来包裹整个表单区域（比如一个完整的登录框、注册区）,  但它内部的零件是行内块标签
```

语义:创建表单

action属性:表单提交给目标服务器的地址,(相对路径,绝对路径)

```html
<form action="./04.target.html">  表单零件等等  </form>
```

form标签是否有点抽象呢,其实是为了将用户填写的整体信息全部提交到服务器中,不然不写form和写了form在最终呈现样式上是一样的



#### 1.input：用户输入数据的地方

除了当按钮,还可以用来进行用户输入

表单项：用户输入数据的地方

```apl
type属性：决定了input的类型
可选值：
text ：文本框 
password ：密码框(输入后不可见)
radio : 单选框,如果想实现单选，需要给单选按钮设置同样的name属性值
checkbox ：复选框，可以选择多个
还有三个按钮选项
```

```apl
name属性：提交给服务器的数据的名字(键)
如果想实现单选，需要给单选按钮设置同样的name属性值
```

```apl
value属性：提交给服务器的值
如果设置value属性值，就是设置默认提交给服务器的值,该值会默认填写,这个和placeholder是不一样的,这个是真实填上去了
```

```apl
checked属性：是否默认选中
不需要设置属性值，需要选中就直接写个checked属性，不需要就不写
```

```apl
placeholder：输入框的提示信息
readonly: 只读属性:这个不能使用,只能读
disabled：禁用:禁止使用
```



#### 2.select: 下拉框option

```apl
select: 创建下拉框，设置name属性
option: 下拉框的具体内容，设置value属性
```

```
可选属性:
select : name属性
option : value属性  selected属性 : 默认选中

设置name实际上是键,value是值,
选中下拉框后提交到目标服务器上实际是name=value,
如下:当你选中专科时,提交上去就是学历=专科,
input标签内有name和value属性,他一条就可以完成键值对应操作
```



#### 3.文本域: textarea

​       cols="40" :设置多少宽，文本域的宽度
​       rows="10" ：设置多少行，文本域的高度

```html
<textarea name="int" cols="10" rows="1"></textarea>
```



#### 4.按钮标签:  input  button

上面

```html
<!--表单示例代码-->
<form action="./04.target.html">
    用户名： <input type="text" name="username" value="张三" /><br />
    密码：<input type="password" name="password" checked placeholder="123" /><br />
    性别：男 <input type="radio" name="gender" value="boy" checked /> 
         女 <input type="radio" name="gender" value="girl" />  
    	 无 <input type="radio" name="gender" value="other" disabled /><br />
    兴趣爱好：跑步：<input type="checkbox" checked /> 
   		     跳舞：<input type="checkbox" checked />
   		     游泳：<input type="checkbox" />
    		 读书：<input type="checkbox" /><br />
    学历：<select name="=学历">
      		<option value="gz">高中</option>
      		<option value="zk">专科</option>
      		<option value="bk" selected>本科</option>
      		<option value="ss">硕士</option>
    	 </select><br />
    <textarea name="int" cols="10" rows="10"></textarea><br />
    <button type="submit">提交</button>
</form>
```





## 表格标签

```apl
table创建表格(块标签)
tr表示一行
th是表头单元格
td表示一个单元格
```

```html
<table border="1">
      <tr>
        <td>序号</td>
        <td>姓名</td>
        <td>性别</td>
      </tr>
    <tr>
        <td>1</td>
        <td>唐僧</td>
        <td>男</td>
      </tr>
      <tr>
        <td>2</td>
        <td>孙悟空</td>
        <td>男</td>
      </tr>
</table>
```



## html结构搭建的基本原则和要求

### 结构搭建的基本原则：

```
1、块标签可以包裹任意标签，块，行内，行内块皆可
2、行内标签一般只包裹文本
3、特殊的块标签：p，它里面不能放块标签,包括自己也不行
4、特殊的行内标签：a，它里面什么都能放，除了自己
```



### 搭建结构要求

```
一个稳固的，优雅的，合理的，好的结构
必须：有区块、 有层次、简洁 
   技巧：
   从整体到局部，
   从大到小
   从上到下
   从左到右
```



# css层叠样式表

html/css/js是三种不同的语言，虽然他们一般搭配使用，所以他们的开发环境是不一样,可以创建对应的后缀名文件

css学习:

```
css的书写位置 : 3种写法
css的选择器 : 50-80种，我们学常用的选择器30种左右
css的具体样式 : 字体文本样式，背景样式，动画样式，改变布局的方式等等样式
```

```apl
格式   样式名:样式值;   一定要写分号
如	  color:red； 设置字体颜色 
	   background-color:red;设置背景色
        width:100px;  设置宽度
	   height:100px;设置高度
	   font-size:30px;设置字体大小,html默认的字体大小是16px
```



## css的书写位置 : 3种写法

### 第一种写法:行内样式/内联样式

**概念:**在开始标签或自结束标签中，添加一个style的属性，将css样式书写在style的属性值的位置,就是之前用的方式

​	可以写多组css样式，样式与样式之间，要用;隔开

**优点：**标签明确，谁要设置样式，我就给谁添加style属性，不用单独选中标签

**缺点：**

1、结构和样式耦合(混在一起了)了，不美观
2、不方便后期维护，不好找
3、修改不方便
4、优先级比较高，后期通过js修改样式，不好修改



**总结：**内联样式，尽量少用

关于下一节的选择器的优先级,内联样式的优先级是10000,最高的

```html
<!--示例-->
<div style="
        width: 200px;
        height: 200px;
        background-color: orange;
        font-size: 20px;
        color: white;">
        书措子逃你导有内动恼若皇统法韩，希归弄。
</div>
```







### 第二种写法:内部样式

**概念：**在head标签内,添加style子标签,一旦写了style标签,就***开辟了一个css的环境***

```
注意事项:
在style标签体中，可以通过选择器(非常多)选中对应的结构， 在{}内，书写css样式

style里面是css,规则都是css的,比如出现的注释就是css的而不再是html的

html哪里都可以写style标签,但是写在head里面最规范,最多最多最多body的第一个子标签写一个style
```

```
优点：一定程度上弥补了行内样式的缺点，修改比较方便了
缺点：1、不方便复用，复制不是复用
      2、会导致你的html文件非常冗余
```

**总结：**如果页面相对比较简单，且不考虑后期复用，可以用内部样式

```html
<head>
	<style>
      /* 标签选择器：通过标签名选中对应的结构 */
      div {
        width: 200px;
        height: 200px;
        background-color: orange;
        color: white;
        font-size: 20px;
      }
	</style>
</head>
<!--这样html里的所有div标签都用这个样式了-->
```






### 第三种写法:外部样式表

**概念:**在html文件的外部,新建一个.css的文件,在css文件中,通过选择器选中对应的结构设置样式 (同上,在{}里面写样式),再将css文件**引入**到当前的html文件中

**引入方式**

```html
<!--引入方式一：在head标签内添加link子标签引入-->
<link rel="stylesheet" href="./04.css" />

<!--引入方式二：在css环境中(怎么进入css环境看02)，通过 @import url();引入-->
<head>
	<style>
      	@import url("./05.css");
	</style>
</head>
 
<!--引入方式三：在js环境中，通过import '  '  引入(未学)-->
```

优点：方便复用，方便维护
总结：实际开发，要用外部样式表



### 附加

```apl
面试题：link和@import url()都能引入css文件，他们的区别是什么？

link是标签，写在html环境中，@import url()是css的语法，写在css的环境中
link是将css文件和html文件关联起来（联系起来）
@import url()是将css文件，整个代码复制到当前css环境中
相对来说，link的性能会更高
```

```apl
引入的css文件中,css文件的写法
1.直接用任何原生 CSS 支持的选择器
2..因为已经在css环境中了,可以直接写@import url();再次引入别的css文件样式;
```



## css的选择器

### 01.最常用的四种选择器

#### 1.标签选择器

​    **语法：**标签名{····}

​    **作用：**选中对应标签，标签体的内容

​    **注意：**标签选择器尽量少用，常用的标签就那么多，搭建一个页面结构，必然会重复使用标签，因为他是给所有这种标签设计样式,  使用标签选择器，可能会误选

​    **优先等级：**标记为1(越大越强)

```html
<style>
	h1 {
   		color: red;
   	   }
</style>
```



#### 2 .id选择器

给你要选中的结构(标签)打一个id属性，起一个id属性值

```html
<!--div+#box+enter:快速打上id属性,属性值是box-->
<li id="first" class="pink white">id选择器</li>
```

   **语法：**#id属性值{····}

   **作用：**选中对应id属性值的结构

   **注意点：**

```apl
1、id属性值（名字）：不能以数字开头、不能用中文、不能重复使用,  每个id都是唯一的
2、id选择器一般用于外层框架，内部结构一般不使用
```

​    **优先等级:**标记为100

```html
<style>
#first {
    background-color: pink;
   }
</style>
```



#### 3.class选择器

给你要选中的结构打一个class属性，起一个class属性值

```html
<!--div+.box+enter:快速打上class属性,属性值是box-->
<li id="first1" class="pink white">示例一</li> 
<li id="first2" class="pink green">示例二</li>
```

   **语法：**.class属性值{····}

   **作用:**  选中这一整类class的标签

   **注意：**

```apl
1、class属性值可以被复用,就是把某些标签分类成一种标签,对这类标签统一设计样式,一个类可以有多个标签
2、可以使用多个calss属性值,空格隔开每个值,,一个标签可以有多个类
```

​    **优先等级:**标记为10

```html
<style>
#first1 {
    background-color: pink;
   }

.white {
    color: white;
   }

.green {
    color: green;
   }
</style>
```



#### 4.通配选择器

   **语法：***{···}

   **作用：**选中页面中所有的标签

   **注意：**

```apl
1.通常用来选中页面中所有的标签，给它们去除默认样式
2.还可以和别的选择器一起用,就是对应等级下的全部标签
```

   **优先等级:**等级为0，任意选择器的样式，都能覆盖*,也就是说先执行其他的样式,最后执行通配选择器的全局样式

```html
<!--如何去除所有样式-->
<style>
* {
    margin: 0;	<!--去除外边距-->
    padding: 0;	<!--去除内边距-->
    list-style: none;	<!--去除列表符号-->
    text-decoration: none;	<!--去除链接下划线-->
   }
</style>
```



### 02.4种关系选择器

*关系选择器，通过标签之间的关系，进一步确定要选中的结构*

```html
<!--示例-->
<div id="outer">
   <hgroup id="title">
        <h1>静夜思</h1>
        <h3 class="subtitle">李白</h3>
   </hgroup>
      <ul>
        <li class="bo">床前明月光</li>
        <li>疑是地上霜。</li>
        <li>举头望明月。</li>
        <li class="bo">低头思故乡。</li>
        <p>写的好啊</p>
        <div>写的真是好啊</div>
      </ul>
</div>
<hr />
<div>
      <h3 class="subtitle">文章的标题</h3>
	  <a href="#">先帝创业未半而中道崩殂</a>
      <del>今天下三分</del>
</div>
```



##### 1.子选择器

   **原理：**通过父标签确定子标签的范围

   **语法：**父标签>子标签{······}

   **注意:**

```apl
1.标签选择器,id选择器,类选择器之间在关系选择器中可以嵌套使用,如下:
2.选择器嵌套，一般不超过3层   
```

```html
<style>
	body > div > hgroup > .subtitle {
    	color: red;
   	}
</style>
```



##### 2.后代选择器

   **原理：**通过祖先标签，确定后代标签的范围,包括子选择器

   **语法：**祖先标签 后代标签{······}     #空格

   **注意：**

```apl
能用子选择器，不用后代选择器，子选择器的性能会更高(父子关系也是祖先后代关系)
```

```html
<style>
	#outer .subtitle {
    	background-color: red;
   	}
    #title .subtitle {
        background-color: pink;
      }
</style>
```



##### 3.下一个兄弟选择器

   **原理：**通过兄标签，选中**紧跟其后**的弟标签   

   **语法：**兄标签+弟标签{}

```html
<style>
	.bo+li{
     	color: red;
  	 }  #第二个li有样式
</style>
```



##### 4.下面所有的弟选择器

   **原理：** 通过兄标签，选中下面**所有符合条件**的弟标签

   **语法：**兄标签~弟标签{}

```html
<style>
	.bo~li{
    	color: orange;
    	}
	.bo~p{
    	color: blueviolet;
    	}
</style>
```



### 04.2种复合选择器

##### 1.交集选择器

​      作用：选中选择器1，选择器2,····，多个选择器共同作用的结构(多个选择器选中的标签的交集)

​      语法：选择器1选择器2···{}   #没有空格,直接相连

​      注意：

```apl
1.如果选择器中有标签选择器，标签选择器要放在首位
2.交集选择器不能直接写两个标签名
```

```html
<style>
li.bo {
    color: red;
   }
</style>
```



##### 2.并集选择器（群组选择器）

​      作用，选中选择器1，选择器2···，所有选择器对应的结构(多个选择器选中的标签的并集)

​      语法：选择器1,选择器2···{}   #逗号相连

```html
<style>
	h1,h3,.bo {
    	color: orange;
   	}
</style>
```



### 05.属性选择器

常见属性:id,class,src,href,alt,constrols,autoplay,checekd,name,type

补充title属性:

​	鼠标悬停在有该属性的内容上时,显示设置的属性值

**属性选择器：**通过标签的属性及属性值来选中对应的结构	

语法：

```apl
[属性名]{}        选择含有指定属性的元素
[属性名=属性值]{}  选择含有指定属性和属性值的元素
[属性名^=属性值]{} 选择属性值以指定值开头的元素  
[属性名$=属性值]{} 选择属性值以指定值结尾的元素
[属性名*=属性值]{} 选择属性值含有某值的元素
```

```html
<head>
    <title>属性选择器</title>
    <style>
      [title*="ab"] {
        color: red;
      }
      [title$=ab]{
        background-color: aquamarine;
      }
      [class]{
        font-size: 30px;
      }
    </style>
  </head>
  <body>
    <h1 title="a" class="p">满江红·写怀</h1>
    <h3 title="ab" class="sub">岳飞·宋</h3>
    <p>····</p>
    <p title="abc" class="p2">靖康耻，犹未雪。臣子恨，何时灭！</p>
    <p title="abcdab">驾长车，踏破贺兰山缺。</p>
    <p>壮志饥餐胡虏肉，笑谈渴饮匈奴血。</p>
    <p>待从头、收拾旧山河，朝天阙。</p>
  </body>
```



### 06.伪类选择器

#### 一、伪类定义

**概念:**不是固定的类，而是类的某种状态，当这个状态满足时，则类生效，否则类不生效）

**比如：**第一个元素，被点击的元素，鼠标移入的元素...

**语法结构：** :伪类名     #一般和别的选择器进行交集复用



#### 二、顺序伪类：

```html
<!--
1、 :first-child  选择父元素中的第一个子元素
2、 :last-child   最后一个子元素
3、 :nth-child()  选中第n个子元素
-->

<!--
nth-child特殊值： 
n   所有的
2n或even  选中所有偶数
2n+1或odd  选中所有奇数
-->

<!-- 比如 -->
<style>
	li:first-child{
		  color: red;
		}	
</style>
<!--含义:li标签和li的父元素的第一个子元素的交集-->
```

```apl
1、:first-of-type
2、:last-of-type
3、:nth-of-type() 

含义:li标签和li的父元素的所有第一个子类型的交集
在一个父元素里，把所有的“同一种标签”挑出来，然后选中其中的“第一个”

功能跟上面的first-child相似
```

```html
<!--:first-child和:first-of-type区别:-->

<!--
first-child要作为第一个子元素,选的是这个元素,
first-of-type需要作为第一个子类型,选的也是这个类型
-->

<div>     
    <p>段落1</p>     
    <p>段落2</p>     
    <span>文字</span> 
</div>

p:first-child {
    color:red;
}    #结果段落一变红:p标签和div的第一个孩子

换一下顺序

<div>
    <span>文字1</span>
    <span>文字2</span>
    <p>段落1</p>
    <p>段落2</p>
</div>  

<!--p:first-child结果不生效,因为这时div的第一个孩子变成了span,和p没有交集-->

p:first-of-type {
    color:red;
}  
<!--结果段落一依旧变红,他取出了第一个span和第一个p,和p取交集-->


<!--p:first-of-type一定能取到内容,first-child不一定(有该标签的情况下)-->
```



#### 三、否定伪类

:not()   将符合条件的元素从选择器中去除

```html
<style>
	li:not(#l1){
   		color: red;
    	}
 </style>
<!--除了id=l1的li全部变红-->
```



#### 四、用户交互伪类

```html
<!--示例代码-->
<body>
    <h1>自答是过气样，知极。</h1>
    <a href="https://www.baidu.com">百度</a>
    <a href="https://www.baidu123456.com">百度</a>
  </body>
```

**超链接a标签独有的** :  link  ,  visited

##### 1. :link  

用来表示未访问过的链接（正常链接）

```html
<style>
	a:link {
    	color: red;
   	}
 </style>
```



##### 2:visited 

用来表示访问过的链接,由于隐私的原因，所以visited只能改颜色,设置别的无效

```html
<style>
a:visited {
        color: green;  <!--只有颜色生效-->
        font-size: 100px;
        background-color: red;
      }
 </style>
```



**所有标签都可以用**  :  hover   active

##### 3. :hover

 用来表示鼠标移入的状态

```html
<style>
	h1:hover{
    	color: red;
    	background-color: green;
   	}
 </style>
```

鼠标放在h1这个标签上,h1会出现的效果样式



##### **4 :active**

  鼠标点击后的状态 ,鼠标不点击了，效果就没了

```html
<style>
	h1:active{
    	color: orange;
   	}
 </style>
```

鼠标点击h1这个标签上,h1会出现的效果样式



##### 5. 鼠标移入显示与隐藏效果实现

```html
<style>
.box {  <!--设置盒子的大小-->
    width: 200px;
    height: 0px;
    background-color: red;
    transition: 1s;     <!--设置过渡效果为1s：使样式的值缓慢的变化，不要一下子完成-->
   }

.outer:hover > .box {    
    height: 300px;
   }
<!--当鼠标放上去的时候,盒子出现,并设置盒子的高度,表示只有 .outer 的直接子元素 .box 才会响应动画，不会误伤到更深层嵌套的盒子。-->
</style>
```

加hover选择器实现效果,outer这个整体加:hover,这样盒子出现时放到盒子上时也不会消失

并且我们用父子关系选择器选中div设置了盒子的高度

```html
<body>
	<div class="outer">
      	<button>移入显示/隐藏</button>
      	<div class="box"></div>
    </div>
</body>
```





### 07.伪元素选择器

*伪元素，*

**概念:**表示页面中一些特殊的并不真实存在的元素（比如元素的位置）

##### 语法： 

```apl
::伪元素选择器名
::first-letter  表示第一个字母
::first-line  表示第一行
::selection  鼠标选中该内容时样式
::before  元素的开始位置
::after  元素的结束位置
```

​        *before和after必须要结合content使用* ,*before和after是唯一可以通过css去修改页面结构*的操作

  

```html
<style>
/*第一个字母修改*/
p::first-letter{
            color: red;
            font-size: 32px;
          }    
/*第一行修改*/
p::first-line{
            background-color: pink;
          }
/*选中文字修改*/
p::selection{
            color: green;
          }
    
/*  before和after是唯一可以通过css去修改页面结构 */
/*在p元素开始位置加上你好*/
p::before{
      content: '你好';
     }    

/*在p元素结束位置加上hello*/
p::after{
      content: 'hello';
     }     

</style>
```

```html
<!-- 测试文字 -->
<p>
	sit amet consectetur adipisicing elit. Porro fugiat maiores sit ex expedita beatae, sint quisquam amet quod cupiditate, tempora omnis impedit deleniti, iure fuga illo
quibusdam. Alias, soluta?
</p>

<p>
	这是一个测试文字，选中它看看效果。
</p>
```



### 08.继承

简单,就是你的祖先的元素设置过后,你要是没特意设置样式,你就继承祖先的样式

​      **定义：**为一个元素设置的样式，同时也会应用到它的后代元素上

​      **优势：**方便我们开发，将一些通用的样式统一设置到共同的祖先元素上，子元素的样式都可以获取到样式

**继承的样式，是没有优先级的，任意方式修改样式都会覆盖继承的样式**

##### 继承性

**注意：**并不是所有的样式都会被继承

**可继承：** 跟文字、字体相关的属性，大部分可以继承。

font-size , font-family  , color; 

**不可继承 ：**背景相关的，布局相关等不会被继承

border , padding , margin , width , height , background;



##### 强制继承：inherit

有些属性默认不能继承，但是可以强制：

```
.child {
    border: inherit;
}
```

表示：

> 让子元素继承父元素的 border



### 09.选择器的权重

##### 概念:

用不同的选择器对同一个元素分别进行样式设定,发生样式冲突时，哪个选择器生效由选择器的权重(优先级)决定

**选择器的权重**

```apl
!important   最高优先级    
内联样式/行内样式                 1000  
id选择器                         100
类选择器/伪类选择器/属性选择器     10
标签选择器/伪元素选择器            1
通配符、子选择器、相邻选择器等。如\*、>、+   0000
继承的样式                        没有优先级
```



##### 注意：

​      **1.**比较优先级时，需要将所有选择器的优先级进行相加计算，最后优先级越高，则优先显示样式

​      **2.**(并集选择器)分组选择器是单独计算的

​      **3.**交集选择器是累加计算的

​      **4.**如果优先级计算后相同，此时则优先使用靠下的样式,后写的覆盖先写的。

​      **5.**选择器的累加，不会超过上一级数量级，例如：类选择器再高也不会超过id选择器  量变达不到质变 

​     **6.**一般来说：选择器越具体，优先级越高

​     **7.**!important  慎用,因为会破坏 CSS 管理



rem:根标签html

如果想去掉strong的加粗效果,可以用normal,比如div里,面有span什么的



## css的具体样式

### 01.长度单位

长度单位:常用的6种

##### 1、像素 px

font-size: 14px

​     **绝对单位**，px我们叫程序员像素，也叫css像素

​     在pc端，1个px就是1个物理像素，物理像素就是1个最小的发光小元件，

​     1x1的物理像素，正常情况下，我们是看不到，但我们对元素进行放大，从而看到



**下面这些单位，都是相对单位，会随着某些参考标准变化而变化**  

##### 2、百分比 %

width: 50%;

​				相对于其父元素的大小变化而变化，百分比主要可以做流式布局

​				a的父元素是100px,a设置50%,a就是50px,a的父元素变化,a也变化

##### 3、em:

height: 10em;

​				**相对于当前元素的字体大小(font-size)变化而变化**，如果当前元素没有设字体大小，

​    		    它会继承其祖先元素的字体大小,**直到最后继承根标签(html)的默认字体大小，16px**

​				1em就是1个font-size的大小

##### 4、rem：

height: 10rem;

​			**只相对于根标签(html)的字体大小**(不设置情况下默认16px)变化而变化,无关其他因素

​			1rem就是1个16px的大小

##### 5、vw：

width: 50vw;

​			viewport width  视口宽度:就是屏幕的宽度，**只相对于屏幕的宽度大小变化而变化**,100vw=100%视口宽度，1vw=1%视口宽度

​			*百分比参照父元素,是不固定的,vw参照屏幕,是固定的*

##### 6、vh: 

​			viewport heigth 视口高度,**同vw,宽度变高度** 100vh=100%视口高度，1vh=1%视口高度

##### 注意:

移动端的开发，主要用的单位就是rem和vw

只有px是固定单位,别的都是相对单位

vw,vh,rem相对的对象是固定的

百分比和em是不固定的





### 02.颜色单位

##### 1:颜色单词

**熟悉的就用这个**

color:red

red,yellew,orange,blue,green,white,black,等等，
单词不好记，而且有些颜色不好描述，无法用单词

##### 2:RGB值

color:rgb(255,0,0)

rgb(red,green,blue)
颜色的浓度比值：在0-255之间,红绿蓝不同的比值，勾兑出不同的颜色

##### 3:RGBA

background-color:rgba(255,0,0,0.5);

rgba(red, green, blue, alpha)

**就是RGB加了个透明值**

alpha 透明度  0-1，0表示透明，1表示不透明

##### 4:十六进制的rgb值(常用)

**截图工具选到颜色然后按c复制色号**

background-color:#62D1D2 ;

使用十六进制的rgb值来表示颜色，原理和上边RGB原理一样
#红色绿色蓝色   
只不过这个颜色的浓度比值，会转成16进值得表示方式
 0，1，2，3，4，5，6，7，8，9，a,b,c,d,e,f

##### 注意

###### 像这样两两重复得颜色，我们可以省略1位

#ff0000  红色  ==》#f00

###### 常用的省略颜色

#0f0  #00f  
#ccc  浅灰色
#f60 橙色

###### 透明度样式属性

opacity: 0.5;

50%透明度颜色



### 03.字体的样式

##### 1:字体颜色

color  设置字体颜色,也可以设置其他颜色

##### 2:字体大小

font-size   同长度单位

##### 3:设置字体类型(什么这个体那个体)

font-family可以指定文字的字体

font-family:'Courier New', Courier, monospace;

注意这里你输入一个字体会出现这样很多字体(一个逗号就是一个字体),但是最后一个不是一个具体的字体,而是一类字体,找不到这些字体时会用这最后的字体兜底

###### 字体的分类

在网页中将字体分成5大类：

​     serif  ['serif]（衬线字体）

​     sans-serif（非衬线字体）

​     monospace （等宽字体）

​     cursive ['kə:siv]（草书字体）

​     fantasy  ['fæntəsi]（虚幻字体）

   可以将字体设置为这些大的分类,当设置为大的分类以后，

​    浏览器会自动选择指定的字体并应用样式

   一般会将字体的大分类，指定为font-family中的最后一个字体 ,**用来兜底**

 

```html
.box {
        color: #f00;
        font-size: 2em;
        /* 设置字体类型 */
        font-family:
          "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
      }
```



##### 4:设置自定义字体

就是你需要用自己下载的字体时

首先先保存字体文件(相对路径/绝对路径)

src: url()属性输入路径

```html
<style>
@font-face {
    <!-- 给你的字体起个名字 -->
    font-family: "abc";
     <!-- 自定义字体路径 -->
    src: url(./字体/FuLuLingGanHeChaTi/FuLuLingGanHeChaTi-2.ttf);
   }
<!--引用该字体-->
h1{
	font-family:abc;
}
</style>
```



##### 4.设置文字斜体

font-style: ;

可选值：

italic/oblique：斜体

normal:文本正常显示,去掉斜体



##### 5.设置文字的加粗

font-weight: ;

可选值：

bolder/bold：加粗

100-900 :100最细，900最粗

normal:文本正常显示,去掉加粗



##### 6.font简写(记得看)

p{

​		font-size: 30px;

​		font-weight: bold;

​		font-style: italic;

​		font-family: 'Courier New', Courier, monospace; 

}

###### font简写的要求

**1、必须要写字体大小和字体类型**

**2、字体大小必须在倒数第二位，字体类型必须在倒数第一位**

p{

​		font: bold italic 30px monospace;

}  #这两个写法效果一样,但是简写符合程序员习惯



### 04.行间距(行高)

##### 1、行高（line-height）

###### 怎么理解行高:

​		文字占有的实际高度,**选中这一行文字时,背景颜色的高度** 

###### 行高计算

​		行高=上间距+字体大小+下间距

​        上间距=下间距，文本被挤到中间

###### 字体之间的距离

我们通常通过设置行高，来调整**一行文本与另一行文本之间的间距** ：行高-字体大小>>>>>>>也就是上下间距之和,很好理解

如果字体大小和行高设置的一样,那么行与行之间就没有间距

###### 可选值       

line-height: ; 可选值：

​         (1).xxxpx,  

​         (2)数值  当前字体大小的倍数,**也就是em**，如果当前元素没设字体大小，就继承祖先标签的

​			几就是几em

​         (3)百分比  也可以换算成数值:200%=2



##### 2、如何设置单行文本在父元素中垂直居中

​        给元素设置行高，跟高度一致，即可让**单行文本**，垂直居中

​		因为上间距和下间距一样,文字寄到中间了

​	    **注意是单行文本**

如   

```HTML
h1 {
    width: 50%;
    height: 70px;
    background-color: pink;
    line-height: 70px;  /*设置成一样的*/
    font-size: 20px;
    font-style: italic;
    font-weight: bold;
    font-family: "Courier New",Courier,monospace;
      }    #h1的文字高度就在h1的中间
```



##### 3、font中也可以指定行高(简写)

**写法:**行高写在字体大小后面用/连接

font:字体大小/行高  字体类型;

font: bold italic 20px/80px monospace;

加粗   斜体   字体大小20px/行高80px  字体为monospace



### 05文本样式   

##### 1: text-transform 可以用来设置文本的大小写

只针对单词,对汉字没用

**可选值**

uppercase单词变大写

lowercase单词变小写

capitalize单词（以空格为分隔），首字母大写

none正常显示，默认值



##### 2: text-decoration  可以用来设置文本的修饰

 a,del{

​		text-decoration:none

}   #去除a标签的下划线和del标签的删除线

**可选值**

none  没有文本修饰线
underline   添加下划线
overline   添加上划线
line-through   添加删除线



##### 3: letter-spacing可以指定字符间距 

##### 4: word-spacing可以设置单词之间的距离

letter-spacing和word-spacing一起讲

letter-spacing设置汉字之间的间距

word-spacing设置单词之间的间距

lettel-spacing:20px





##### 5: text-align用于设置文本的对齐方式

**可选值:**center  居中对齐    #和center效果一样

​			left    靠左对齐(默认值)

​			right  靠右对齐

**注意：**text-align有作用的前提：要有文本可以移动的空间

​			例如,a标签这种行内标签设置text-align属性就没有效果



##### 6: text-indent 设置首行缩进

就是首行缩进

设置当前字体大小两个大小的空格    text-indent:2em

样式值：

设置正值，向右缩进，

设置负值，向左缩进

偶尔我们可以用它来隐藏元素，设置一个比较大的负值，保留语义，看不到内容





##### 7: white-space: ; 设置网页如何处理空白

##### 8: text-overflow 文本溢出包含元素时发生的事情

***设置单行文本省略号操作***>>>>>>一行放不下,多余部分用省略号

width:200px  设置宽度

white-space:nowrap   哪怕一行放不下也不会跑到第二行,会突出框架外面

overflow:hidden    裁剪多余的文本(第一行突出框架的)

text-overflow:ellipsis    设置多余的文本,以省略号的形式存在



***设置多行文本省略号操作***>>>>自己探索





##### 9: vertical-align  设置元素垂直对齐的方式

###### 作用1:设置图文对齐方式

​	可选值:bottom>>>图文靠下对齐

​				top>>>图文靠上对齐

​				middle>>>图文居中对齐

​				baseline>>>基线对齐(默认值)  什么是基线,一段文字前面加上xX,xX的底线就是基线,比文字高

###### 作用2:解决图片之间3像素问题

上下两张图片之间会有3像素的空隙

vertical-align:bottom   (写top,middle也行)

这样就没有间隙了





##### 10: text-shadow  设置文本阴影效果

四个参数

h-shadow   阴影的水平位移距离   正负都可以,正向右,负向左

v-shadow   阴影的垂直位移距离   正负都可以,正向下,负向上

blur   阴影的模糊半径,模糊程度,不能小于0,值越大阴影越模糊

color  阴影的颜色 ,默认是字体颜色



box-shadow图片阴影效果

基本一样,阴影颜色不设置默认也是字体颜色



## 盒子模型

可以把盒子模型想象成**网购的快递盒**，由内到外分为 4 层：

### 盒子模型 4层结构

由内到外

1. **Content（内容区）**：里面装的“货物”。
   - **对应属性**：`width` 和 `height`。
   - **说明**：就是文本、图片本身的区域。
2. **Padding（内边距）**：货物和纸箱内壁之间的“防震气泡膜”。
   - **对应属性**：`padding`。
   - **说明**：用来撑开内容与边框的距离，让内容不贴边。
3. **Border（边框）**：快递盒的“纸箱壁”。
   - **对应属性**：`border`（如 `border: 2px solid black;`）。
   - **说明**：盒子的边界线。
4. **Margin（外边距）**：这个快递盒和旁边另一个快递盒之间的“空隙”。
   - **对应属性**：`margin`。
   - **说明**：用来控制**盒子与盒子之间**的距离。注意，`margin` 是透明的，不占盒子本身的面积。



### 案例

```html
<style>
        /* 去除内外边距 */
        * {
            margin: 0;
            padding: 0;
        }

        .father {
            /* 设置盒子自身content */
            width: 800px;
            height: 600px;
            background-color: rgb(157, 255, 0);
            
            /* 盒子与另个一盒子的距离:外边距 */
            /* 复杂写法
            margin-left:100px;
            margin-top: 100px; */
            /* 简写:上右下左,记得这个就行 */
            margin:100px 0 100px 100px;
            /* margin:0 auto;  */
            /* 上下为0 左右自动居中 */
           
		   /* 盒子与内部盒子的距离:内边距 */
		   /* 上右下左,记得这个就行 */
            padding: 200px 0 0 250px;
           
            /* 设置盒子边框:边框 */
            border: 5px solid red;

            /*把padding或者border纳入到宽和高中，让盒子不变形。后面有精讲*/
            box-sizing: border-box;
        }

        .son {
            width: 300px;
            height: 200px;
            background-color: #0f0;
            /* 设置盒子水平居中,但是由于上面的外部盒子设置了内部边距,所以这里无法水平居中 */
            margin:0 auto;
            /* 如果设置内边距和外边距则距离为对应方向相加*/
            /* margin:80px 0 0 80px; */
            border: 5px solid red;
        }
        /* 参照盒子 */
        #new{
            width: 300px;
            height: 200px;
            background-color: #0f0;
            margin:100px 0 100px 200px;
        }
    </style>
    
    
    
<body>
    <!-- margin:外边距（top，right,bottom,left） padding内边距（top，right,bottom,left） border:边框-->
    <!-- 顺时针观看或者操作 -->
    <div class="father">
        11外层
        <div class="son">22内层</div>
    </div>

    <div id="new">
        11外层
        <div>22内层</div>
    </div>
</body>
```



### 盒子模型的方向和简写

`padding` 和 `margin` 都支持 4 个方向：上 (top)、右 (right)、下 (bottom)、左 (left)。
它们都有极其好记的**“简写规则”（顺时针）**：

- **写 1 个值**：`padding: 10px;`（上下左右全是 10px）
- **写 2 个值**：`padding: 10px 20px;`（上下 10px，左右 20px）
- **写 3 个值**：`padding: 10px 20px 30px;`（上 10，左右 20，下 30）
- **写 4 个值**：`padding: 10px 20px 30px 40px;`（上、右、下、左 —— 顺时针方向）



### box-sizing属性

📦 假设我们有一个快递盒：

```html
.box {
    width: 200px;
    height: 200px;
    padding: 20px;          /* 四周内边距 20px */
    border: 5px solid red;  /* 四周边框 5px */
}
```



#### 情况 1：不写 box-sizing（默认值 `content-box`）

- **含义**：你设置的 `width: 200px` 仅仅代表**装货物的空间（Content）**是 200px。
- **结果**：加上内边距和边框，这个盒子在页面上**实际占据的宽度** = `200px (内容)` + `20px*2 (左右内边距)` + `5px*2 (左右边框)` = **250px**。
- **痛点**：你明明写了 200px，结果它把旁边的东西挤走了。如果你的容器刚好是 200px 宽，这盒子直接“爆”出去了。

#### 情况 2：写了 `box-sizing: border-box;`

*把padding或者border纳入到宽和高中，让盒子不变形*

- **含义**：你设置的 `width: 200px` 代表的是**整个盒子的总宽度（包含边框和内边距）**。
- **结果**：盒子在页面上实际占据的宽度**就是死死钉在 200px**。
- **内在变化**：浏览器为了凑够这 200px，会自动把中间“装货物的空间（Content）”压缩。真实内容区宽度 = `200px - 20px*2 - 5px*2 = 150px`。

### border边框属性细讲

```css
<style>
        * {
            margin: 0;
            padding: 0;
        }

        .father {
            width: 800px;
            height: 200px;
            background-color: #f00;

/* 常见边框线条 实线(solid)、虚线(dashed)、点线(dotted) */
            /* border: 4px solid #160081; */
            /* border:4px dashed #160081; */
            /* border:4px dotted #160081; */


            /* 不常见  双实线*/
            border: 20px double #160081;

            
/* 可以分别设置四个边的边框 上右下左  */
            /* border-top: 5px solid #000;
            border-right: 5px dashed yellow;
            border-bottom: 5px dotted blue;
            border-left: 5px double orange; */

/* 用 border-style 一次性写四边:还是上右下左  */
            /* border-style: solid dashed dotted double;
            border-width: 5px 5px 5px 5px;
            border-color: #000 yellow blue orange;  */
/* 和上面效果一样 */


            
/* 常见 圆角魔法（Border-radius）*/
/* 设置成50%时 盒子是正方形为圆  长方形就是椭圆 这个百分比越大,圆角越尖*/
            /* border-radius:50% ;  */

/* 固定设置像素的圆角,你的尺寸（800x200），
上下高度只有 200px，你写 100px，
那么左上、左下角加起来刚好是 200px，它会变成一个“药丸形状” */
            border-radius: 100px;
/*左右两边变为椭圆*/
        }

        .son {
            width: 300px;
            height: 200px;
            background-color: #0f0;
            margin: 0 auto;
        }
    </style>
```

```html
<body>
    <div class="father">
        <div class="son">11</div>
    </div>
</body>
```



### display: inline_block和Margin

1.设置 inline_block就变成行内摆放,同时可以设置宽高,你可以理解成不设置的话,块级元素就是单独一行

2.`inline-block` 布局中一个经典问题,他是按照文字底部的基线对齐的,比如下面的案例,由于块2导致基线被拉低,使得对齐后块134像被拉下来一样,我们可以设置vertical-align: top;使得顶部对齐而不是基线对齐

3.margin之间上下距离是取最大的的,左右是相加的

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .wrapper {
            font-size: 0;
        }

        .item1 {
            background-color: #f00;
            display: inline-block;
            font-size: 16px;
            margin: 0 10px;

        }

        .item2 {
            background-color: #0f0;
            display: inline-block;
            font-size: 16px;
            margin: 0 20px;

        }

        .item3 {
            background-color: #ccc;
            display: inline-block;
            font-size: 16px;
        }

        .item4 {
            background-color: #f60;
            display: inline-block;
            font-size: 16px;
        }

        .item1,
        .item2,
        .item3,
        .item4 {
            width: 300px;
            height: 200px;
            /* 顶部对齐 */
            /* vertical-align: top; */
        }
    </style>
</head>

<body>
    <!-- margin之间上下距离是取最大的的,左右是相加的 -->
    <div class="wrapper">
        213427话都说
        <div class="item1">11</div>
        <div class="item2">2234535353453453434255242423235252</div>
        <div class="item3">33</div>
        <div class="item4">44</div>
    </div>
</body>
```



## 浮动

###  浮动基础

让几个块级元素都浮动起来,变成了可以横向排列的“行内块”(不加就是从上到下垂直排列,单独一行)

```html
<!--设置向右浮动,则靠右排列,如下是4>>3>>2>>1依次摆放-->
<style>
        * {
            margin:0;
            padding:0;
        }
        .item1 {
            background-color: #f00;
            /* float:left; */
            float:right;

        }

        .item2 {
            background-color: #0f0;
            /* float:left; */
              float:right;

        }

        .item3 {
            background-color: #ccc;
             /* float:left; */
               float:right;
        }

        .item4 {
            background-color: #f60;
             /* float:left; */
               float:right;
        }

        .item1,
        .item2,
        .item3,
        .item4 {
            width: 300px;
            height: 200px;

        }
    </style>
</head>
<body>
    <!-- 浮动：左浮动（float:left;）右浮动（float:right;） -->
     <div class="item1">11</div>
     <div class="item2">22</div>
     <div class="item3">33</div>
     <div class="item4">44</div>
</body>
```





### 浮动和inline_block的区别

##### 1. 是否脱离“文档流”（最核心的区别）

- **`inline-block`**：**不脱离文档流**。它就像乖乖排队的学生，依然占着自己的位置，父元素能正常计算它的高度，后面的元素也会规规矩矩地跟在它后面（或者被挤到下一行）。
- **`float`**：**脱离文档流（半脱离）**。它像个气球一样“飘”了起来。父元素算高度的时候看不见它（导致**高度塌陷**），后面的元素会直接顶上来，跑到它的下面（实现图文环绕效果）。

##### 2. 如何处理代码里的“回车换行”

- **`inline-block`**：极其敏感。代码里多一个回车，页面上就多一个空白缝隙（幽灵空白节点）。
- **`float`**：完全无视。你回车敲得再多，浮动元素也是紧紧贴在一起，没有缝隙。

##### 3. 对齐方式（垂直方向）

- **`inline-block`**：默认按照**文字基线（baseline）**对齐。如果里面文字行数不一样，盒子就会高低不平（你之前踩过的坑）。
- **`float`**：默认**顶部对齐**。无论盒子里内容多高，它们总是从最上面开始对齐。

##### 4. 谁会把谁“卡住”（错位现象）

- **`inline-block`**：如果因为宽度不够换行，第二行的第一个元素会自动找好位置，正常排列。
- **`float`**：极其容易“卡住”。如果第一行有个很高的盒子，第二行有个很矮的盒子，当第二行左浮动时，它会被那个高盒子**挡住**，卡在中间，导致排版错乱（俗称“阶梯陷阱”）。

##### 5. 最神奇的专属技能：图文环绕

- **`inline-block`**：做不到。文字只能在它下面，或者被挤走。
- **`float`**：这是它发明的初衷。文字会非常自然地环绕在浮动元素的周围。例如：

# JS编程

## 0729.js的输入输出和书写位置

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



### 0730.变量和变量的声明

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



## 0731.数据类型(简单数据类型)

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
- 显式清空引用类型值（释放内存 / 重置状态/间接可以解绑事件）
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



## 0803类型的转换

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







## 0804运算符

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





## 0805运算符

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







## 0806流程控制

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

















  









































