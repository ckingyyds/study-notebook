# Oracle 数据库笔记


## SQL 基础

### 数据库入门 · 简单查询 / 限定查询 / 模糊查询 / 函数
什么是数据库?
统一存储和管理数据的仓库

分类┌关系型数据库:Oracle        数据的存储与管理查询
    └非关系型数据库:nosql,MongoDB   数据的传输
    
语言:SQL语言--结构化查询语言

学习SQL只要掌握sql的关键字和关键字之间的顺序即可

--增删改查
--查询

#### 一、简单查询
一:简单查询
语法:
```sql
select 要查询的内容
from 数据来源
```

数据来源┌表格
        ├视图
        ├子查询
        ├...

要查询的内容┌字段名    多个字段之间可以用逗号隔开,*代表所有的字段
            ├常量
            ├变量
            ├子查询
            ├函数
            ├...
            
            
数据类型:┌数值型    int|number                    直接写
         	  ├字符型    char|varchar|varchar2        用单引号引起来,引号内严格区分大小写的
         	  └日期型    date|timestamp                date'年-月-日'

万能表:dual
没有自身数据也没有字段的特殊表格,查询什么就返回什么,一般用来进行函数测试

ascii<-->chr互转

别名:小名
┌列别名:  字段名 [as] 别名
└表别名:  表名 表别名

别名的注意事项┌不能数字开头
              			├不能包含符号(除了_)
              			└不能区分大小写,就是如果不加双引号,小写字母会默认转成大写,而汉字一定要用大写

如果必须违反上述规定,直接用双引号引起来
Oracle 的标识符（列别名）默认不能以数字开头，加双引号后变成"带引号的标识符"，才能强制使用数字作为列名。

注释┌单行注释        注释一整行        --注释的内容
       └多行注释        注释多行             /*注释的内容 */




#### 二、限定查询
二:限定查询
```sql
select 要查询的内容
from 数据来源
where 限定条件
```

限定条件:字段 操作符 值

操作符┌比较操作符:  >  <  >= <= = !=
      ├逻辑运算符:  and or not   用来连接多个限定条件
      ├between and操作符:between 小值 and 大值 →  小值..大值 这个范围,包含两端值
      ├in:in(值1,值2...) → =值1 or =值2...
      ├is null: 判断是否为空
      ├算数运算符: + - * /
      └连接符:||  → str1||str2 → str1str2

-- 3.查询入职日期是1981-2-20的员工的名字,入职日期,部门编号
```sql
select *
FROM EMP
WHERE HIREDATE = DATE '1981-02-20'
-- 4.查询10部门1981年入职的人的名字,职位,入职日期 
select *
FROM EMP
WHERE DEPTNO = 10  and HIREDATE >= DATE '1981-01-01' and HIREDATE < DATE '1982-01-01'
```

空:空是客观存在的,不占用任何的空间,没有实际的数值,空和任何数计算结果一定为空
   在比较大小时,比大则空最大,比小则空最小

优先级
1.小括号
2.乘除
3.加减、连接符
4.比较符
5.is NULL 、 IN
6.between AND
7.not
8.and
9.or




#### 三、模糊查询
三:模糊查询   特殊的限定查询
```sql
select 要查询的内容
from 数据来源
where 字段 like '匹配内容'
```

通配符
┌_  任意单个字符
└%  0-无数个任意字符

转义:escape 
例如:where 字段 like '匹配内容' escape '符号'
功能:escape能够赋予这个符号一个特殊的功能
     该功能能够消除紧跟着的一位字符的特殊含义



#### 四、函数
四:函数
┌系统函数┌单行函数┌数值函数
│         │          ├字符函数
│         │          ├日期函数
│         │          ├转换函数
│         │          └通用函数
│         └多行函数
└自定义函数┌带参数
           └不带参数


学习函数的方法┌函数的名字
              ├函数的功能
              ├函数的参数个数
              ├函数的每个参数的类型
              └返回值的类型


##### 数值函数
数值函数┌abs
        ├mod
        ├ceil
        ├floor
        ├round
        ├trunc
        ├sign
        └power

1.abs(数)          返回绝对值        返回值:数值
2.mod(数1,数2)    取余            返回值:数值   

数一对数二取余
注意┌结果的正负情况只和数1的符号有关
    ├如果数2为1,则返回数1的小数部分
    └如果数2为2,则能判断奇偶性

3.ceil(数)      向上取整,往大了取最近的整数        返回值:数值
4.floor(数)     向下取整,往小了取最近的整数        返回值:数值

5.round(数1[,数2])    四舍五入        数1四舍五入保留数2位小数
6.trunc(数1[,数2])     截断               数1截断到保留数2位小数
注意┌如果数2不写,默认到整数
    └如果数2是负数,则表示保留到小数点前数2位
    
7.sign(数)            判断正负零    如果数1为正数返回1,负数返回-1,零返回0
8.power(数1,数2)    次方和开方    数1的数2次方


##### 字符函数
字符函数┌upper
        	  ├lower
       	   ├initcap
      	    ├length
     	     ├lengthb
    	      ├trim
    	      ├ltrim
    	      ├rtrim
    	      ├lpad
      	    ├rpad
    	      ├instr
     	     ├substr
    	      ├replace
      	    └translate

--大小写切换
┌upper(str)   转大写
├lower(str)   转小写
└initcap(str) 首字母转大写

--长度计算
┌length    字符长度    一个中文占一个字符长度,一个英文占一个字符长度
└lengthb   字节长度    一个中文占两个字节长度,一个英文占一个字节长度

--去除两端的字符串
┌trim(str)              去除str两端的空格
├ltrim(str1,str2)    去除str1左端的str2
└rtrim(str1,str2)    去除str1右端的str2

--填充
┌lpad(str1,数,str2)    在str1的左端填充str2,到数个字节长度
└rpad(str1,数,str2) 在str1的右端填充str2,到数个字节长度

高级字符函数
┌instr
├substr
├replace
└translate

1.instr(str1,str2[,数1,[数2]])    查找字符串出现的位置
解析:在str1中找str2,从第数1位开始找,找第数2次出现的位置

注意┌如果数1,数2不写则默认为1
    ├数1如果位负数,表示倒数第数1位,从后往前找,从前往后第几位
    └不能省略数1,直接写数2

2.substr(str1,数1,[数2])   截取字符串
解析:在str1中截取,从第数1位开始截取,截取数2个字符长度   (包括数1位置上的那个字符)
注意:┌如果数1为负数,表示倒数第数1位,从前往后截取
     └如果数2不写,则表示截取到字符串的最后

3.replace(str1,str2[,str3])  整体替换
4.translate(str1,str2,str3)  逐一替换

解析:把str1中所有str2整体(逐一)的替换成str3
加星大法:translate('helloworld','*ow','*')





##### 例题：字符函数练习
-- 2.将员工姓名(first_name)转成全宽,筛选出字节长度为18的员工,显示其员工编号,全宽姓名
```sql
SELECT 
    employee_id AS 员工编号,
    TO_MULTI_BYTE(first_name) AS 全宽姓名
FROM 
    employees
WHERE 
    LENGTHB(TO_MULTI_BYTE(first_name)) = 18;



-- 6.★查询ename和job有一个或一个以上相同的字母的员工信息
SELECT *
FROM emp 
WHERE LENGTH(TRANSLATE(job, '~' || ename, '~')) < LENGTH(job);


-- 7.将emp表中的所有sal,转为中文大写格式--如(1250转为壹仟贰佰伍拾)
select sal,translate(ltrim(rtrim(replace(replace(substr(sal,-4,1)||'仟'||
                     substr(sal,-3,1)||'佰'||
                     substr(sal,-2,1)||'拾'||
                     substr(sal,-1,1),'0佰'),'0拾'),0),'仟'),'123456789','壹贰叁肆伍陆柒捌玖')||'元整'
from emp
```




字符型在格子中靠左,数字型靠右


























### 转换函数 · 日期函数

#### 一、转换函数
转换函数   实现数据类型之间的转换
┌to_number(str)
├to_date(str,''日期格式)
└to_char┌(num,'数字格式')
        └(date,'日期格式')

1.to_number(str)   能够将字符串转为数值类型,必须是纯数字的字符串
隐式转换:纯数字的字符串在参与计算的时候会自动转为数值型进行计算

```sql
select '123'+'456'
from dual
```

2.to_date(str,'日期格式')  能够将字符串根据指定的日期格式转换为对应的日期数据
都要引号
to_date是根据日期格式,将你给的字符串(和日期格式匹配)转换成日期

日期格式┌yyyy   年
        ├mm     月
        ├dd      日
        ├hh        小时(12制)
        ├hh24    小时(24制)
        ├mi        分钟 minute,和月做区分所以用mi
        ├ss     秒
        ├day    星期几(中文)
        ├d      星期几(数字) 星期六对应7,美国是从周日开始算一周开始的,周日算1
        ├q      季度
        ├ddd    当前天是当年的第几天
        └sssss    当前秒是当天的第几秒

```sql
-- 将标准日期字符串转为日期
SELECT TO_DATE('2023-12-25', 'YYYY-MM-DD') FROM DUAL;
-- 结果: 2023-12-25

-- 转换带时间的字符串
SELECT TO_DATE('2023-12-25 14:30:45', 'YYYY-MM-DD HH24:MI:SS') FROM DUAL;
-- 结果: 2023-12-25 14:30:45

-- 处理斜杠分隔的日期
SELECT TO_DATE('12/25/2023', 'MM/DD/YYYY') FROM DUAL;
-- 结果: 2023-12-25

-- 处理包含月份名称的字符串
SELECT TO_DATE('25-DEC-2023', 'DD-MON-YYYY') FROM DUAL;
-- 结果: 2023-12-25

-- '2013-005' 表示2013年的第5天
SELECT TO_DATE('2013-005', 'YYYY-DDD') FROM DUAL;
-- 结果: 2013-01-05

-- '5' 表示一个未知月份的，星期值为5的那一天。
-- 这里的 'D' 单独使用没有意义，需配合月份。以下为错误示例：
SELECT TO_DATE('2013-9-5', 'D') FROM DUAL; -- 这会报错
```

如果缺了,会补系统时间的年月,天只能补1
```sql
SELECT TO_DATE('9-5', 'yyyy-dd') FROM DUAL;
```

3.to_char┌(num,'数字格式')   格式化数字   
         └(date,'日期格式')  提取日期元素(常用)

数字格式┌0    占位符      前面补0,不好看,放在个位上,防止零点几
        ├9  占位符(常用)  前面补空格
        ├$  美元(占位符)
        └L  本地货币(占位符)

```sql
select trim(to_char(12345678912,'L999,999,999,999.99'))
from dual
```
结果:
￥12,345,678,912.00

```sql
select trim(to_char(0.75,'$999,999,999,990.99'))
from dual
```
结果:
$0.75


这才是使用d和ddd,q的主要地方
```sql
SELECT TO_CHAR(TO_DATE('2013-2-1', 'YYYY-MM-DD'), 'd') FROM DUAL;

SELECT TO_CHAR(SYSDATE, 'day') FROM DUAL;

SELECT TO_CHAR(SYSDATE, 'mi') FROM DUAL;

SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD hh24:mi:ss') FROM DUAL;
```









#### 二、日期函数
日期函数  专门处理日期类型数据的函数
┌sysdate
├add_months
├months_between
├trunc
├next_day
└last_day

标准日期字面量  date'2026-10-1'  加个date表示这个字面量是日期型

1.sysdate     返回当前的系统时间
日期的计算规则┌日期加减数字 = 日期加减天数
              		   └日期-日期   =  日期之间相差的总天数

例:查询3天2小时28分32秒后的日期
```sql
SELECT SYSDATE,SYSDATE+3+2/24+28/60/24+32/60/60/24
FROM dual
```
例:距离国庆还有多少天
```sql
SELECT trunc(date'2026-10-1'-sysdate)+1
FROM dual
```

2.add_months(日期,数字)   日期加减数字个月
注意:后面的数字会自动截断到整数,4.5会变4,负数是减

```sql
SELECT ADD_MONTHS(SYSDATE, 4)
FROM dual
```
2027-01-08 14:17:22

3.months_between(日期1,日期2) 返回两个日期之间相差的总月数,日期一-日期二
注意:在months_between中换算方式是:1月=31天
    不会截断会有小数

```sql
SELECT TRUNC(MONTHS_BETWEEN(SYSDATE, DATE'2027-1-1'))
FROM dual
```

4.trunc(日期,'日期格式')   截断日期到日期格式之初
截断到今年之初

```sql
select TRUNC(SYSDATE, 'YYYY')
from dual 
```
2026-01-01 00:00:00
当你写 TRUNC(日期) 而不写第二个参数（日期格式）时，Oracle 会默认按照 'DD'（天） 来截断，也就是：
去掉时间部分，只保留日期，时间归零为 00:00:00

5.next_day(日期,'星期几'|数字)    查询日期的下一个星期几,正常求可能就在本周

如何精确的求到下周的星期几呢?
先把日期减去一天(防止今天就是星期日),然后求到本周日,再求本周日的下一个星期几就一定是下一周的星期几

```sql
SELECT NEXT_DAY(NEXT_DAY(sysdate-1, '星期日'), '星期一')
from dual
```
-14就是求上周
```sql
SELECT NEXT_DAY(NEXT_DAY(sysdate-14, '星期日'), '星期一')
from dual
```

6.last_day(日期)    返回日期那个月的最后一天
可以求倒数多少多少天

例一:
--假设emp表的hiredate为员工的出生日期
--查询每位员工目前几岁零几个月零几天了

```sql
select HIREDATE 出生日期,
             trunc(MONTHS_BETWEEN(sysdate, HIREDATE)/12)  岁,
             trunc(mod(MONTHS_BETWEEN(sysdate, HIREDATE)/12,1)*12) 月,
             trunc(mod(mod(MONTHS_BETWEEN(sysdate, HIREDATE)/12,1)*12,1)*31) 天 
from emp
```

例二:

-- 年份能被4整除不能被100整除或者能被400整除

- 数学计算
- 判断今年一共多少天
- 判断二月一共多少天
- 今年最后一天是今年的第几天

-- 数学计算

```sql
select case when mod(to_char(sysdate,'yyyy'),4)=0 and MOD(to_char(sysdate,'yyyy'),4)!=100  then '普通闰年'
                        when mod(to_char(sysdate,'yyyy'),400)=0 then '世纪闰年'
                        else '平年'
            end  什么年
from dual


- 判断今年一共多少天
```
明年第一天减今年第一天
```sql
select case ADD_MONTHS(trunc(SYSDATE,'yyyy'), 12)-trunc(SYSDATE,'yyyy') 
                when 365 then '平年'
                when 366 then '闰年'
                END 什么年
from dual
```


今年最后一天是今年的第几天
```sql
select sysdate ,to_char(ADD_MONTHS(TRUNC(sysdate, 'yyyy'), 12)-1,'ddd')
from dual
```






#### 三、习题
习题
-- 2.查询每个季度入职的人数,按照人数降序排列

```sql
SELECT TO_CHAR(HIREDATE, 'Q') AS 季度,
       COUNT(1) AS 入职人数
FROM EMP
GROUP BY TO_CHAR(HIREDATE, 'Q')
ORDER BY 入职人数 DESC;


-- 7.查询同年同月入职的人数，员工姓名用，隔开
SELECT 
    TO_CHAR(HIREDATE, 'YYYY-MM') AS 入职年月,
    COUNT(*) AS 人数,
    LISTAGG(ENAME, ', ') WITHIN GROUP (ORDER BY ENAME) AS 员工姓名
FROM EMP
WHERE HIREDATE IS NOT NULL
GROUP BY TO_CHAR(HIREDATE, 'YYYY-MM')
ORDER BY 入职年月;


-- 6.查询每年每个月入职的人数,筛选出人数大于3的月份,按人数降序排序
select TO_CHAR(HIRE_DATE, 'YYYY-MM'),COUNT(1)
FROM employees
GROUP BY TO_CHAR(HIRE_DATE, 'YYYY-MM')
HAVING COUNT(1)>3


-- 3.把hire_date列看做是员工的生日，查询本月过生日的员工信息
select *
FROM employees
where TO_DATE(TO_CHAR(SYSDATE, 'YYYY') || '-' || TO_CHAR(HIRE_DATE, 'MM-DD'), 'YYYY-MM-DD') BETWEEN trunc(SYSDATE,'mm') and ADD_MONTHS(trunc(SYSDATE,'mm'), 1)
```





















### 通用函数 · 聚合函数 · 分析函数

┌系统函数┌单行函数 ┌数值函数
│        │        ├字符函数
│        │        ├日期函数
│        │        ├转换函数
│        │        └通用函数
│        └多行函数┌聚合函数(group by)
                 └分析函数(开窗函数)
└自定义函数┌带参数
          └不带参数





#### 一、通用函数
一:通用函数       所有数据类型都可以使用的函数
1.case when   相当于python中的if判断
语法一:

```sql
case 字段 when 值1 then 返回值1
          when 值2 then 返回值2
                ...
      [else 返回值n]
          ...
          [else 返回值n]
          end  --end相当于一个结束关键字,结束这个case when判断
```
```sql
SELECT DEPTNO ,CASE DEPTNO WHEN 10 THEN '十部门'
                           WHEN 20 THEN '二十部门'
                           ELSE '三十部门'
                END  中文部门编号
from EMP
```


语法二:
```sql
case when 条件1 then 返回值1
     when 条件2 then 返回值2
      ...
 [else 返回值n]
 end
```
```sql
select DEPTNO ,case WHEN deptno = 10  then '十部门'
                    WHEN deptno = 20  then '二十部门'
                    else '三十部门'
                end 中文部门编号
from EMP
```


注意:不论case when写了多长,都只是相当于一个字段,建议加个别名
     case when的返回值必须保证类型一致,不能第一个then是字符型,第二个then是数字型,else是日期型,不可以
     这里的case when其实只是相当于值映射,并没有真的使用条件判断分支结构
     第一个语法只能对一个字段进行判断,第二个语法没有限定只能对一个字段操作
     第一个语法只能等值操作,第二个还可以用<=,>=等非等值判断



2.decode  相当于case when的第一种语法
语法:decode(字段,值1,返回值1,值2,返回值2...返回值n)  
--从底层来说decode更快
```sql
select DEPTNO,DECODE(DEPTno, 10,'十部门',20,'二十部门', '三十部门')
from EMP
```



3.nvl(含空字段,空值变后的值)     处理空值
解析:如果字段中的值为空则返回"值",否则正常返回字段中的值

```sql
select sal,comm,sal+NVL(COMM, 0)  总工资
from emp
```



4.distinct    去重
注意:只能放在select关键字后面,他会将他后面的所有字段放在一起进行去重操作

```sql
select DISTINCT DEPTNO  --只考虑这一个字段的重复
from emp  

select DISTINCT DEPTNO,EMPNO  --两个字段同时考虑重复,两个字段的一条数据都一样才算重复
from emp  
```

单行函数除了distinct,不会影响行数
而多行函数会影响行数,一般都是减少行数





#### 二、聚合函数
二:聚合函数         
功能:将多条数据根据分组的字段,高度聚合成一条数据的过程
语法:

```sql
select    要查询的内容(聚合函数)
from 表名
[where 限定条件]                 --分组前的限定
[group by 字段1[,字段2...]]      --分组的字段  ,我发现一个事,题干中每后面的一般都是要分组的
[having 限定条件]                --分组后的限定
[order by 字段 排序方向[,字段2 排序方向...]]    --排序(不是只有多行函数可以使用,所有的sql都能用),asc:升序(默认)  desc:降序
```

聚合函数 ┌max(字段):求最大值
        		├min(字段):求最小值
        		├avg(字段):求平均值
        		├sum(字段):求和
        		└count(字段):字段计数
例一:简单例子,根据deptno分组,升序

```sql
select DEPTNO,MAX(SAL),MIN(SAL),SUM(SAL),round(AVG(SAL),2)
FROM emp 
group BY DEPTNO
order BY DEPTNO ASC
```


例二:where和having
```sql
select max(ename),DEPTNO,MAX(SAL),MIN(SAL),SUM(SAL),round(AVG(SAL),2)
FROM emp 
where SAL > 1500
group BY DEPTNO
having sum(SAL) > 6000
order BY DEPTNO ASC
```


例三:不管字段是否有空值的情况下计数,用count(任意内容就行)
```sql
select deptno,count(1)
from emp
group by deptno
```
用nvl处理也行count(nvl(comm,0)),但是使用单行函数nvl,如果在数据多的情况下查询会很慢

```sql
SELECT COUNT(1)
from 表名  --那我们就可以用这个差一张表有多少数据
```


例四:多个排序条件
```sql
select  *
FROM EMP
order BY SAL desc,EMPNO asc
```
先按sal降序排,如果sal有相同的,对相同sal的数据再按empno升序排

注意事项┌select关键字后面只能放分组的字段和聚合函数,写别的会报错,比如ename,job等(第一个例子)
        ├where是对分组前进行限定,只能限定普通的字段
        ├having是对分组后进行限定,只能限定聚合的字段和分组的字段
        ├聚合函数中空值不参与计算
        ├count(1)在进行计数的时候只会记录数据的条目,不考虑实际的数值
        ├order by 字段 [asc]|desc[,字段2 [asc]|desc...]
        └order by的字段可以是select里的别名

聚合函数补充:
1.wm_concat(字段)    分组连接字符串
把这一组的该字段字符串放在一起,默认用逗号作为分隔符

```sql
select DEPTNO,wm_concat(ENAME),COUNT(ENAME),SUM(SAL),round(AVG(SAL),2)
FROM emp 
group BY DEPTNO
order BY DEPTNO ASC
```

2.listagg(字段,'分隔符')within group(order by 字段)
和wm_concat作用一样,但是可以指定分隔符,within group是给放在一起后的字段排序

```sql
select DEPTNO,listagg(ename,'-')within group(order by sal desc),COUNT(ENAME),SUM(SAL),round(AVG(SAL),2)
FROM emp 
group BY DEPTNO
order BY DEPTNO ASC
```




#### 三、分析函数（开窗函数）
三:分析函数(开窗函数)
功能:既能够显示聚合的结果又能够显示组内的明细
函数┌max(字段)over([p o])    
    ├min(字段)over([p o])
    ├sum(字段)over([p o])
    ├avg(字段)over([p o])
    └count(字段)over([p o])
    [p o]是开窗子句,可以不写,但是()不能省略,po之间用空格连接就行

语法:
```sql
select 要查询的内容
from 表名
[where 限定条件]
[order by 排序]
```

开窗子句┌partition by 字段[,字段2...]   --p            相当于group by,用来指定分组的字段
        └order by 字段 排序方向[,...]   --o            组内排序,表示累计的效果(切记)
        
按行累计求和:rows between unbounded preceding and current row (背下来可能会考)

例一:查询员工的姓名,工资,部门编号,部门的工资总和,不同岗位的工资平均,部门的数据条数,
```sql
select ENAME,sal,deptno,
             SUM(sal)over(partition by DEPTNO),
             round(AVG(sal)over(partition by JOB),2),
             COUNT(1)over(partition by DEPTNO)
from EMP
```

例二:不分组,排序,表示累计的效果
```sql
select ename,sal,deptno,
        sum(sal)over(order by sal)
from emp 
```
sum显示的是叠加,中间如果有相同的sal,sum会显示相同
再加个排序字段就行
```sql
select ename,sal,deptno,
        sum(sal)over(order by sal,ename)
from emp 
```
或者加那个句子
```sql
select ENAME,sal,deptno,
             SUM(sal)over(ORDER by SAL rows between unbounded preceding and current row)
from EMP;
```


分析函数补充:
排序和移动行的o都是不能省略的
1.排列                                          分数  100  90  90  80
┌row_number()over([p] o)    不并列       计数       1   2    3   4
├rank()over([p] o)          并列跳一级    排名      1   2    2   4
└dense_rank()over([p] o)    并列不跳级    颁奖      1   2    2   3

括号内不用加字段,想根据什么排,在order by后面加就行
```sql
SELECT SAL,rank()OVER(order by SAL) 排名
FROM EMP 
```

取前三名,在from里面填写另一个查询结果
```sql
SELECT *
FROM(SELECT SAL,rank()OVER(order by SAL) 排名 FROM EMP )
where 排名 <=3
```

2.移动行
┌lead(字段,数字[,值])over([p] o)    向上提
└lag(字段,数字[,值])over([p] o)     向下拉

字段:发生移动行的字段
数字:一次移动几行 
值:移动后的空值用什么值去填充

常用来做和下一行进行计算时
```sql
select ename,sal,lead(SAL,1,0)over(ORDER BY sal asc) AS 下一个人
from EMP
```

3.分组连接字符串
和聚合函数相同,多了一个开窗语句,有点鸡肋这个
┌wm_concat(字段)over([p][o])    默认以逗号作为分隔符
└listagg(字段,'分隔符')within group(o)over([p])   指定分隔符和组内排序顺序的分组连接字符串



#### 补充：SQL 逻辑执行顺序
6666666666666666666666
SQL 语句的逻辑执行顺序大致是： 废物刚好是哦

```sql
FROM（确定数据源）
WHERE（过滤行）
GROUP BY（分组）
HAVING（过滤分组）
SELECT（计算表达式、定义别名）
ORDER BY（排序）
```

GROUP BY 在第 3 步执行，而别名（级别）是在第 5 步才被定义的。所以当数据库解析到 GROUP BY 级别 时，它根本不认识 级别 这个名称（因为还没执行到 SELECT），于是报错：
ORA-00904: "级别": invalid identifier（Oracle）或类似错误。

WHERE 子句中不能直接使用窗口函数（如 AVG(SAL) OVER()），因为窗口函数是在 FROM 和 WHERE 之后、SELECT 之前计算的，逻辑顺序上不允许在 WHERE 中引用。






















































### 数据库五大语言 · 集合运算 · 行列转换
数据库五大语言┌DQL 数据库查询语言    简单查询,限定查询,模糊查询,函数...
              ├DCL 数据库控制语言    权限的赋予与回收
              ├DML 数据库管理语言    数据库的增删改(表中的数据) insert,update,delete,merge into
              ├DDl 数据库定义语言    数据库的结构管理    create,drop,alter,modify
              └TCL 事务控制语言        数据库的事务管理    ACID
                        

#### 一、集合运算
集合┌并集    union|union all  union是去重的并集,union all是不去重的并集
    ├交集    intersect
    └差集    minus  减去公共部分
    
注意┌集合是作用于两个SQL之间的
    ├如果我们需要添加别名,则必须添加到第一个sql上
    └多个集合连接必须保证字段的个数和类型统一

```sql
select ename,deptno
from emp
where deptno = 10  -- deptno in (20,30)
union  --union all,intersect, minus
select ename,deptno
from emp
where deptno = 20  -- deptno in (10,20)
```



#### 二、行列转换
行列转换:一维表和二维表之间的转换

##### 1.行转列
1.行转列    把一维表转为二维表
推导式:
因为一个 CASE表达式只能生成“一列”数据，而你最终的目标表需要“5列”（语文、数学、英语、历史、化学），所以必须写 5 个 CASE

```sql
select id,name,
       sum(case when course ='语文' then score end) 语文,
       max(case when course ='数学' then score end) 数学,
       max(case when course ='英语' then score end) 英语,
       max(case when course ='历史' then score end) 历史,
       max(case when course ='化学' then score end) 化学
from kecheng
group by id,name
order by id asc
```

公式:
```sql
select
from
[where]
```
...
```sql
pivot( 聚合函数(被聚合的列) for 行转列的列 in (列中值1,列中值2...) )
```

这里聚合函数优势也用count(1),一般用在一共就两列数据时,如习题最后一题
被聚合的列找消失的列(成为新值的列)
行转列的列就是里面要成为新字段的列 
列中值就是新字段,记得加引号,用别名

这种是每个学生的科目都是固定的这么多,所以不会有空值
```sql
SELECT *
FROM kecheng
PIVOT (
    MAX(SCORE)          -- 聚合函数（因为每人每科只有一条记录，用 MAX 或 SUM 均可）
    FOR COURSE IN (
        '语文' AS 语文,
        '数学' AS 数学,
        '英语' AS 英语,
        '历史' AS 历史,
        '化学' AS 化学
    )
)
ORDER BY ID;
```

但是水果demo表每个id下的水果不是确定的,会有空值,需要当作一个整体再查一遍用nvl去空
```sql
select *
from DEMO
pivot(sum(nums) for name in ('苹果' 苹果,'橘子' 橘子, '葡萄' 葡萄,'芒果' 芒果 ))

select NVL(苹果, 0) 苹果,
             NVL(橘子, 0) 橘子,
             NVL(葡萄, 0) 葡萄,
             NVL(芒果, 0) 芒果
from (select *
from DEMO
pivot(sum(nums) for name in ('苹果' 苹果,'橘子' 橘子, '葡萄' 葡萄,'芒果' 芒果 )))
```




##### 2.列转行
2.列转行    把二维表转为一维表
推导式:
```sql
select id,name,'语文' course,语文 score
from kecheng1
union
select id,name,'数学',数学
from kecheng1
union
select id,name,'英语',英语
from kecheng1
union
select id,name,'历史',历史
from kecheng1
union
select id,name,'化学',化学
from kecheng1
```

公式:
```sql
unpivot(被聚合的列的新列名(值的列) for 列转行的新列名(聚合的字段) in (字段1,字段2..))
```

把刚才行转列的表建出来
```sql
create table demo_new as select id,NVL(苹果, 0) 苹果,
             NVL(橘子, 0) 橘子,
             NVL(葡萄, 0) 葡萄,
             NVL(芒果, 0) 芒果
from (select *
from DEMO
pivot(sum(nums) for name in ('苹果' 苹果,'橘子' 橘子, '葡萄' 葡萄,'芒果' 芒果 )))
```

字段列名不用加引号
```sql
SELECT *
from kecheng_new
unpivot(分数 for 科目 IN (语文,数学,英语,历史,化学))
```

UNPIVOT 要求所有被转换的列具有相同的数据类型。
而 EMP 表中的列包含字符串（ENAME, JOB）、数字（MGR, SAL, COMM, DEPTNO）、日期（HIREDATE），类型不一致。

如果你希望 COMM 即使为 NULL 也显示一行（值为空或占位符），需要在 UNPIVOT 子句中加入 INCLUDE NULLS 关键字：

sql
```sql
SELECT empno, key, value
FROM (
    SELECT empno,
           TO_CHAR(ename) AS ename,
           TO_CHAR(job) AS job,
           TO_CHAR(mgr) AS mgr,
           TO_CHAR(hiredate, 'DD-mm月-YY') AS hiredate,
           TO_CHAR(sal) AS sal,
           TO_CHAR(comm) AS comm,
           TO_CHAR(deptno) AS deptno
    FROM emp
    WHERE empno = 7369
)
UNPIVOT INCLUDE NULLS (value FOR key IN (ename, job, mgr, hiredate, sal, comm, deptno))
ORDER BY key;
```


发的题目挺有意思,可以多看看








































### 子查询 · 表连接

#### 一、子查询
一:子查询        查询嵌套查询
┌相关子查询:子查询和主查询相互依赖,子查询不能够单独执行,
            先外后内，循环执行,外层每读取一行，就把这行的值传进内层，内层针对这一行重新计算一次
└非相关子查询:子查询和主查询相互独立,子查询可以单独执行,子查询执行的结果提供给主查询使用,
             先内后外。子查询独立执行一次，把结果丢给外层用

1.非相关子查询
分类┌单行单列子查询            一个值
    ├多行单列子查询            一个字段
    └多列子查询                一张表

例:
单行单列子查询
--查询SMITH所在部门的部门编号
--查询SMITH所在部门中的所有员工信息
```sql
select *
from emp
where deptno = (select deptno
                from emp
                where ename = 'SMITH')
```

多行单列子查询
--查询工资比20部门所有人都高的员工信息

先查20部门所有人工资
```sql
select sal
from emp
where deptno = 20
```

再查员工信息
补充:
比较符┌单行比较符  >  < = !=  >=  <=
      	 └多行比较符  any-任意  all-所有
      

>all相当于>max()
>any相当于>min()

```sql
select *
from emp
where sal > all (select sal
      from emp
      where deptno = 20)
```

多列子查询:
多列子查询叫做内联视图
例:按照工资排名,查询排名前三的员工信息

```sql
select *
from (select e.*, rank() over(order by sal desc) 排名
      from emp e)
where 排名 <= 3
```

e.*可以查所有的列加上别的列

用子查询
```sql
select e.*, rank() over(order by sal desc) 排名
from emp e
where 排名 < =3
```




2.相关子查询    不好用
一般不用:相关子查询放在select关键字后面叫做标量子查询又叫做性能杀手
推荐改写成表连接

相关子查询
--查询emp表中的员工姓名,工资,部门编号,部门名称,部门所在地
deptno每查出来一条,放内层再判断一次,所以要是数据多了就不好用
```sql
select ename,sal,deptno,
    (select dname from dept d where d.deptno = e.deptno) dname,
    (select loc from dept d where d.deptno = e.deptno) loc  
from emp e
```

补充:in的特殊用法┌in相当于 =any ,括号里任何一个
                 ├in里面如果有空结果不受影响,not in里面如果有空结果为空,这是一个陷阱
                 └where (字段1,字段2..) in (select 字段1,字段2.. from 表名), where后面写多个字段,可以和子查询选出来的多个字段一一匹配
查询出 “所有担任经理（管理者）职务的员工”

```sql
select *
from EMP
where EMPNO  in  (SELECT MGR FROM EMP) 
```
用not in就是空,原来查的是所有不是经理的普通员工,但是因为not in 陷阱,结果为空,实际不为空,可以用is not null排除
```sql
select *
from EMP
where EMPNO not in  (SELECT MGR FROM EMP WHERE MGR IS NOT NULL)
```

补充:with as--简化代码结构
语法:with 变量 as (子查询)        只能用于多列子查询,可以用这个变量来代表子查询
     select语句

```sql
with aa as (select deptno,max(sal) a,min(sal) b,count(1) c,avg(sal) d
            from emp
            group by deptno)
select *
from dept
where deptno = (select deptno
    from aa
    where (a-b)/c = (select max((a-b)/c) 最大公式
    from aa))
```







#### 二、表连接
二:表连接
定义:是用来将n张表连接成一张大表的过程,至少需要n-1个关联条件

分类:
共有数据和独有数据的区分是通过关联条件去判断的,满足关联条件就是共有数据,反之是独有数据
┌内连接:[inner] join → 只返回两张表的共有数据
└外连接┌左外连接:left [outer] join       返回两张表的共有数据和左表的孤儿数据,右表用空补齐,左外连接补右空
       ├右外连接:right [outer] join     返回两张表的共有数据和右表的独有数据,左表用空补齐,右外连接补左空
       └全外连接:full [outer] join      返回两张表的共有数据和各自独有的数据,互相用空补齐,全外连接补两边


语法:
```sql
select 要查询的内容
from 左表 
join 右表
on 关联条件 (e.字段 = d.字段)
join 第三张表
on 关联条件
```
...

刚才的例题,用表连接写
查询emp表中的员工姓名,工资,部门编号,部门名称,部门所在地

```sql
select ename,sal,e.deptno,DNAME,LOC  --两个都有的字段要加别名确定
from emp  e --别用as
join dept  d
on e.deptno = d.deptno
```

ON 和 where 不一样,on是关联条件,where是过滤条件



自然连接:?
等值连接:?
非等值连接:?
自连接
上下级可以用自连接
例题:查询员工编号,姓名,她的上级的员工编号,她的上级的姓名,他和上级的工资差
```sql
select e.empno,e.ename,e.mgr,d.empno,d.ename,e.sal-d.sal
from emp e
join emp d
on e.mgr=d.empno
```



补充:表连接的第二种写法
我去我去,这才是我之前的写法,现在才想起来,没有全外连接
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来
我去我去,这才是我之前的写法,现在才想起来

```sql
select 要查询的内容
from 左表 别名,右表 别名...
where 关联条件
```

┌内连接    where e.字段 = d.字段
├左外连接  where e.字段 = d.字段(+)  (+) 表示可以用空格补齐
└右外连接  where e.字段(+) = d.字段


注意:交叉连接→如果忘记写where了,则会出现笛卡尔积,会产生左表条目*右表条目 条数据




其他:

课时
3. ★查询20部门所有工资高于全表平均工资的员工姓名，工资，全表平均工资

4. ★查询工资最高的员工所在部门的员工信息
   从外往内写
```sql
   select *
   from emp
   where deptno = (select deptno
   from emp
   where sal = (select max(sal)
    from emp))
```
   从内往外写
   代码一样,只是思路不同

5 查询emp表每个部门的最高工资、最低工资、人数、平均工资显示(最高-最低工资)/人数的值最大的部门

```sql
with aa as (select deptno,max(sal) a,min(sal) b,count(1) c,avg(sal) d
            from emp
            group by deptno)
select *
from dept
where deptno = (select deptno
    from aa
    where (a-b)/c = (select max((a-b)/c) 最大公式
    from aa))
```


补充:
三范式:面试
表连接直接就是违反了三范式

写时
5.查询平均工资最高的部门信息
聚合函数还能聚合
```sql
SELECT d.*
FROM dept d
WHERE d.deptno = (
    SELECT deptno
    FROM emp
    GROUP BY deptno
    HAVING AVG(sal) = (
        SELECT MAX(AVG(sal)) FROM emp GROUP BY deptno
    )
);
```



in的特殊用法
8.查询职位和经理同员工SCOTT或BLAKE完全相同的员工姓名,职位,经理编号,结果不显示SCOTT和BLAKE

```sql
SELECT
    * 
FROM
    EMP 
WHERE
    ( job, mgr ) IN ( SELECT job, MGR FROM EMP WHERE ename IN ( 'SCOTT', 'BLAKE' ) ) 
    AND ename NOT IN ( 'SCOTT', 'BLAKE' )
```



5.★公司组织一场足球赛,每个部门都要和其他部门踢一场,查询出所有的比赛组合
(要求去取重复的组合,比如10-20和20-10算重复的组合)(dept表)
```sql
select d1.deptno 部门1,
             d1.DNAME,
             d2.deptno 部门2,
             d2.DNAME
FROM dept d1,dept d2
WHERE d1.deptno > d2.deptno
```



6.★查询同一部门入职日期晚,但是工资高的不同组合(要求去除重复的组合,规则同上一题)
```sql
SELECT e1.ename 职员1,
             e1.HIREDATE 晚入职时间,
             e1.sal 晚入职高工资,
             e1.deptno 职员1部门,
             e2.ename 职员2,
             e2.HIREDATE 早入职时间,
             e2.sal 早入职低工资,
             e2.deptno 职员2部门
FROM emp e1,emp e2
WHERE e1.HIREDATE>e2.HIREDATE 
            AND e1.sal>e2.sal
            AND e1.DEPTNO = e2.DEPTNO
```



































### 伪列 · 树形查询 · DCL · DDL

#### 一、伪列（rownum / rowid / level）
一:伪列:创建表时默认有的字段,但是看不见隐藏起来了
┌rownum    行号
├rowid    行地址
└level    序列

特性:不用手动创建,数据插入的时候自动生成的字段,如果不查询则不会自动显示
补充:伪列的特性:作为查询条件时,必须是从1开始且连续


1.行号 rownum
数据刚插入的时候就确定了,跟查询时的排序没有关系,第几条插入的数据就是几
补充:伪列的特性:作为查询条件时,必须是从1开始且连续

分页查询
oracle各版本通用语法,使用伪列查询
其实这不是完整的,完整的在内部还要排序

```sql
select *
from (select e.*,rownum rw
      from EMPLOYEES e)
where rw between 8 and 20 --第8到20条数据
```

宏代换,用户可以输入页数
```sql
select *  --为什么这里要用子查询,因为伪列从1开始的特性
from (select e.*,rownum rw
      from emp e)
where rw between 4*&a-3 and 4*&a  --plsql支持宏代换,navicat不支持,&a只能写死成数字

oracle12及以上才支持的语法(查看oracle版本:SELECT version FROM product_component_version)
SELECT *
FROM EMPLOYEES
ORDER BY EMPLOYEE_ID  -- 或任何能唯一确定顺序的字段
OFFSET 0 ROWS FETCH NEXT 10 ROWS ONLY;  --从0开始,后面10条数据
```





2.rowid   行地址

去重操作(查重)
创建测试数据
```sql
create table e as select * from emp
insert into e select *from emp where deptno=10
```

未重复的数据
```sql
select max(rowid)
from e e
group by empno
```

查重:
```sql
select *
from e
where rowid not in (select max(rowid)
                    from e
                    group by empno)
```

去重:                
```sql
delete from e where rowid not in (select max(rowid)
                                  from e
                                  group by empno)                    
```



3.level
直接查查不出来,需要特殊子句connect by
序列可以作为一个范围内递加的参数进入sql中
子句:connect by level <= xxx
--查询今年所有的周末


作用一:作为参数和条件参与查询
```sql
select trunc(sysdate,'yyyy')+level-1  日期,
       to_char(trunc(sysdate,'yyyy')+level-1,'day') 星期几
from dual
where to_char(trunc(sysdate,'yyyy')+level-1,'day') in('星期日','星期六')
connect by level <= to_char(add_months(trunc(sysdate,'yyyy'),12)-1,'ddd') --后面这是一年的天数,这句sql会生成1到365的序列
```



作用二:树形结构里的深度（层级）
补充:树形查询     
语法:
```sql
select 要查询的内容
from 表名
[start with 条件]     如果省略,表示每个人都做一次根节点查一次
connect by prior 父母列=孩子列      查根节点的上级
                 孩子列=父母列      查根节点的下级
```


level:显示节点深度

例子:
显示king的所有下级以及节点深度
同时用空格表示架构

```sql
select lpad(' ',2*level-2,' ')||ename  姓名, 
       level 等级,
             sys_connect_by_path(ename,'\') 当前路径到根节点的路径,
             connect_by_root ENAME  根节点名字,
             connect_by_isleaf 是否是叶子节点
from emp
start with ename = 'KING'  --可以去掉玩玩
connect by prior empno = mgr
```

函数:┌sys_connect_by_path(列,分隔符)   返回从当前节点一直到根节点的绝对路径
        ├connect_by_root 列              返回根节点的某个字段
        └connect_by_isleaf               判断当前节点是不是叶子节点






******自己的笔记的用户管理,这一块有点构造的,建议整理的时候结合以前的笔记和d老师一起看******

#### DCL 数据库控制语言
DCL:数据库控制语言    赋权和回收权限
┌create
├delete
├select
├with read only
├dba
├...

dba权限:数据库最高管理者权限
赋权┌grant 权限  [on 表名] to 用户
       └revoke 权限 [on 表名] from 用户

┌cmd+sqlplus
└plsql+命令窗口
******自己的笔记的用户管理,这一块有点构造的,建议整理的时候结合以前的笔记和d老师一起看******






#### DDL 数据库定义语言
DDL        数据库定义语言      进行对象结构管理
┌create      创建
├drop        删除
├alter       修改(表)
└modify      修改(字段)

1.创建表
┌复制表:create table 表名 as select语句       会把select查询的结果集创建成一张表,但是这样只是复制数据和表结构,只有其形,没有其神,约束什么的都没有
└手工创建表

复制表:
创建一张表叫emp1，结构同emp
```sql
create table emp1 as select * from emp 
```
创建一张表叫emp2，结构同emp,但不要内容
```sql
create table emp2 as select * from emp where 1=2
```

手工创建表:
```sql
create table 表名 (
                    字段名1 字段类型(类型长度) 约束,
                    字段名2 字段类型(类型长度) 约束,
                    字段名3 字段类型(类型长度) 约束
                    ...
                        )
```

┌数值型  number/int   number:浮点型→ number[(数1[,数2])]   int:整形 → int
├字符型  char|varchar|varchar2  char:定长字符    varchar|varchar2:可变长字符
└日期型  +date|timestamp        date:短日期结构    timestamp:长日期结构(时间戳)


约束(constraint)
┌主键约束    primary key     pk        唯一且非空   一张表只能有一个主键,但可以设置联合主键(作业有,记得看)
├唯一约束    unique          un        唯一
├检查约束    check           ck        枚举选值    check (字段 in (值1,值2..))
├非空约束    not null        nn        非空
├外键约束    foreign key     fk        参照表的参考列需要是唯一的,主键或唯一性
└默认值约束  default(不算约束)          当我们的值传进来是空时,会自动替换成default后面的值

创建一张学生表
```sql
create table student(
    sno char(4) primary key,
    sname varchar2(20) not null,
    gender char(2) check (gender in ('男','女')), --检查约束用法
    age int not null,
    address varchar2(200),
    phone_number number(11) unique,
    goschoolday date default sysdate
)
```

外键:
```sql
-- 级联删除        on delete CASCADE
-- 级联删除为空    on delete SET NULL
-- 不指定级联删除  on delete NO action --默认
```
删除父表的数据时，
如果不指定级联删除，必须先删除子表的相关项，再删除父表的对应行
如果指定了级联删除，子表的相关项和父表的对应行一起被删除
如果指定了级联删除为空，父表的对应行被删除，子表的相关项还在，外键列的值变成空
设置外键:
后面是参考表参考字段
```sql
CONSTRAINT fk_1 FOREIGN KEY(deptno) REFERENCES dept(deptno) ON DELETE SET NULL;
```



2.删除表
```sql
drop table 表名 [purge]
```
┌不加purge  逻辑删除    会将表删除到回收站(user_recyclebin)
└加了purge  物理删除    直接从本地文件中删除

补充:闪回命令→从回收站收回表
```sql
flashback table 表名 to before drop
```


补充:truncate  清空表
语法:truncate table 表名
truncate只是清空数据,表结构还在,不过数据是闪不回来的
drop直接删掉一张表



3.修改
老王让我们ds搜,666
┌alter        修改(表结构)
└modify        修改(字段)







作业作业没看作业作业没看
作业作业没看作业作业没看
作业作业没看作业作业没看
作业作业没看作业作业没看
作业作业没看作业作业没看
作业作业没看作业作业没看
作业作业没看作业作业没看
作业作业没看作业作业没看
作业作业没看作业作业没看





































































### 视图 · 序列 · DML · TCL 事务

#### 一、视图和序列
一:视图和序列
还是DDL
--视图view            
视图是一张虚拟表,其内容由查询定义    --相当于把查询出来的结果集拍了个照片,不能增加和删除列,但是可以更新插入数据
相当于把查询结果创建成一个新表(视图)给不懂sql得业务人员看

创建
```sql
CREATE [OR REPLACE] VIEW 视图名 AS select语句 [WITH READ ONLY];
```
·如果加上 OR REPLACE ,如果视图名已存在,会覆盖之前的视图；
  如果不加 OR REPLACE ,如果视图名已存在,会报错(名称已由现有对象使用),不能创建。
·如果加上 WITH READ ONLY 是创建只读视图,视图不能被修改；
  如果不加 WITH READ ONLY 是创建普通视图,视图可以被修改,修改视图,原表也会被修改,最好加上。
修改创建视图时使用的表,视图也会自动修改

查询视图和查表一样
删除视图:drop view 视图名

视图的优缺点：(可能考)
优点：
1.可以定制用户的数据
2.可以简化数据操作
3.基表中的数据有了一定的安全性（视图只能让他们看想让他们看的列）
4.可以合并分离数据，创建分区视图（多个表连接时可以只显示几个列）
5.几乎不占空间
缺点：
1.性能差（每次查看一次视图就会执行一次创建视图的SQL语句）
2.修改受限（因为修改视图会修改原表，所以一般都是只读）

例:
```sql
create or replace view v2 as select lower(ename) ename,sal+10000 s
    from e with read ONLY

select *
FROM v2

DELETE  from  v2
where S >12000  --报错,无法删除
```



--序列 sequence
数据库中一个特殊存放等差数列的表。主要是用来提供主键值。(因为主键的要求为唯一且非空,序列肯定不为空,我们只要不循环或者控制他的循环不重复)

创建序列
```sql
CREATE SEQUENCE 序列名
[START WITH 数]--从几开始,默认1
[MAXVALUE 数]--到几结束,默认9999999999999999999999999999,相当于无穷大
[MINVALUE 数]--最小值,默认1(循环之后回到的最小值),有cycle这个数才有用
[INCREMENT BY 数]--步长/等差,默认1
[CACHE 数]--缓存值,默认20(缓存值必须小于循环值)
[CYCLE]--是否循环,默认不循环
```

查询序列
序列.currval 当前值   --后执行,必须执行一次nextval才有值
序列.nextval 下一个值 --先执行

删除序列:drop sequence 序列名

例:
```sql
create sequence s_3
start with 5
maxvalue 100
minvalue 2
increment by 7
cache 10
cycle
```
依次取下个值:
```sql
select s_3.nextval
from dual 
```






#### 二、DML 数据库管理语言
二:DML 数据库管理语言     事务管理
┌插入   insert
├更新    update
├删除   delete
└合并   merge into

1.插入
┌批量插入:insert into 表名[(字段1,字段2..)] select语句
└手动插入:insert into 表名[(字段1,字段2..)] values(值1,值2..)
字段和值一定要匹配
手动插入一次只能插入一条

2.更新

```sql
update 表名 set 字段=字段|值,字段2=字段|值.. [where 条件]
```
例子:
```sql
update emp3 set ename=lower(ename),sal=sal+10000,comm=0 where deptno != 30
```


3.删除
```sql
delete from 表名 [where条件]
```


delete和truncate的区别
┌truncate是DDL语言(不能回滚),执行快,不能加限定条件,会降低高水位线
└delete是DML语言(能回滚),执行慢,可以加限定条件,不会降低高水位线


4.merge into 合并        拉链表
有则改之无则加勉
语法: 
```sql
merge into 目标表 别名
using 参考表 别名
on (判断条件)   --根据哪个字段判断有无
when matched then   --当匹配上(有则改之)
update set 字段1=字段|值... 
when not matched then  --当没匹配上(无则加勉)
insert [(字段1,字段2...)] vlaues(值1,值2...)
```

示例
示例表
```sql
create table emp_old as select * from emp where deptno in (10,20);
create table emp_new as select * from emp where deptno in (20,30);
update emp_new set ename=lower(ename),sal+10000 where deptno=20;

merge into emp_old a  --更新old表
using emp_new b
on (a.empno = b.empno)
when matched then
    update set a.ename = b.ename ,a.sal = b.sal
when not matched then
    insert values (b.empno, b.ename, b.job, b.mgr, b.hiredate, b.sal,b.comm,b.deptno)
comit;
truncate table emp_new;
```









#### 三、TCL 事务控制语言
TCL    事务控制语言
只有dml可以可以进行事务的提交和回滚
┌commit        提交
└rollback   回滚

你不需要（也不能）使用 BEGIN 或 START TRANSACTION 来手动开启事务。Oracle 事务的开启是自动的，但有明确的触发条件：
当事务已经结束，你执行下一条可执行的 SQL 语句时，一个新事务会自动开启。这里的“可执行 SQL”主要指 DML 语句（INSERT、UPDATE、DELETE、MERGE）。如果你执行的是 SELECT 查询，则不会开启事务。

Oracle：默认不自动提交，第一条 DML 会自动开启事务(不用START TRANSACTION)，后续 DML 自动加入同一个事务，直到你 COMMIT 或 ROLLBACK。





TCL/TPL 事务控制语言/事务处理语言
事务:
狭义事务:从一次提交或者回滚到
         下一次提交或者回滚的所有DML操作
广义事务:任何一次要么成功要么失败的DML操作
会话:一个用户从登陆到退出叫做一次会话

事务的四个特性:ACID
原子性:事务包含的所有操作，要么成功，要么失败
       回滚，成功则必须全部应用到数据库，否则
       不会对数据库造成影响
一致性:事务执行前和执行后保持一致的状态
隔离性:当多个用户并发访问数据库时，数据库为
       每个用户开启的事务不被其他用户的操作
       所干扰，多个并发事务之间是相互隔离的
持久性:一个事务一旦提交，对数据库的改变是永久的
       即使在数据库遇到故障的情况下也不会丢失
       事务的操作

```sql
commit   提交
rollback 回滚
```


连接数据库的五项条件:用户名/密码@ip:端口/schema ->如  scott/scott@127.0.0.1:1521/ocrl1



操作事务：
1、执行commit语句提交事务
2、执行rollback语句撤销或者回滚事务
3、执行一条数据定义语言，如create、alter、drop是。如果执行成功，自动提交，否则自动执行rollback命令。
4、执行一条数据控制语言，如grant、revoke等，操作完毕。Oracle自动执行commit命令。
5、正常断开sql*plus，Oracle会自动提交或者回滚。

```sql
insert into emp_1 select * from emp
commit
select * from emp_1
```

什么是锁：(有兴趣看看)
锁是对数据进行并发控制的机制，用来保证事务的隔离性，即事务内部的操作和使用的数据对并发执行的其他事务是隔离的，互不干扰的。
为什么要加锁：
数据库是一个多用户使用的共享资源。当多个用户并发地存取数据时，在数据库中就会产生多个事务同时存取同一数据的情况。若对并发操作不加控制就可能会读取和存储不正确的数据，破坏数据库的一致性。
锁的分类：
排他锁：又称为X锁或者写锁。当需要进行insert、update、delete时加上排它锁。若事务T1对数据R加上了排它锁，则只允许T1对数据R进行读取和修改，其他事务可以select数据R，但是不能修改数据R，除非T1事务解除对数据R的排他锁。
共享锁：又称为S锁或者读锁。若事务T2对数据R加上了共享锁，允许T2读取R，其他事务也可以读取R。
加锁的方法：lock table xx in row share mode | exclusive mode 
当程序对所做的修改进行提交(Commit)或回滚(Rollback)后，锁住的资源便会得到释放，从而允许其他用户进行操作
什么是死锁：
如果两个事务，分别锁定一部分数据，而都在等待对方释放锁才能完成事务操作，这种情况下就会发生死锁
多个用户之间相互占用对方资源,导致谁都不能完成自己的操作,陷入无穷无尽的等待
死锁的预防：
1、执行事务时尽可能快得提交，能大程度上避免死锁。
2、进行批量操作时，不同程序员操作表的顺序应该一致。





### 正则（仅提及）
讲了一点正则



作业没写

作业没写

作业没写

作业没写

作业没写

作业没写

作业没写

作业没写


---

## PL/SQL 编程


### 匿名块：变量 / 判断 / 循环
plsql:流程化控制的SQL语句
┌变量
├变量赋值
└变量声明

┌匿名块┌匿名块的创建方式
│      ├条件判断 
│      ├循环
│      ├异常处理
│      └游标
└有名块┌自定义函数
       ├存储过程
       ├动态SQL
       ├数据字典
       ├闪回技术
       ├触发器
       └包
       
匿名块:没有名字的代码块

我感觉目前来说就是类似Python编程那种,一般写个小程序啥的,
然后:declare部分就是负责声明变量,像pyhton的a = 3
     degin后面开始程序的真正逻辑部分
     end结束程序

语法:


```sql
[declare]
声明部分;
begin
执行部分;
[excpetion]
异常处理部分;
  end;   结束
```
匿名块的注意事项:
┌每条语句的结尾必须有分号(declare,begin,exception不用)
├=表示判断是否相等,:=表示赋值
└declare可以省略,表示没有声明任何的变量

  

┌变量:只是一个容器,占用实际的内存空间,值通过赋值来决定
├变量赋值:给一个定义好的变量容器进行赋值
└变量声明:声明一个变量,把一个字符串声明成一个容器



#### 一、变量的声明
一:变量的声明
变量名 类型(类型长度)

注意:数据库是强类型变量,不能够自动转换数据类型(Python是弱类型,你给变量什么值变量就是什么类型)

类型拓展 ┌数值型:number,int
        ├字符型:char,varchar,varchar2
        ├日期型:date,timestamp
        ├表名%rowtype        可以拷贝某张表的类型,该表类型变化时,拷贝时也会变化
        └表名.字段名%type   可以拷贝某张表的这个字段的类型,该字段类型变化时,拷贝时也会变化


#### 二、变量的赋值
二:变量的赋值
1.在声明时直接赋值
2.在执行部分(begin)进行赋值
3.变量之间赋值   a1=a2
4.select 内容 into赋值       把查询出来的结果,赋值into后面的变量
5.returning into 赋值       它的作用是：在执行 INSERT、UPDATE 或 DELETE 操作的同时，顺便把受影响行的某些列的值，直接抓取并赋值给 PL/SQL变量。delete会将原来的值赋值给变量

┌返回:return  暂时不学
└打印:┌dbms_output.put_line(参数)  换行打印
      └dbms_output.put(参数)  不换行打印
不论写多少个不换行打印最后都要加上一个换行打印

代码示例:
```sql
declare
v1 number:=10;
v2 varchar2(20);
v3 date;
v4 varchar2(20);
v_ename emp1.ename%TYPE;
begin
    v2 := 'abc';
    v4 := v2;
    
    --select into 赋值
    select sysdate into v3
    from dual;
    
    --returning into 赋值
    update emp1 set ename = lower(ename) where empno=7788
    returning ename into v_ename;
        
    DBMS_OUTPUT.PUT_LINe(V1);
    DBMS_OUTPUT.PUT(v_ename);
    DBMS_OUTPUT.PUT_LINe('1231231');
    DBMS_OUTPUT.PUT_LINe(v2);
    DBMS_OUTPUT.PUT_LINE('');
end;   
```



#### 三、条件判断
三:条件判断
1.if判断

```sql
if 条件表达式 then 要执行的语句;
elsif 条件表达式 then 要执行的语句2;
[else 要执行的语句n;]
end if;   --要写end if 表示判断结束
```

简单示例
```sql
DECLARE
SCORE NUMBER := [$input];
BEGIN
    IF SCORE <=60 and score >=0 then DBMS_OUTPUT.PUT_LINE('不及格');
    elsif score >60 and score <=80 then DBMS_OUTPUT.PUT_LINE('还行');
    elsif score >80 and score <=100 then DBMS_OUTPUT.PUT_LINE('优秀');
    ELSE DBMS_OUTPUT.PUT_LINE('请输入正确成绩');
    end if;
end;
```



2.case when 判断
```sql
case when 条件表达式 then 要执行的语句;
     when 条件表达式 then 要执行的语句2;
     [else 要执行的语句n]
end case;  -同理
```

简单示例:
```sql
DECLARE 
level number:=[$input];
begin 
    case when level=1 then DBMS_OUTPUT.PUT_LINE('工资10000');
         when level=2 then DBMS_OUTPUT.PUT_LINE('工资8000');
         when level=3 then DBMS_OUTPUT.PUT_LINE('工资4000');
         when level=4 then DBMS_OUTPUT.PUT_LINE('工资2000');
         else DBMS_OUTPUT.PUT_LINE('该等级不存在');
         end case;
end;
```




#### 四、循环
四:循环
1.loop循环
循环的三要素 ┌初始值
            ├自增变量
            └退出条件

语法:
```sql
loop
循环体;
exit when 条件;  --退出条件
end loop;
```
```sql
--循环打印1-10
declare
v1 number:=1;
begin
    loop
        dbms_output.put(v1);
        exit when v1=10;
        v1 := v1+1;
    end loop;
    dbms_output.put_line('');
end;


--打印1+2+3+..+10=55这个式子
declare
v1 number:=1;
v_sum number:=0;
begin
    loop
        exit when v1 = 11;
        if v1 <10 then dbms_output.put(v1||'+');
        else dbms_output.put(v1||'=');
        end if;

        v_sum := v_sum+v1;
        v1 := v1+1;
    
    end loop;
    dbms_output.put_line(v_sum);
end;
```



2.while循环
当满足条件才会进入循环
语法:

```sql
while 条件 loop
    循环体;
end loop;
```



```sql
--循环打印1-10
declare
v1 number:=1;
begin
    while v1<=10 loop
        dbms_output.put_line(v1);
        v1:=v1+1;
    end loop;
end;
```



3.for循环
语法:
```sql
for 变量 in 小值..大值 loop
    end;
```


```sql
begin
    for i in 1..10 loop
        dbms_output.put_line(i);
    end loop;
end;
```






navicat中&a的使用  [$a]
```sql
SELECT * FROM emp WHERE deptno = [$input];
```




















































### 游标 · 异常 · 有名块（函数 / 存储过程 / 索引 / 触发器 / 包）

#### 游标 cursor
--游标 cursor
游标是一种能从多条记录的结果集中每次提取一条记录的机制。
游标可以充当指针。游标能遍历结果中的所有行，但是一次只能指向一行。
游标提供了在逐行的基础上操作表中数据的方法。游标的结果集是由select语句产生的。
--游标的分类 
┌静态游标┌显式游标
│        │        
│        └隐式游标
└动态游标:动态游标

--静态游标-显式游标
语法:

```sql
declare
--声明游标 
cursor 游标名 is select语句;
begin
    --打开游标,游标是需要打开的
    open 游标名;
    
    --提取游标中的值,fetch一次只取一个值
    fetch 游标名 into 变量; 
    
    --关闭游标
    close 游标名;
end;
```

游标+循环 --为了不需要写很多fetch
1.loop+游标名%notfound
loop循环┌初始值        open cur1 --原神启动
        ├自增条件    fetch cur1 --fetch一次加一次
        └退出条件    游标名%notfound  --通过游标属性判断是否结束

游标的属性┌游标名%isopen           判断游标是否打开,打开则返回真,否则报错
          ├游标名%found           判断当前游标中是否有值,有值返回真,否则返回假
          ├游标名%notfound        判断当前游标是否没有值,如果没有值返回真,否则返回假
          └游标名%rowcount        返回当前游标遍历了多少行
示例一:

```sql
declare
cursor cur1 is select ename from emp;
v_ename emp.ename%type;
begin
    open cur1;
    loop
        fetch cur1 into v_ename;
        exit when cur1%notfound;  --退出条件要写在这里,不然会多打印最后一条数据
        dbms_output.put_line(v_ename);
    end loop;
    close cur1;
end;
```

示例二-打印dept表:(重点看,他说要考)
```sql
DECLARE
cursor cur1 is select * from dept;
v_dept dept%rowtype;
begin 
    open cur1;
    
    loop
        fetch cur1 INTO v_dept;
        exit WHEN cur1%notfound;
        DBMS_OUTPUT.PUT_LINE(v_dept.deptno||' '||v_dept.dname||' '||v_dept.loc); --这是字段,全写出来了
        end loop;
close cur1;
end;
```

2.while+游标名%found  --搭配的found



--静态游标-隐式游标
不用声明、不用打开、不用赋值、不用关闭。
这些都是oracle系统自动完成的。
常用的方式有delete/UPDATE/INSERT/select..into单行赋值。(DML和select...into)

它不是一种独立的数据类型，而是数据库内部自动开辟的一块“内存工作区”（上下文区），并且向外暴露了几个“属性（特征）”，供你在 PL/SQL 中调用，从而获知刚才那条 SQL 执行的状态。
我觉得这个隐式游标是作为一种特殊的数据特征来辅助plsql中进行运作的,所以想用就用,不用声明打开赋值啥的,只要你执行了dml操作他就会作为一种数据特征供你使用

隐式游标的四个属性
1.sql%ISOPEN 判断游标是否打开,永远返回假。
2.sql%FOUND 判断游标的指针里是否有值，如果最近一次有结果，返回真，否则假。      最近的一次DML操作是否影响结果
3.sql%NOTFOUND 判断游标的指针里是否没值，如果最近一次没结果，返回真，否则假。   最近的一次DML操作是否没影响结果
4.sql%ROWCOUNT 游标的指针指了多少行，返回最近一次从游标读取的数据。最近的DML操作影响了多少条结果,这个我觉得挺有用的

```sql
declare
begin
    delete from emp1 where deptno = [$a];
    if sql%notfound then dbms_output.put_line('没有删除数据');
    elsif sql%found then dbms_output.put_line('删除数据,删除了'||sql%rowcount||'条数据');
    end if;
end;
```




--动态游标
--动态游标

和静态游标本质上是一样的,只不过动态游标select的结果集会一直变化

**动态游标每次打开都是新的结果集，静态游标是声明的时候指定结果集**

语法:

```sql
declare
游标名 sys_refcursor;  --sys_ref + cursor
begin
    --打开游标
    open 游标名 for select 语句     --每次打开都是一个新的结果集
    loop
        fetch 游标名 into 变量;
        exit when 游标名%notfound;
        dbms打印;
      end loop;

    close 游标名;
  end;
```
说白了,一模一样

示例:打印emp表和dept表
```sql
DECLARE 
v_1 sys_refcursor;
v_emp emp%rowtype;
v_dept dept%rowtype;
begin 
        open v_1 for select * from EMP;
        loop
            fetch v_1 into v_emp;
            exit when v_1%notfound;
    DBMS_OUTPUT.PUT_LINE (v_emp.EMPNO||' '||v_emp.ENAME||' '||v_emp.JOB||' '||v_emp.MGR||' '||v_emp.HIREDATE||' '||v_emp.SAL||' '||v_emp.COMM||' '||v_emp.DEPTNO
);
        end loop;
        close v_1;

        open v_1 for select * from dept;
        loop
            fetch v_1 into v_dept;
            exit when v_1%notfound;
    DBMS_OUTPUT.PUT_LINE (v_dept.DEPTNO||'    '||v_dept.DNAME||'    '||v_dept.LOC);
        end loop;
        close v_1;
end;
```



上面的都可以不用会,但是一定要会这个
--for游标:性能消耗比动态游标大
语法:
```sql
for 变量 in (select语句) loop
    循环体

    end loop;
```

示例
```sql
DECLARE
begin
    for i in (select * from dept) loop
        dbms_output.put_line(i.deptno||' '||i.dname||' '||i.loc);
    end loop;
end;
```
推荐使用动态游标和for游标,隐式游标作为一种属性参数辅助操作,而显式游标则较为鸡肋





#### 异常 exception
--exception 异常
┌预定义异常--系统给的,有名字
├非预定义异常--系统给的,没名字
└自定义异常--用户自定义

为什么要处理异常?
由于plsql程序块一旦产生异常而没有指出如何处理时,程序会自动终止。
异常情况处理就是用来处理这些正常执行中未预料到的事件,使程序还能正常运行

第一天上匿名块就讲了,begin后面可以加个exception
语法为

```sql
exception
    when 异常 then 怎么办;
```

--预定义异常（不用声明）
违反唯一性限制 dup_val_on_index
没有找到数据   no_data_found
返回多行       too_many_rows
试图用0做除数  zero_divide
其他异常       OTHERS--写到最后  ┌SQLCODE:错误代码 搭配others一起用
                                └SQLERRM:错误描述 会将错误代码和原因输出出来


```sql
declare
v_sal int;
begin
    select sal into v_sal
    from emp
        where ename = [$input];  --删掉这行会报too_many_rows的错误

    dbms_output.put_line(v_sal);
exception
    when no_data_found then dbms_output.put_line('未找到数据');
    when too_many_rows then dbms_output.put_line('值过多');
    when others then DBMS_OUTPUT.PUT_LINE(sqlcode||'  '||sqlerrm);
end;

declare
v_sal int;
begin
    insert into emp(empno) values(7788);

    dbms_output.put_line(v_sal);
exception
    WHEN dup_val_on_index then dbms_output.put_line('违反唯一性约束');
    when others then DBMS_OUTPUT.PUT_LINE(sqlcode||'  '||sqlerrm);
end;
```

--非预定义异常
就是数据库系统有的异常,你给他一个新名字
--向emp表插入一条数据;

```sql
DECLARE
err1 EXCEPTION;--声明一个异常,异常名为err1
PRAGMA EXCEPTION_INIT(err1,-2291);--把-2291这个错误赋给异常err1


declare
v_sal int;
yes exception;
pragma exception_init(yes, -1422);
begin
    select sal into v_sal
    from emp;

    dbms_output.put_line(v_sal);
exception
    when yes then dbms_output.put_line('值过多1234567');
end;
```
--自定义异常

当我们的业务对数据有要求,那我们就可以手动添加异常来保证数据的合理性
语法:

```sql
declare
--声明异常变量
变量名 excpetion;  --这步不和非预定义异常一样吗
begin
    --抛出异常
    raise 异常名;  --只要运行到这个raise就会跳转到exception处,下面的代码不会再执行

exception
    when 异常名 then 要执行的操作;
end;
```


如何弹窗报错:raise_application_error(错误代码,错误描述)   --错误代码的取值范围要求为-20000~-20999


--emp1表和emp表格式一样.向emp1插入一条empno,sal,job
--要求empno必须是4位数字并且7开头,工资必须大于3000,工作不能为空
--如果不合规定,弹窗报错"...必须..."
```sql
declare
v_empno emp.empno%type:=[$empno];
v_sal emp.sal%type:=[$sal];
v_job emp.job%type:=[$job];
err1 exception;
err2 exception;
err3 exception;
begin
    --数据验证
    if v_empno not like '7___' then raise err1;
    elsif v_sal <=3000 then raise err2;
    elsif v_job is null then raise err3;
    end if;

    insert into emp1(empno,sal,job) values(v_empno,v_sal,v_job);
    commit;

exception
    when err1 then raise_application_error(-20000,'员工编号必须是7开头');
    when err2 then raise_application_error(-20001,'工资必须大于3000');
    when err3 then DBMS_OUTPUT.PUT_LINE('职位必须不为空');
end;
```
























匿名块
┌判断
├循环
├游标
└异常处理

有名块:像pyhton的函数,想用就调用,匿名块写完后用完就没有了
┌自定义函数:实现功能
├存储过程:实现业务的封装
├数据字典
├动态SQL
├闪回
├触发器
└包


#### 自定义函数
一:自定义函数
语法:        
```sql
create [or replace] function 函数名[(参数 [in]|[out] 数据类型,参数2..)] return 返回值的数据类型
is  --理解成匿名块的declare
声明部分;
begin
    执行部分; --一定要有return
exception
    异常处理部分;
  end;
```

注意事项:┌函数的形参只需要数据类型不需要数据类型长度  , 比如varchar2即可,但是声明部分一定要给哦
         ├函数的形参不能再被赋值--参数不可以直接赋值,可以在过程中用个中间变量接着赋值,out的参数可以直接赋值
         ├return关键字后面只要放类型即可,一个函数只能有一个return
         ├[OR REPLACE] 是什么意思？它是“如果函数已存在，就覆盖它”的意思,最好加上。
         ├[IN | OUT | IN OUT]    参数模式三选一，默认 IN   输入用 IN，输出用 OUT，既读又改用 IN OUT
         ├在 Oracle PL/SQL 中，函数一定要有 RETURN 语句，而且必须保证每条执行路径最终都会执行到 RETURN（或者抛出异常）
         └函数一般没有out参数,out参数都是放在存储过程中

示例1:创建一个自定义函数,功能是相反数
```sql
CREATE OR REPLACE FUNCTION MY_XFS(num1 number) return NUMBER
IS 
    num2 number;
BEGIN
    num2 := -num1;
    return num2;
END;
```

在左边函数里面就能看到,可以点击运行,但是也可以用代码调用
-- 在 SQL 中调用,参数切记一一对应
```sql
select my_xfs(10) from dual;      -- 输出 -10
select my_xfs(-6) from dual;      -- 输出 6
```


示例2:
写一个函数
传入：部门编号
返回：如果部门存在且有员工，返回66，
      如果部门存在却没员工，返回88，
      如果部门不存在，报错'部门编号错误'
```sql
create or replace function fun3(v_d number) return number
IS
    v_deptno dept.deptno%type;
    v_num number;
BEGIN
    select deptno into v_deptno
    from DEPT
    where deptno = v_d;


    SELECT COUNT(1) into v_num
    from emp
    WHERE deptno =v_d;
    if v_num = 0 then return 88;
    else return 66;
    end if;

EXCEPTION
    when no_data_found THEN raise_application_error(-20001,'部门编号错误');
END;
```
调用
```sql
SELECT fun3(40)  FROM dual;
```

示例3:写一个函数
返回值:全表工资最低的员工所在部门的所有人的名字(用逗号隔开)

```sql
create or replace function fun2 return varchar2
is
v_name varchar2(100);
begin
    select wm_concat(ename) into v_name
    from emp
    where deptno in (select deptno
                     from emp
                     where sal = (select min(sal)
                                  from emp));
    return v_name;
end;
```

调用
```sql
select fun2
from dual
```

示例4:写一个函数，输入两个数，返回这两个数的最大公约数
```sql
create or replace function fun4(v_11 int ,v_22 int) return int 
IS 
v_1 int := v_11;
v_2 int := v_22;
v_3 int;
BEGIN
        loop
            v_3 := mod(v_1,v_2);
            exit WHEN v_3 = 0;
            v_1:=v_2;
            v_2:=v_3;
            end loop;
            return v_2;
END;
```





#### 存储过程
二:存储过程    就是没有返回值的函数
语法:
```sql
create [or replace] procedure 函数名[(参数 [in]|out,参数2..)]
is
声明部分;
begin
    执行部分;
exception
    异常处理部分;
  end;
```


--调用存过
┌无参数     直接调用    call pro1()
├有in参数   传参调用    call pro1(参,参2..)
└有out参数  匿名块      略

示例:
编写一个存过，输入一个数字和一个部门编号，
要求数字必须是0-9的整数（如果不是抛出异常，报错'请输入0-9的整数'）
如果输入的编号emp1表不存在，抛出异常，报错'部门错误'
当部门人数小于该数字，则将部门的员工信息插入到emp_kk，
并打印插入了多少行
当部门人数大于该数字，则将部门的员工姓名、员工编号删除，
并打印删除了几个人
当部门人数等于该数字，则更新该部门的员工工资/100之后再3次方，
并打印更新了几个人的工资
```sql
create table emp_kk as select * from emp  where 1=2;

create or replace procedure pro1(v_number number,v_deptno number)
is
err1 exception;
err2 exception;
v_count number;
begin
    if not regexp_like(v_number,'^[0-9]$') then raise err1;
    end if;

    select COUNT(1) into v_count
    from emp1
    where deptno = v_deptno;
    
    if v_count = 0 then raise err2;
    end if;
    
    if v_count<v_number then insert into emp_kk select * from emp1 where deptno = v_deptno;
        dbms_output.put_line('插入了'||sql%rowcount||'行');
    elsif v_count>v_number then update emp1 set ename = '', empno = '' where deptno = v_deptno;
        dbms_output.put_line('删除了'||sql%rowcount||'行');
    elsif v_count = v_number then update emp1 set sal=power(sal/100,3) where deptno = v_deptno;
        dbms_output.put_line('更新了'||sql%rowcount||'行');
    end if;
    
    commit;

exception
    when err1 then raise_application_error(-20001,'请输入0-9的整数');
    when err2 then raise_application_error(-20002,'部门错误');
end;
```


navicat直接运行会报错的
```sql
CALL pro1(2, 10);
```



有out的题目,其实很鸡肋这个
--输入一个员工编号,输出部门编号和工资
```sql
create or replace procedure pro2(v_empno number,v_deptno out number,v_sal out number)
is
begin
    select deptno,sal into v_deptno,v_sal
    from emp
    where empno = v_empno;

    dbms_output.put_line('存过中的'||v_deptno||' '||v_sal);
end;
```
要使用匿名块调用
```sql
declare 
v1 number;
v2 number;
begin
  pro2(7788,v1,v2);
  dbms_output.put_line('匿名块中的'||v1||' '||v2);   --结果一样,所以out用途不大,不如在上面直接v1,v2
  end;


--创建emp_tt数据同emp
CREATE TABLE emp_tt AS SELECT * FROM emp;
--编写一个存过 输入两个名字 更改他们两人的名字为两人的首字母中间加上+
--输出两个人更新后的名字
--比如 SMITH   KING
--      S+K    K+S
CREATE OR REPLACE PROCEDURE 
 pro_14(v1 VARCHAR2,v2 VARCHAR2,v3 OUT VARCHAR2,v4 OUT VARCHAR2) IS
BEGIN
  UPDATE emp_tt SET
    ename=SUBSTR(v1,1,1)||'+'||SUBSTR(v2,1,1) 
      WHERE ename=v1
        RETURNING ename INTO v3;
  UPDATE emp_tt SET 
    ename=SUBSTR(v2,1,1)||'+'||SUBSTR(v1,1,1)
      WHERE ename=v2
        RETURNING ename INTO v4;
  --commit;
  dbms_output.put_line(v3||' '||v4); 
END;
--调用
DECLARE
v3 VARCHAR2(20);v4 VARCHAR2(20);
BEGIN
  pro_14([$a],[$b],v3,v4);
END;  

SELECT * FROM emp_tt;  
```





















#### 索引
3.3.3.4 建立索引的原则
1、有主键或者唯一约束，自动创建唯一索引
2、在经常用作过滤的字段上建立索引
3、在sql语句中经常用作分组，排序的字段上建立索引
4、经常用作表连接的列要建立索引
5、在不同值较少的字段一般不适合建立b树索引，考虑数据分布
6、对于经常dml的列或者表不适合建立索引
7、避免选择大型数据类型的列建立索引 blob clob
8、小表不适合建立索引
3.3.3.5 建立索引的优缺点
优点：
1、加快数据的检索速度
2、通过使用索引，可以在查询过程中使用优化隐藏器提高系统性能（建索引是优化的一种方式）
3、加速表与表之间的连接
4、通过创建唯一性索引，保证数据的唯一性
缺点：
1、索引占据物理空间，增加了存储的消耗
2、当对有索引的表进行dml操作时，索引的动态维护会降低dml的效率
3、创建和维护索引需要消耗时间，而且随着数据量的增加而增加















#### 数据字典
一:数据字典(视图)        所有对象的目录
┌静态┌dba_*  存储了整个数据库所有的对象的信息--必须拥有管理员权限
│    ├all_*  存储了当前用户能访问的所有对象的信息--不一定属于当前用户
│    └user_* 存储了当前用户所拥有的对象的信息
└动态 v$*

user_tables--所有的表
user_views--所有的视图
user_constraints--所有的约束
user_Sequences--所有的序列
user_Indexes--所有的索引
user_procedures--存过/函数的名字和属性
user_source--所有的有名块
user_recyclebin--回收站

user_tab_cols--用户所有表的所有列的属性
user_tab_columns--用户所有表的所有列的属性


#### 动态SQL
二:动态SQL
语法:
```sql
create or replace procedure pro1(参数..)
authid current_user    --使用当前的用户执行
is
v_sql varchar2(2000);        --动态SQL
begin
    v_sql := 'DDL';
    execute immediate v_sql;   --立即执行v_sql
  end;
```

--子句
·INTO 子句  从动态sql中取值
·USING 子句 向动态sql中传值





#### 随机函数
三:随机函数
```sql
dbms_random.value(数1,数2)--生成数1-数2之间的随机数,默认生成小数
dbms_random.string('格式',数)--生成随机字符串
```
                                    └长度
u 大写字母
l 小写字母
a 字母
x 大写字母和数字
p 可打印字符



#### 闪回
四:闪回
--闪回技术
┌闪回回收站中的表
├闪回到修改前的状态(中间没有更改表定义)
└闪回查询
```sql
--闪回回收站中的表
FLASHBACK TABLE 表名/对象名 TO BEFORE DROP RENAME TO 新表名;
```


```sql
--闪回到一个时间点(时间戳)
FLASHBACK TABLE 表名 TO TIMESTAMP 时间戳;
```
开启了行移动功能
```sql
ALTER TABLE 表名 ENABLE/DISABLE ROW MOVEMENT;


--闪回查询
SELECT 列 
FROM 表 AS OF TIMESTAMP 时间戳
WHERE 条件....
```










┌触发器
└包


#### 触发器
--触发器trigger
DML触发器
语法
```sql
CREATE OR REPLACE TRIGGER tri_name
BEFORE|AFTER--before是事前触发器,after是事后触发器
DML操作 ON 表  [of 字段]
[FOR EACH ROW]--默认语句级,写上是行级
[DECLARE 声明]
BEGIN
  要执行的语句;
END;
```

语句级触发:当某事件发生时,该触发器执行一次
行级触发:  当某事件发生时,影响一行触发一次

子句
┌:old  原数据
└:new  DML操作之后的数据

例如;:old.ename

补充:触发器是为了保证数据的安全
原则:触发器在使用的时候才创建,不用的时候一定要删除

触发器有如下作用：
1）可维护数据库的安全性、一致性和完整性。
2）可在写入数据表前，强制检验或转换数据。
3）当触发器发生错误时，异常的结果会被撤销。
4）部分数据库管理系统可以针对数据定义语言（DDL）使用触发器，称为DDL触发器，还可以针对视图定义替代触发器（INSTEADOF）。
优点：
①　触发器可通过数据库中的相关表实现级联更改。
②　从约束的角度而言，触发器可以定义比CHECK更为复杂的约束。
③　触发器也可以评估数据修改前后的表的状态，并根据其差异采取对策。
④　一个表中的多个同类触发器（INSERT、UPDATE或DELETE）允许采取多个不同的对策以响应同一个修改语句。
缺点：
①　滥用会造成数据库及应用程序的维护困难。在数据库操作中，可以通过关系、触发器、存储过程、应用程序等来实现数据操作，同时，规则、约束、缺省值也是保证数据完整性的重要保障。如果对触发器过分地依赖，那么势必会影响数据库的结构，同时增加了维护的复杂性。
②　一个表上的触发器越多，对于表的DML操作性能影响越大
③　如果触发频率高，占用内存，降低数据访问速度
④　相对不灵活，一旦触发马上执行，不能排除特殊情况
⑤　一定程度上打乱代码结构，相关的代码都需要特别注释，否则造成阅读和维护上的困难
⑥　过度使用也会造成数据库的维护困难





#### 包
--包package
大型项目中,有可能有很多模块,而每个模块又有自己的
存过,函数等.而这些存过,函数,默认是放在一起的,那么
非常不容易查询和维护,甚至可能发生误删情况.
┌包定义 PACKAGE --包头
└包主体 PACKAGE BODY--包体


创建包头的语法:
```sql
create or replace package 包头名
is
--声明
函数;
存过;
变量;
游标;
...
end 包头名;
```

创建包体的语法:
```sql
create or replace package body 包体名
is
--实际的内容
函数;
存过;
变量;
游标;
...
end 包体名;
```


注意:┌同一个包的包头名与包体名必须一样
     └执行时,包头和包体要分开执行,先执行包头

案例:
创建一个包,里面包含fun1 pro1 pro2
--fun1 函数 输入一个员工编号,返回它的部门名称
--pro1 存过 输入一个员工编号,输出部门平均工资
--pro2 存过 输入一个名字,打印他的名字和经理的名字

--创建包头
```sql
create or replace package pck_1
is
  function fun1(v_empno number) return varchar2;
  procedure pro1(v_empno number,v_avg_sal out number);
  procedure pro2(v_ename varchar2);
end pck_1;

--创建包体
create or replace package body pck_1
is
  function fun1(v_empno number) return varchar2
    is
    v_dname varchar2(20);
    begin
      select dname into v_dname
      from dept
      where deptno = (select deptno
                      from emp
                      where empno=v_empno);
      
      return v_dname;
      end;

  procedure pro1(v_empno number,v_avg_sal out number)
    is
    begin
      select avg(sal) into v_avg_sal
      from emp
      where deptno=(select deptno 
                    from emp 
                    where empno=v_empno);
      
      dbms_output.put_line(v_avg_sal);
      end;

  procedure pro2(v_ename varchar2)
    is
    v1 varchar2(20);
    v2 varchar2(20);
    begin
      select e.ename,a.ename into v1,v2
      from emp e --员工表
      join emp a --上级表
      on e.mgr=a.empno
      where e.ename = v_ename;
      dbms_output.put_line('员工名:'||v1||'   经理名:'||v2);
      end;
end pck_1;
```
