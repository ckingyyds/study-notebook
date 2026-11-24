# 简单概念

### 一.数据库分类

关系型数据库:Mysql ,  Oracle,  DB2

非关系型数据库(nosql数据库):MongoDb,Redis,HBase



### 二.小海豚(MySQL3306)

**安装,卸载看视频,看文档**

programData  数据目录

programfiles   安装目录

#### 系统自带客户端工具连接Mysql

连接Mysql--->C:\Program Files\MySQL\MySQL Server 5.7\bin--->cmd--->mysql -u root -p--->密码

show databases;

 

### 三.navicat连接Mysql

简单+

使用navicat本质上还是写sql语句

主机地址--->库--->表--->数据



### 四.sql语言介绍

数据查询语言  DQL      查 

数据操作语言  DML     DML针对数据,增删改,insert,update,delete

数据定义语言  DDL      DDL针对数据库对象,库,表,索引,视图等   create,drop,alter,modify

数据控制语言  DCL      授权 

事务控制语言  TCL       事务控制,确认机制  ACID



sql语言不分大小写,分号结尾





# 数据定义语言  DDL

CREATE：创建数据库对象

ALTER：修改数据库对象(表)

DROP：删除数据库对象

modify:修改(字段)

### 建库和删库

**建库**

命令:CREATE DATABASE  数据库名   Default  Character set   指定编码类型;

​		 CREATE DATABASE  test   Default  Character set   utf8;

软件:右键项目--->新建数据库

**删库**

命令:DROP DATABASE  数据库名 ;

软件:右键项目--->删除数据库

**选择数据库**

命令:use 数据名 ;

软件:双击666



### Mysql的数据类型

##### 整数类型

​				*常用的两个*

​				tinyint(m)   1个字节(-128~127)    

​				int(m)          4个字节(                 )

还有smallint(m), mediumint(m), bigint(m)

m可以不写

m指显示长度,并不表示存储长度，只有字段指定zerofill时有用,如int(3), 长度为3位,如果实际值为2,则显示002

##### 浮点型

​				float(m,d)		单精度浮点型8位精度（4字节）m总个数，d小数位

​				double(m,d)	双精度浮点型16位精度（8字节）m总个数，d小数位

##### 字符类型

​				常用的几个

​				char(n)    固定长度,最多255个字符   适用于定长,会用空格补齐

​				varchar(n)  可变长度,最多65535个字符

​				text   可变长度,最多65535个字符

​				n必须要写

​				这三个从上到下查询速度越来越慢

​				用单引号引起来,引号内严格区分大小写

##### 日期类型

				date  日期YYYY-MM-DD
				time  时间 HH:MM:SS
			datetime  日期时间 YYYY-MM-DD HH:MM:SS
		   timestamp  时间戳YYYYMMDD HHMMSS   跨时区的项目要用时间戳



##### 二进制数据类型(BLOB)



### 建表和删表

**建表**

命令:CREATE TABLE  表名(列名1  数据类型,列名2  数据类型) ;

​		 CREATE TABLE  student(name  varchar(10) , id int,money  float(8,2));

navicat:右键库

**删表**

命令:DROP TABLE  表名 ;

navicat:右键库



### 修改表



##### **修改表名**

**命令: **    ALTER  TABLE  旧表名  RENAME  新表名 ;

**navicat: **  点击表按F2,或者右键重命名



##### 修改列名

**命令:**     ALTER  TABLE   表名  change column  旧列名  新列名  **类型** ;

**navicat:**右键设计表



##### 修改列类型

**命令:**     ALTER  TABLE   表名   modify  列名  新类型 ;

**navicat:**右键设计表



##### 添加新列

**命令:**     ALTER  TABLE   表名   add  column 列名  类型 ;

**navicat:**右键设计表



##### **删除某列**

**命令:**     ALTER  TABLE   表名  drop  column  列名 ;

**navicat:**右键设计表





### Mysql的约束

对表中的数据作出限制,以前你不懂,现在就不多说了



#### 约束类型

##### 	1.主键约束(primary key)

​					**非空非重复**

​					保证数据唯一性

​					一般一张表只设置一个主键



##### 	2.外键约束(Foreign key)

​					**可空可重复**

​					由别的表控制

![image-20260808112118531](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260808112118531.png)

dep_id列的值一定要在id列中,这也是外键约束的结果,可空可重复

外键参照的列一般都是主键列

##### 	3.唯一性约束(Unique)

​					**可空非重复**



##### 	4.非空约束(Not Null)

​					**非空可重复**



##### 	5.检查约束(Check)

​					用户自己定义约束条件

​					Mysql目前不支持检查约束



#### 添加/删除主键约束

##### 1.联合与单一主键

单一主键:只设置单一某列为主键

联合主键:多个列联合成一个主键

例如:    A列        B列 			设置A列和B列为联合主键

数据       1            2 				则.1   2视为一条数据   ,    2     1是视为一条数据

​			   2            1                 1    1不会报错,1     2会报错,重复了

​			   1            1 

​    	       1           2



##### 2.添加主键

*添加主键实际上是修改表*

**命令:**ALTER  TABLE  表名   ADD  primary  key(列名,列名...);

报错:error 1138(22004):Invalid  use  of   NULL  value:  列里有空值或重复值添加主键时会报错

**navicat:**设计表--->确定不允许为空值--->点击键,出现钥匙



##### 3.设置主键自增长

Oracle没有自增长

**要求:**(1)表中只有一个列可以自增长

​		 (2)列必须是整型

​		 (3)自增长只能添加到具有主键约束,唯一性约束的列上,因为不允许有重复值

​		 (4)删除主键或唯一性约束时,如果有自增长能力,需要先去掉自增长,再删除约束

**命令:**ALTER TABLE 表名  modify   键列名  auto_increment;

**Navicat:** 设计表--->点击该列--->下面有自动递增



##### 4.删除主键

去除自增长:ALTER  TABLE  表名  modify   键列名  列类型;

删除主键:ALTER  TABLE  表名  DROP  primary  key;





#### 添加/删除外键约束

##### 1.添加外键约束

**命令:**ALTER   TABLE  表名  ADD   CONSTRAINT  约束名  FOREIGN  KEY(原列名)   REFERENCES   参照的表名(参照的列名);

外键约束需要一个约束名 ,  约束名我们一般起  表名_fk

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260808112118531.png" alt="image-20260808112118531" style="zoom: 67%;" />

**示例:**给dep_id列添加外键约束,让他参照部门表中的id列

alter table emp add constraint emp_fk foreign key (dep_id) references dept(id)

**Navicat**:设计表--->添加外键--->要选别的表和表的列

##### 2.删除外键约束

**命令:**ALTER table  表名  drop foreign  key  约束名

**Navicat:**设计表--->外键--->右键删除



#### 添加/删除唯一性约束

##### 1.添加唯一性约束

**命令:**ALTER TABLE 表名 ADD constraint 约束名  unique(列名);

​	示例:向emp表中的name添加唯一约束

​			alter  table emp add constraint  **emp_uk**  unique(name);

**navicat:**设计表--->索引--->添加索引

##### 2.唯一性约束注意事项

如何查看唯一性约束:设计表--->索引

空格也是单独的一个值,如果两个数据都是空格,那就是重复了

不输入数据是空值NULL

##### 3.删除唯一性约束

**命令:** ALTER  TABLE  表名  Drop  Key  约束名

**Navicat:**设计表--->索引-->右键删除索引



#### 添加/删除非空约束

##### 1.添加非空约束

**命令:**ALTER TABLE  表名  MODIFY  列名  类型  NOT  NULL;

**Navicat:**设计表--->勾选not  null

##### 2.删除非空约束

**命令:**ALTER TABLE  表名  MODIFY  列名  类型    NULL;

**Navicat:**设计表--->取消勾选not  null



#### 创建表时添加约束

之前都是表建好了之后, 通过修改表给列添加约束, 我们现在在创建的表的时候就设计好约束

##### 1.查询表中的约束信息

​	show keys From 表名;

##### 2.命令:

create  table  表名(列名  类型  键名特征等,列名  类型  键名特征等, ...)

##### 3.示例:

创建dept表  ,  含dep_id列为主键且自增,dep_name列唯一性约束,loc_id列为非空约束

create table dept (dep_id  int  primary key  auto_increment,

​								dep_name varchar(30)  unique,

​								loc_id  int   not null								

);

##### 4.Navicat:

新建表--->按需所取





# 数据操作语言 DML

#### 增:添加数据

##### 1.选择插入

**命令:** INSERT  INTO  表名(列名1 , 列名2 , 列名3...)  VALUES(值一 , 值二 , 值三);

向表中的某些选择的列插入数据,而不是全部列

例如: 向depot表中添加一条数据,部门id : 1,部门名称 : market,地点id : 1 

​		 INSERT INTO  dept(dept_id,dept_name,loc_id)   values(1 , "market" ,1);

##### 2.完全插入

**命令**: INSERT  INTO  表名  VALUES(值一 , 值二 , 值三, ......);

插入顺序是根据表的列顺序依次插入,且数量等于列数

自增长的主键列需要用 0 , null ,default  占位

不能同时插入多条数据



#### 默认值处理(default)

也可以理解成约束

##### 1.创建表时指定列的默认值

**命令:**create table 表名(列名 类型  default  默认值,  ......);
可以和创建表时添加约束组合使用,默认值和类型一定要匹配

**示例：**创建 emp3 表，该表包含 emp_id 主键且自动增长，包含 name、包含 address 该列默认值为“未知”。
create table emp3(emp_id int primary key auto_increment,name varchar(10),address varchar(50) default '未知');

##### 2.修改表-添加新列并指定默认值

**命令:**alter table 表名 add column 列名 类型 Default 默认值

**示例:**alter table students_score add column score int Default 0; 

##### 3.修改表-修改字段的数据类型和默认值

**命令:**ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型 DEFAULT 新默认值;
**示例:**将 students 表中 age 字段的默认值修改为 20。
	    ALTER TABLE students MODIFY COLUMN age INT DEFAULT 20;

##### 4.用navicat设置默认值

navicat:设计表--->单击该字段--->下面有默认栏--->如果想设置字符要加引号

##### 插入数据时的默认值处理

如果在插入数据时并未指定该列的值，那么MySQL会将默认值添加到该列中。如果是完全项插入需要使用default来占位。

示例：向emp3表中添加数据，要求address列与job_id列使用默认值作为该列的值。
选择插入:insert into emp3(name) values("admin");
完全插入:insert into emp3 values(default,"oldlu",default,default);



#### 删:删除数据

#### 改:更新数据





# 数据查询语言  DQL

### 一.基础查询练习

#### 1.查询练习（单表条件查询）

**1.显示收入超过 12,000 的雇员的名字和薪水**

```sql
SELECT last_name, salary
FROM employees
WHERE salary > 12000;
```

**2.显示雇员号为 176 的雇员的名字和部门号**

```sql
SELECT last_name, department_id
FROM employees
WHERE employee_id = 176;
```

**3.显示所有薪水不在 5000 和 12000 之间的雇员的名字和薪水**

```sql
SELECT last_name, salary
FROM employees
WHERE salary NOT BETWEEN 5000 AND 12000;
```

**4.显示所有在部门 20 和 50 中的雇员的名字和部门号，并以名字按字母顺序排序**

```sql
SELECT last_name, MANAGER_ID
FROM employees
WHERE department_ID IN (20, 50)
ORDER BY last_name;
```

注：原笔记这一题 SELECT 的是 MANAGER_ID，按题意应该是 department_id。

**5.列出收入在 $5,000 和 $12,000 之间，并且在部门 20 或 50 工作的雇员的名字和薪水，将列标题分别显示为 Employee 和 Monthly Salary**

```sql
SELECT last_name AS Employee,
       salary AS 'Monthly Salary',
       department_id
FROM employees
WHERE salary BETWEEN 5000 AND 12000
  AND department_id IN (20, 50);
```

**6.显示所有没有主管经理的雇员的名字和工作岗位**

```sql
SELECT last_name, job_id
FROM employees
WHERE manager_id IS NULL;
```

**7.显示所有有佣金的雇员的名字、薪水和佣金，以薪水和佣金的降序排序数据**

```sql
SELECT last_name, salary, commission_pct
FROM employees
WHERE commission_pct IS NOT NULL
ORDER BY salary DESC, commission_pct DESC;
```

**8.显示所有名字中有一个 a 和一个 e 的雇员的名字**

```sql
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%'
  AND last_name LIKE '%e%';
```

**9.显示所有工作岗位是销售代表（SA_REP）或者普通职员（ST_CLERK），并且薪水不等于 2,500、3,500 或 7,000 的雇员的名字、工作岗位和薪水**

```sql
SELECT last_name, job_id, salary
FROM employees
WHERE (job_id = 'SA_REP' OR job_id = 'ST_CLERK')
  AND salary NOT IN (2500, 3500, 7000);
```

### 二.单行函数

单行函数：对每一行数据计算一次、返回一个结果，和聚合函数（把多行压成一行）正好相反。

#### 1.字符函数

##### （1）大小写处理函数

ucase() 转大写，lcase() 转小写。

```sql
SELECT employee_id, ucase(last_name), department_id
FROM employees
WHERE last_name = 'davies';
```

注：where 里比较字符串时严格区分大小写，'davies' 和 'dAVies' 不是同一个值。

##### （2）length 函数：返回字符串长度

```sql
SELECT employee_id,
       length(last_name) AS CHANGDU,
       department_id
FROM employees
WHERE last_name = 'dAVies';
```

##### （3）concat 函数：合并字符串

```sql
SELECT employee_id,
       concat(first_name, last_name, salary) AS "名字",
       department_id
FROM employees
WHERE last_name = 'dAVies';
```

##### （4）lpad 函数：左填充；rpad 函数：右填充

```sql
SELECT lpad(last_name, 10, 'M'),
       rpad(last_name, 10, 'M')
FROM employees
WHERE last_name = 'dAVies';
```

##### （5）ltrim 函数：去除左边的空格；rtrim：去除结尾的空格；trim：去除两边的空格

```sql
SELECT ltrim('     Zyc牛逼     '),
       rtrim('     Zyc牛逼     '),
       trim('     Zyc牛逼     ');
```

##### （6）replace 函数：替换

语法 `replace(前, 中, 后)`，把「前」里面的「中」替换成「后」。

```sql
SELECT replace('ABCDEFG', 'ABCD', 'abcd');
-- 结果：abcdEFG
```

##### （7）reverse 函数：反转

```sql
SELECT reverse('asdaf');
```

##### （8）substr 函数：截取字符串

`substr(a, b, c)`：从 a 的 b 位置开始截取 c 长度的字符串；如果没有 c 参数，那就是截取到尾。

```sql
SELECT substr('abcdefg', 2, 3),
       substr('abcdefg', 3, 2);
```

##### （9）instr 函数：返回字符在字段中的位置

`instr(a, b)`：返回 a 字段中 b 的位置（从 1 开始）。

```sql
SELECT instr('abcdefghijk', 'b');
```

##### （10）字符函数练习

显示所有工作岗位名称从第 4 个字符位置开始、包含字符串 REP 的雇员的 ID 信息：将雇员的姓和名连接显示在一起，还显示雇员名字的长度，以及名字中字母 a 的位置。

```sql
SELECT job_id,
       employee_id,
       concat(last_name, first_name) AS name,
       length(last_name) AS length,
       instr(last_name, 'a') AS 'a location'
FROM employees
WHERE substr(job_id, 4) = 'REP';
```

#### 2.数字函数

**mod(m, n) 函数**：找出 m 除以 n 的余数。

示例：所有 job_id 是 SA_REP 的雇员的名字、薪水，以及薪水被 5000 除后的余数。

```sql
SELECT job_id, last_name, salary, MOD(salary, 5000) AS number
FROM employees
WHERE job_id = 'SA_REP';
```

#### 3.日期函数

**sysdate() 函数**：取当前日期时间。

```sql
SELECT sysdate();
```

**datediff(d1, d2) 函数**：计算 d1 和 d2 之间相隔的天数（d1 - d2）。

示例一：向 employees 表中添加一条数据，雇员 ID 304，添加完后查询。

```sql
-- 添加语句（原笔记中注释掉了）：
-- insert into employees(employee_id, last_name, email, hire_date, job_id)
-- values (304, 'joke', 'joke@sxt.cn', '2049-5-1 8:30:3', 'IT_PROG');

SELECT * FROM employees WHERE employee_id = 304;
```

示例二：显示所有在部门 90 中的雇员的名字和从业的周数。雇员的总工作时间以周计算，用当前日期（sysdate）减去雇员的受雇日期，再除以 7。

```sql
SELECT last_name,
       datediff(sysdate(), hire_date) / 7 AS weeks
FROM employees
WHERE department_id = 90;
```

#### 4.转换函数

**str_to_date(字符串, 模板) 函数**：字符串转换成日期。

模板的格式要和日期字符串一致，比如模板 `%Y年 %m月 %d日` 就对应 `2049年5月1日` 这种写法，前面的汉字要和后面一样，一一对印匹配格式。

```sql
INSERT INTO employees(employee_id, last_name, email, hire_date, job_id)
VALUES (400, 'oldlu', 'oldlu@sxt.cn', str_to_date('2049年5月1日', '%Y年 %m月 %d日'), 'IT_PROG');

INSERT INTO employees(employee_id, last_name, email, hire_date, job_id)
VALUES (401, 'younglu', 'younglu@sxt.cn', str_to_date('2049年5月1日 17点45分23秒', '%Y年 %m月 %d日 %H点 %i分 %S秒'), 'IT_PROG');

SELECT * FROM employees WHERE employee_id = 400;
SELECT * FROM employees WHERE employee_id = 401;
```

**date_format(日期, 模板) 函数**：日期转换成字符串。

示例：查询 employees 表中雇员名字为 King 的雇员的入职日期，要求显示格式为 yyyy年MM月dd日。

```sql
SELECT date_format(hire_date, '%Y年 %m月 %d日')
FROM employees
WHERE last_name = 'King';
```

#### 5.通用函数

##### （1）isnull(expr)

判断 expr 是否为空值，返回 true 或者 false。

##### （2）if(expr, v1, v2)

expr 成立返回 v1，否则返回 v2。注意 if 的 expr 判断的是「是否成立」，而不是「是不是空值」。

##### （3）ifnull(expr, v)

expr 为空则返回 v，不为空则返回 expr 本身。

##### （4）nullif(expr1, expr2)

两个值相等返回 NULL，不相等返回 expr1。

##### （5）coalesce(v1, v2, ...)

从左往右找，返回第一个非空的值。

##### （6）示例一

查询部门编号是 50 或者 80 的员工信息，包含他们的名字、薪水、佣金。在 income 新列中，如果有佣金则显示 'SAL+COMM'，无佣金则显示 'SAL'。

思路：先判断是不是空用 isnull(expr)，再用 if(expr, v1, v2) 选择 sal 或者 sal+comm。if 的 expr 判断的是是否成立，而不是是不是空值，而 isnull 返回的是判断 expr 是否为空后的 true 或者 false。

```sql
SELECT department_id, last_name, salary, commission_pct,
       if(isNULL(COMMISSION_PCT), 'sal', 'sal+comm') AS income
FROM employees
WHERE department_id IN (50, 80);
```

##### （7）示例二

计算雇员的年报酬：需要用 12 乘以月薪，再加上佣金（等于年薪乘以佣金）。有的人月薪是空值，不能直接用 commission_pct 计算，得把空换成 0。

错误写法（别名不能这样用）：

```sql
SELECT concat(first_name, last_name) AS name,
       ifnull(COMMISSION_PCT, 0) AS comm,
       (salary * 12) + (salary * 12 * comm) AS year_salary
FROM employees;
```

在这里用 comm 别名会报错（1054 错误）。原因：虽然官方文档说 SELECT 列表中后面的表达式可以引用前面的别名，但在某些情况（比如算术表达式）下，解析器会优先把 comm 当作表的列名去查找，而不是你刚定义的别名。由于 employees 表中没有 comm 这一列，就报了 1054 错误。

正确写法，直接使用表达式：

```sql
SELECT concat(first_name, last_name) AS name,
       ifnull(COMMISSION_PCT, 0) AS comm,
       (salary * 12) + (salary * 12 * ifnull(COMMISSION_PCT, 0)) AS year_salary
FROM employees;
```

##### （8）示例三

查询员工表，显示他们的名字、名字的长度（该列名为 expr1）、姓氏、姓氏的长度（该列名为 expr2）。在 result 列中，如果名字与姓氏的长度相同则显示空，如果不相同则显示名字长度。

```sql
SELECT first_name,
       LENGTH(first_name) AS expr2,
       last_name,
       LENGTH(last_name) AS expr1,
       nullif(LENGTH(last_name), LENGTH(first_name))
FROM employees;
```

报错原因和上面相同（不能在同一个 SELECT 列表里引用刚定义的别名），所以这里直接使用表达式。

##### （9）示例四

查询员工表，显示他们的名字：如果 COMMISSION_PCT 值是非空，显示它；如果 COMMISSION_PCT 值是空，则显示 SALARY；如果 COMMISSION_PCT 和 SALARY 值都是空，那么显示 10。在结果中对佣金列升序排序。

```sql
SELECT last_name,
       coalesce(COMMISSION_PCT, salary, 10) AS comm,
       COMMISSION_PCT
FROM employees
ORDER BY COMMISSION_PCT;
```

##### （10）示例五（case 表达式）

如果 JOB_ID 是 IT_PROG，薪水增加 10%；如果 JOB_ID 是 ST_CLERK，薪水增加 15%；如果 JOB_ID 是 SA_REP，薪水增加 20%；对于所有其他的工作角色，不增加薪水。

```sql
SELECT last_name,
       CASE job_id
           WHEN 'IT_PROG'  THEN salary * 1.1
           WHEN 'ST_CLERK' THEN salary * 1.15
           WHEN 'SA_REP'   THEN salary * 1.2
           ELSE salary
       END AS 'new salary'
FROM employees;
```

#### 6.单行函数练习

**1.显示受雇日期在 1998 年 2 月 20 日和 2005 年 5 月 1 日之间的雇员的名字、岗位和受雇日期，按受雇日期顺序排序查询结果**

```sql
SELECT concat(first_name, '   ', last_name) AS name, job_id, hire_date
FROM employees
WHERE hire_date BETWEEN '1998-02-20' AND '2005-05-01'
ORDER BY hire_date ASC;
```

**2.显示每一个在 2002 年受雇的雇员的名字和受雇日期**

```sql
SELECT concat(first_name, '   ', last_name) AS name, hire_date
FROM employees
WHERE hire_date BETWEEN '2002-01-01' AND '2002-12-31'
ORDER BY hire_date;
```

也可以想到用 like 来做：

```sql
SELECT concat(first_name, '   ', last_name) AS name, hire_date
FROM employees
WHERE hire_date LIKE '2002%'
ORDER BY hire_date;
```

**3.对每一个雇员，显示 employee number、last_name、salary 和 salary 增加 15%，并且表示成整数，列标签显示为 NewSalary**

```sql
SELECT employee_id AS 'employee number',
       last_name,
       salary,
       TRUNCATE(1.15 * salary, 0) AS NewSalary
FROM employees;
```

**4.写一个查询，显示名字的长度，对所有名字开始字母是 J、A 或 M 的雇员，用雇员的 last_name 排序结果**

```sql
SELECT last_name, LENGTH(last_name)
FROM employees
WHERE last_name LIKE 'M%'
   OR last_name LIKE 'A%'
   OR last_name LIKE 'J%'
ORDER BY last_name;
```

这里整体用 or 连接。

**5.创建一个查询，显示所有雇员的 last name 和 salary，将薪水格式化为 15 个字符长度，用 $ 左填充**

```sql
SELECT last_name, lpad(salary, 15, '$')
FROM employees;
```

**6.创建一个查询，显示雇员的 last_name 和 commission（佣金）比率，如果雇员没有佣金，显示 "No Commission"，列标签 COMM**

```sql
SELECT last_name, ifnull(COMMISSION_PCT, 'No Commision') AS COMM
FROM employees;
```

原笔记中拼写为 "No Commision"，按题目应该是 "No Commission"。

**7.写一个查询，按照 JOB_ID 列值的级别显示所有雇员**

```sql
SELECT job_id,
       CASE job_id
           WHEN 'AD_PRES'  THEN 'A'
           WHEN 'ST_MAN'   THEN 'B'
           WHEN 'IT_PROG'  THEN 'C'
           WHEN 'SA_REP'   THEN 'D'
           WHEN 'ST_CLERK' THEN 'E'
           ELSE '0'
       END AS jibie
FROM employees;
```

### 三.多表查询

多表查询有两种语法标准：SQL92（from 后面用逗号把表列出来，连接条件写在 where 里）和 SQL99（用 join ... on 连接）。

多表查询的意义：在多个表中查询数据时，把多个表连接在一起查。

#### 1.SQL92 标准

##### （1）等值连接

```sql
SELECT employees.employee_id,
       employees.last_name,
       employees.department_id,
       departments.department_id,
       departments.location_id
FROM employees, departments
WHERE employees.department_id = departments.department_id
  AND employees.department_id = 60;
```

**示例一：查询雇员 King 所在的部门名称**

```sql
SELECT emp.last_name, dep.department_name
FROM employees AS emp, departments AS dep
WHERE emp.department_id = dep.department_id   -- 条件一是等值连接的链接条件，查询的是部门 id 相同的信息
  AND emp.last_name = 'King';                 -- 条件二是限制搜索条件为 King
```

**示例二：显示每个员工的 last_name、department_name 和 city**

查找发现 city 在 locations 表里面。

```sql
SELECT last_name, department_name, city    -- 这些表中特有的字段前面不用加前缀
FROM employees AS emp, departments AS dep, locations AS loc
WHERE emp.department_id = dep.department_id           -- 用 and 连接多个条件
  AND loc.location_id = dep.location_id;              -- 寻找 loc 表和其他表的相同字段，发现一个 location_id 列
```

##### （2）自连接

同一个表看成两张表，分别是员工表和经理表，所以自连接时表别名很重要。

**示例一：查询每个雇员的经理的名字以及雇员的名字，雇员名字列别名为 W，经理列别名为 M**

```sql
SELECT worker.last_name AS W,
       manager.last_name AS M,
       worker.MANAGER_ID
FROM employees AS worker, employees AS manager
WHERE worker.MANAGER_ID = manager.EMPLOYEE_ID;   -- 员工表的经理 id = 经理表的员工 id，这就找到了员工对应的经理
```

这样查出的是员工对应的经理，但是经理是没有经理的（经理自己的 MANAGER_ID 为空）。

**示例二：查询 Fox 的经理是谁，显示他的名字**

员工对应的经理，再加别的附加条件就能查到。

```sql
SELECT worker.last_name AS worker, manager.last_name AS manager
FROM employees AS worker, employees AS manager
WHERE worker.MANAGER_ID = manager.EMPLOYEE_ID
  AND worker.last_name = 'Fox';
```

##### （3）非等值连接

连接条件不是等号，而是 between、大于、小于之类的比较。

**示例一：创建 job_grades 表，包含 lowest_sal、highest_sal、grade_level**

```sql
CREATE TABLE job_grades(
    lowest_sal INT,
    highest_sal INT,
    grade_level VARCHAR(30)
);
```

**示例二：插入数据**

```sql
INSERT INTO job_grades VALUES (1000, 2999, 'A');
INSERT INTO job_grades VALUES (2000, 4999, 'B');
INSERT INTO job_grades VALUES (5000, 7999, 'C');
INSERT INTO job_grades VALUES (8000, 12000, 'D');
```

**示例三：查询所有雇员的薪水级别**

```sql
SELECT emp.last_name, j_g.grade_level
FROM employees AS emp, job_grades AS j_g
WHERE emp.salary BETWEEN j_g.lowest_sal AND j_g.highest_sal;   -- 条件是非等号
```

对 emp 里面的 salary 字段与 j_g 里面的 lowest_sal、highest_sal 字段作比较。

#### 2.SQL99 标准

##### （1）交叉连接（cross join）

了解就行，写出来的是笛卡尔乘积。

```sql
SELECT *
FROM employees
CROSS JOIN departments;
```

##### （2）内连接

```sql
SELECT e.employee_id,
       e.last_name,
       e.department_id,
       d.department_id,
       d.location_id
FROM employees AS e                 -- 表一
JOIN departments d                  -- 连接的表二，如果表一和表二一样，那又相当于 SQL92 的自连接
ON e.department_id = d.department_id;   -- ON 后面的是连接条件
```

ON 后面的是连接条件：连接条件为等值连接的条件，在这里用等号相当于 SQL92 的等值连接；如果用大于小于之类的非等号，就相当于非等值连接。

用 inner join ... on 还可以创建更多张表的连接：

```sql
SELECT employee_id, city, department_name      -- 都是特有的列，不用前缀
FROM employees AS e
JOIN departments AS d                          -- employees 表连接 departments 表
ON d.department_id = e.department_id
 AND d.manager_id = e.manager_id
JOIN locations AS l                            -- departments 表连接 locations 表
ON d.location_id = l.location_id;
```

**示例：查询雇员名字为 Fox 的雇员 ID、薪水与部门名称**

```sql
SELECT emp.department_id, emp.salary, dep.department_name
FROM employees AS emp
INNER JOIN departments AS dep
ON dep.department_id = emp.department_id
WHERE emp.last_name = 'Fox';          -- on 后面的连接条件以外的附加条件，在后面用 where 语句
```

##### （3）外连接

外连接中的 outer 关键字去掉也可以。

**左外连接**

```sql
SELECT e.last_name, e.department_id, d.department_name
FROM employees AS e
LEFT OUTER JOIN departments AS d
ON e.department_id = d.department_id;
```

left 关键字的左边是 employees 表，所以左外连接会把 employees 表中的孤儿数据显示出来。查看结果会发现，原来用等值连接查不到的雇员 Grant（他没有 department_id）也被查了出来，Grant 就是 employees 里面的孤儿数据。

左边的表（EMPLOYEES）中即使没有与 DEPARTMENTS 表中匹配的行，该查询也会取回 EMPLOYEES 表中所有的行。

**示例：查询名字为 G 开头的雇员的名字以及他们的部门名称，包含那些没有部门的雇员**

```sql
SELECT emp.last_name, dep.department_name
FROM employees AS emp                                       -- 因为要查雇员，所以把 employees 表放左边
LEFT OUTER JOIN departments AS dep                          -- 连接条件
ON emp.department_id = dep.department_id
WHERE emp.last_name LIKE 'G%';
```

**右外连接**

示例：查询所有雇员的名字以及他们的部门名称，包含那些没有雇员的部门。

查部门啊，部门放右边。

```sql
SELECT dep.department_name, emp.last_name
FROM employees AS emp
RIGHT OUTER JOIN departments AS dep
ON emp.department_id = dep.department_id;
```

记住：查什么，就把那张表放对应关键字的那边。

**全外连接**

示例：查询所有雇员的名字以及他们的部门名称，包含那些没有雇员的部门以及没有部门的雇员。MySQL 不支持 full outer join，用左外 + 右外 + union 来模拟。

```sql
(SELECT emp1.last_name, dep1.department_name
 FROM departments AS dep1
 LEFT JOIN employees AS emp1
 ON dep1.department_id = emp1.department_id)

UNION

(SELECT emp2.last_name, dep2.department_name
 FROM departments AS dep2
 RIGHT JOIN employees AS emp2
 ON dep2.department_id = emp2.department_id);
```

##### （4）自然连接

两个表都有 location_id 这个名字的列，并且两个表的 location_id 数据类型相同，所以可以用 location_id 作为连接条件。写法没有 where，from ... natural join ...，两个表的顺序好像都行。

```sql
SELECT department_id, department_name, location_id, city
FROM locations
NATURAL JOIN departments;
```

但是这样查出来的结果和 SQL92 等值连接没有什么区别。果然，自然连接也可以被写成等值连接，算是一个等值连接的简化写法，实际用的不多。

等值连接写法：

```sql
SELECT dep.department_id, dep.department_name, loc.location_id, loc.city
FROM departments AS dep, locations AS loc
WHERE dep.location_id = loc.location_id;
```

**示例：使用自然连接查询所有部门的雇员的名字以及部门名称**

```sql
SELECT dep.department_name, emp.last_name
FROM departments AS dep
NATURAL JOIN employees AS emp;
```

简单好使。等值连接写法：

```sql
SELECT dep.department_name, emp.last_name
FROM departments AS dep, employees AS emp
WHERE dep.department_id = emp.department_id
  AND dep.manager_id = emp.manager_id;
```

#### 3.多表查询练习

写框架：分析来自哪几张表，确认连接条件。

**1.写一个查询显示所有雇员的 last name、department id 和 department name**

```sql
SELECT emp.last_name, dep.department_id, dep.department_name
FROM employees AS emp, departments AS dep
WHERE dep.department_id = emp.department_id;
```

**2.创建一个在部门 80 中的所有工作岗位的唯一列表，在输出中包括部门的地点**

distinct 去重。

```sql
SELECT DISTINCT job_id, loc.location_id, city
FROM employees AS emp, departments AS dep, locations AS loc
WHERE emp.department_id = dep.department_id
  AND dep.location_id = loc.location_id
  AND dep.department_id = 80;
```

**3.写一个查询显示所有有佣金的雇员的 last name、department name、location ID 和城市**

三张表，起码两个连接条件。

```sql
SELECT DISTINCT emp.last_name, emp.department_id, loc.location_id, city, emp.commission_pct
FROM employees AS emp, departments AS dep, locations AS loc
WHERE emp.department_id = dep.department_id
  AND dep.location_id = loc.location_id
  AND commission_pct IS NOT NULL;
```

**4.显示所有在其 last names 中有一个小写 a 的雇员的 last name 和 department name**

```sql
SELECT dep.department_name, emp.last_name
FROM departments AS dep, employees AS emp
WHERE dep.department_id = emp.department_id
  AND last_name LIKE '%a%';
```

**5.用内连接写一个查询显示那些工作在 Toronto 的所有雇员的 last name、job_id、department id 和 department name**

```sql
SELECT emp.last_name, emp.job_id, emp.department_id, dep.department_name
FROM departments AS dep, employees AS emp, locations AS loc
WHERE dep.department_id = emp.department_id
  AND dep.location_id = loc.location_id
  AND city = 'Toronto';
```

SQL99 写法：

```sql
SELECT emp.last_name, emp.job_id, emp.department_id, dep.department_name
FROM departments AS dep
INNER JOIN employees AS emp
ON dep.department_id = emp.department_id
INNER JOIN locations AS loc
ON dep.location_id = loc.location_id
WHERE city = 'Toronto';
```

**6.显示雇员的 last name 和 employee id，连同他们的经理的 last name 和 manager id，列标签分别为 Employee、Emp、Manager 和 Mgr**

```sql
SELECT worker.last_name AS Employee,
       worker.employee_id AS Emp,
       manager.last_name AS Manager,
       manager.manager_id AS Mgr
FROM employees AS worker, employees AS manager
WHERE worker.manager_id = manager.employee_id;
```

SQL99 写法：

```sql
SELECT worker.last_name, worker.employee_id, manager.last_name, manager.manager_id
FROM employees AS worker
INNER JOIN employees AS manager
ON worker.manager_id = manager.employee_id;
```

### 四.聚合函数

#### 1.五个聚合函数

聚合函数会把多行数据压缩成一行。

##### （1）sum 和 avg 函数

```sql
SELECT SUM(salary)
FROM employees;
```

注意：salary 是非聚合列且没有出现在 GROUP BY 中（GROUP BY，对数据分组）时会报错。数据库不知道应该返回哪一条记录的 salary（因为聚合函数会将多行压缩成一行），所以报错。

```sql
SELECT salary, SUM(salary)
FROM employees;
```

类型不对会返回 0：

```sql
SELECT AVG(last_name)
FROM employees;
```

distinct 的使用（先去重再聚合）：

```sql
SELECT SUM(DISTINCT salary) FROM employees;
```

**示例：计算员工表中工作编号含有 REP 的工作岗位的平均薪水与薪水总和（多条聚合函数写法）**

```sql
SELECT AVG(salary), SUM(salary)
FROM employees
WHERE job_id LIKE '%REP%';
```

##### （2）min 和 max 函数

可以和子查询嵌套：

```sql
SELECT last_name, hire_date
FROM employees
WHERE hire_date = (SELECT MIN(hire_date) FROM employees);
```

**示例：查询员工表中入职时间最短与最长的员工，并显示他们的入职时间**

```sql
SELECT MIN(hire_date), MAX(hire_date)
FROM employees;
```

##### （3）count 函数：返回分组中的总行数

count 函数有三种格式：

1. COUNT(*)：返回表中满足 SELECT 语句的所有列的行数，包括重复行，包括有空值列的行。

```sql
SELECT COUNT(*)
FROM employees;
```

实际上就是员工表中的员工数量。

注：原笔记这里写的是 `select (*)`，漏了 count。

2. COUNT(expr)：返回在列中的由 expr 指定的非空值的数。

```sql
SELECT COUNT(COMMISSION_PCT)
FROM employees;
```

实际上就是员工表中有佣金的员工数量。

3. COUNT(DISTINCT expr)：返回在列中的由 expr 指定的唯一的非空值的数。

```sql
SELECT COUNT(JOB_ID) FROM employees;            -- 114 个 job_id
SELECT COUNT(DISTINCT JOB_ID) FROM employees;   -- 19 种 job_id
```

**示例：显示员工表中部门编号是 80 中有佣金的雇员人数**

```sql
SELECT COUNT(COMMISSION_PCT)
FROM employees
WHERE department_id = 80;
```

**显示员工表中的部门数**

```sql
SELECT COUNT(DISTINCT department_id)
FROM employees;
```

##### （4）在聚合函数中使用 ifnull 函数

```sql
-- 查出所有有佣金的员工的佣金和的平均值，反映的是有佣金记录的员工的平均佣金比例
SELECT AVG(COMMISSION_PCT)
FROM employees;

-- 查出所有员工（包括没有佣金的）的佣金和的平均值，反映的是全体员工的平均佣金比例
SELECT AVG(IFNULL(COMMISSION_PCT, 0))
FROM employees;
```

#### 2.数据分组 group by

查每个部门的平均薪水，用 group by 按照部门 id 进行分组。

```sql
SELECT emp.department_id, dep.department_name, AVG(salary)
FROM employees AS emp, departments AS dep
WHERE emp.department_id = dep.department_id     -- 使用 WHERE 子句，可以在划分行成组以前过滤行
GROUP BY department_id;                          -- GROUP BY 必须写最后，如果有 order by 要写在 order by 之前
```

有 group by 分组后就可以写别的列了，但是要和 group by 后面的列相同或有关。在 GROUP BY 子句中必须包含列。

**示例：计算每个部门的员工总数**

```sql
SELECT department_id, COUNT(employee_id) AS number
FROM employees
GROUP BY department_id;
```

查全部最好用 *：

```sql
SELECT department_id, COUNT(*) AS number
FROM employees
GROUP BY department_id;
```

聚合函数不能嵌套使用，比如 count 里面不能写 sum。

**在多列上使用 GROUP BY 子句**

```sql
SELECT department_id, SUM(salary)
FROM employees
GROUP BY department_id;

SELECT department_id, job_id, SUM(salary)
FROM employees
GROUP BY department_id, job_id;
```

先对 department_id 进行分组，然后对每组 department_id 里面的 job_id 进行分组。和上面一个查询作比较会发现：例如部门 id 为 20 的，这次被 job_id 分成两组，而两组的 salary 和就是上个查询中部门 id = 20 的 salary。

**示例：计算每个部门的不同岗位的员工总数**

```sql
SELECT department_id, job_id, COUNT(*)
FROM employees
GROUP BY department_id, job_id;
```

#### 3.约束分组结果 having

HAVING 子句是对查询出结果集分组后的结果进行过滤。

- 和 where 不一样：where 不能用聚合函数；用 WHERE 子句筛选选择的行，用 HAVING 子句筛选组。
- having 后面都是聚合函数和条件，写在 where 后面。

为了找到每个部门中的最高薪水，而且只显示最高薪水大于 $10,000 的那些部门：用部门号分组，在每个部门中找最大薪水，返回那些有最高薪水大于 $10,000 的雇员的部门。

```sql
SELECT department_id, MAX(salary)
FROM employees
GROUP BY department_id
HAVING MAX(salary) > 10000;      -- 筛选出来的是组
```

**示例：显示那些合计薪水超过 13,000 的每个工作岗位的合计薪水。排除那些 JOB_ID 中含有 REP 的工作岗位，并且用合计月薪排序列表**

```sql
SELECT job_id, SUM(salary)
FROM employees
WHERE job_id NOT LIKE '%REP%'
GROUP BY job_id
HAVING SUM(salary) > 13000
ORDER BY SUM(salary);
```

各关键字的顺序：from → where → group by → having → order by → limit。

#### 4.聚合函数练习

**1.显示所有雇员的最高、最低、合计和平均薪水，列标签分别为 Max、Min、Sum 和 Avg，四舍五入结果为最近的整数**

```sql
SELECT ROUND(MAX(salary), 0) AS Max,
       ROUND(MIN(salary), 0) AS Min,
       ROUND(SUM(salary), 0) AS Sum,
       ROUND(AVG(salary), 0) AS Avg
FROM employees;
```

**2.写一个查询显示每一工作岗位的人数**

```sql
SELECT job_id, COUNT(*)
FROM employees
GROUP BY job_id;
```

**3.确定经理人数，不需要列出他们，列标签是 NumberofManagers。提示：用 MANAGER_ID 列决定经理号**

```sql
SELECT COUNT(DISTINCT MANAGER_ID) AS NumberofManagers
FROM employees;
```

**4.写一个查询显示最高和最低薪水之间的差**

```sql
SELECT MAX(salary) - MIN(salary) AS cha
FROM employees;
```

**5.显示经理号和经理付给雇员的最低薪水。排除那些经理未知的人，排除最低薪水小于等于 $6,000 的组，按薪水降序排序输出**

注意是排除组，还要用 having。

```sql
SELECT MANAGER_ID, MIN(salary)
FROM employees
WHERE MANAGER_ID IS NOT NULL
GROUP BY MANAGER_ID
HAVING MIN(salary) > 6000
ORDER BY MIN(salary) DESC;
```

在启用了 ONLY_FULL_GROUP_BY 的 MySQL 中，不仅 SELECT 列表中的非聚合列受限制，ORDER BY 中的非聚合列同样受限制，所以要用 MIN(salary) 而不是 salary。

**6.写一个查询显示每个部门的名字、地点、人数和部门中所有雇员的平均薪水，四舍五入薪水到两位小数**

```sql
SELECT d.department_name, d.location_id, COUNT(*), ROUND(AVG(e.salary), 2)
FROM employees AS e, departments AS d
WHERE e.department_id = d.department_id
GROUP BY d.department_name, d.location_id;     -- 反正这里一个部门只有一个地点
```

不对 location_id 分组，select 里面就不能查 location_id，下面这样写会报错：

```sql
SELECT d.department_name, d.location_id, COUNT(*), ROUND(AVG(e.salary), 2)
FROM employees AS e, departments AS d, locations AS l
WHERE e.department_id = d.department_id
GROUP BY d.department_name;
```

### 五.子查询

#### 1.子查询简介

**谁的薪水比 Abel 高**

```sql
SELECT last_name
FROM employees
WHERE salary > (SELECT salary
                FROM employees
                WHERE last_name = 'Abel');
```

**使用子查询的原则**

- 子查询放在圆括号中。
- 将子查询放在比较条件的右边。
- 子查询返回的行数是单行就是单行子查询，返回的行数是多行就是多行子查询。
- 在单行子查询中用单行运算符（>、<、!= 之类的），在多行子查询中用多行运算符（in 之类）。
- 可以写在 having、where、from 里面。

**示例：查询与 Fox 同一部门的同事，并显示他们的名字和部门 ID**

```sql
SELECT last_name, department_id
FROM employees
WHERE department_id = (SELECT department_id
                       FROM employees
                       WHERE last_name = 'Fox');
```

#### 2.单行子查询

仅返回一行，使用单行比较符 >、=、>=、<、<>。

**示例：查询 Fox 的同事，但不包含他自己**

```sql
SELECT last_name, department_id
FROM employees
WHERE department_id = (SELECT department_id
                       FROM employees
                       WHERE last_name = 'Fox')
  AND last_name <> 'Fox';
```

单行子查询返回的结果集只能是一行，子查询返回的也不能是多列。

#### 3.多行子查询

返回的结果多于一行，使用多行比较符：
- in：等于列表中的任何成员；
- any：比较子查询返回的每个值；
- all：比较子查询返回的全部值；
- any 和 all 还要和单行子查询的比较符配合使用。

##### （1）any 运算符

ANY 运算符比较一个值与一个子查询返回的每一个值。
- < ANY 意思是小于最大值；
- > ANY 意思是大于最小值。即存在即可。

```sql
SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE salary < ANY(SELECT salary
                   FROM employees
                   WHERE job_id = 'IT_PROG')    -- 结果是 (9000, 6000, 4300)，这里是小于 9000，不一定小于 4300，但是存在即可
  AND job_id <> 'IT_PROG';
```

##### （2）all 运算符

ALL 运算符比较一个值与子查询返回的每个值。
- < ALL 意思是小于最小值；
- > ALL 意思是大于最大值。即包括所有。

```sql
SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE salary < ALL(SELECT salary
                   FROM employees
                   WHERE job_id = 'IT_PROG')    -- 结果是 (9000, 6000, 4300)，这里是小于 4300，小于 4300 就小于全部
  AND job_id <> 'IT_PROG';
```

##### （3）in 与子查询中的空值

NOT 运算符可以与 IN 运算符一起使用。但要注意空值问题：

```sql
SELECT emp.last_name
FROM employees emp
WHERE emp.employee_id NOT IN (SELECT manager_id FROM employees);
```

这条没有返回，查询不到数据。原因：内查询返回的值含有空值（MANAGER_ID 里面有空值），并因此整个查询无返回行，原因是用大于、小于或不等于比较 NULL 值，都返回 null。

所以，只要空值可能是子查询结果集的一部分，就不能用 NOT IN 运算符。IN 运算符是可以的：

```sql
SELECT emp.last_name
FROM employees emp
WHERE emp.employee_id IN (SELECT manager_id FROM employees);
```

NOT IN 运算符相当于 <> ALL，IN 相当于 = ANY。

#### 4.子查询练习

**1.写一个查询显示与 Zlotkey 在同一部门的雇员的 last name 和 hire date，结果中不包括 Zlotkey**

```sql
SELECT last_name, hire_date
FROM employees
WHERE department_id = (SELECT department_id
                       FROM employees
                       WHERE last_name = 'Zlotkey')
  AND last_name <> 'Zlotkey'
ORDER BY last_name;
```

**2.创建一个查询显示所有其薪水高于平均薪水的雇员的雇员号和名字，按薪水的升序排序**

```sql
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees)
ORDER BY salary;
```

**3.写一个查询显示所有工作在「有任一雇员的名字中包含一个 u 的部门」的雇员的雇员号和名字**

```sql
SELECT employee_id, last_name
FROM employees
WHERE department_id IN (SELECT department_id
                        FROM employees
                        WHERE last_name LIKE '%u%');
```

**4.显示所有部门地点号（department location ID）是 1700 的雇员的 last name、department_id 和 job_id**

```sql
SELECT last_name, department_id, job_id
FROM employees
WHERE department_id IN (SELECT department_id     -- 子查询还能连接不同表
                        FROM departments
                        WHERE location_id = 1700);
```

先用多表查询的等值连接也能写出来：

```sql
SELECT emp.last_name, emp.department_id, emp.job_id, dep.location_id
FROM departments AS dep, employees AS emp
WHERE dep.department_id = emp.department_id
  AND location_id = 1700;
```

**5.显示每个向 King（经理）报告的雇员的名字和薪水**

```sql
SELECT last_name, salary
FROM employees
WHERE manager_id IN (SELECT employee_id
                     FROM employees
                     WHERE last_name = 'King');
```

**6.显示在 Executive 部门的每个雇员的 department_id、last name 和 job ID**

```sql
SELECT department_id, last_name, job_id
FROM employees
WHERE department_id = (SELECT department_id
                       FROM departments
                       WHERE department_name = 'Executive');
```

同样应该能用等值连接：

```sql
SELECT dep.department_id, last_name, job_id
FROM departments AS dep, employees AS emp
WHERE dep.department_id = emp.department_id
  AND dep.department_name = 'Executive';
```

### 六.分页查询（limit）

适用于数据量特别大的时候查询。比如 1000 条数据，我们选择 20 条数据作为一页，好看，每次就看 20 条。

MySQL 分页查询原则：
- 在 MYSQL 数据库中使用 LIMIT 子句进行分页查询；
- MySQL 分页中开始位置为 0；
- 分页子句在查询语句的最后侧。

**LIMIT 子句语法**

```sql
SELECT 投影列
FROM 表名
WHERE 条件
ORDER BY ...
LIMIT 开始位置, 查询数量;      -- 从头开始，开始位置就是 0
```

示例：查询雇员表中所有数据按 id 排序，实现分页查询，每次返回两条结果。

```sql
SELECT *
FROM employees
ORDER BY EMPLOYEE_ID ASC
LIMIT 0, 2;
```

**LIMIT ... OFFSET 子句语法**

其实和 limit 是一样的，只是参数顺序不一样。

```sql
SELECT 投影列
FROM 表名
WHERE 条件
ORDER BY ...
LIMIT 查询数量
OFFSET 开始位置;
```

```sql
SELECT *
FROM employees
ORDER BY EMPLOYEE_ID ASC
LIMIT 2 OFFSET 0;
```

# 索引

### 一.索引介绍

索引是什么：新华字典的 ABCDEFG 开头查汉字就是索引，像目录一样，方便快速找到数据。

索引是对数据库表中的一列或多列值进行排序的一种结构。更通俗的说，数据库索引好比是一本书前面的目录，能加快数据库的查询速度。索引是一种特殊的文件，它们包含着对数据表里所有记录的位置信息。MySQL 索引的建立对于 MySQL 的高效运行是很重要的，索引可以大大提高 MySQL 的检索速度。

**索引的作用**：索引相当于图书上的目录，可以根据目录上的页码快速找到所需的内容，提高性能（查询速度）。

**什么时候需要创建索引**
- 频繁作为查询条件的字段应该创建索引；
- 查询中排序的字段创建索引将大大提高排序的速度（索引就是排序加快速查找）；
- 查询中统计或者分组的字段。

**MySQL 中的索引类型**
- 普通索引：最基本的索引，它没有任何限制。
- 唯一索引：索引列的值必须唯一，但允许有空值，如果是组合索引，则列值的组合必须唯一。
- 主键索引：特殊的索引，唯一的标识一条记录，不能为空，一般用 primary key 来约束。
- 联合索引（组合索引）：在多个字段上建立索引，能够加速查询的速度。

### 二.普通索引

普通索引（index）是最基本的索引，它没有任何限制。在创建索引时，可以指定索引长度。length 为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度，如果是 BLOB 和 TEXT 类型必须指定 length。

创建索引时需要注意：如果指定单列索引长度，length 必须小于这个字段所允许的最大字符个数；也可以不指定长度。

**查询索引**

```sql
SHOW INDEX FROM employees;
```

**直接创建索引（DDL 语句）**

```sql
CREATE INDEX 索引名 ON 表名(列名(length));
```

示例：为 emp3 表中的 name 创建一个索引，索引名为 emp3_name_index（表在 test1 里面，这里只能做个代码示例）。

```sql
CREATE INDEX emp3_name_index ON emp3(name);
```

索引命名一般是 表名_列名_index。

注：原笔记写作 `create index emp3_name_index emp3(name);`，漏了 ON 关键字。

**修改表添加索引**

```sql
ALTER TABLE 表名 ADD INDEX 索引名 (列名(length));
```

示例：修改 emp3 表，为 address 列添加索引，索引名为 emp3_address_index。

```sql
ALTER TABLE emp3 ADD INDEX emp3_address_index (address);
```

**创建表时指定索引列**

```sql
CREATE TABLE 表名(
    列名 类型 PRIMARY KEY,          -- 设置键
    列名 类型,
    列名 类型,
    INDEX 索引名 (列名(length))      -- 这里的列名一定要是上面创建的
);
```

示例：创建 emp4 表，包含 emp_id、name、address 列，同时为 name 列创建索引，索引名为 emp4_name_index。

```sql
CREATE TABLE emp4 (
    emp_id INT(30),
    name VARCHAR(30),
    address VARCHAR(30),
    INDEX emp4_name_index (name)
);
```

有主键的话会自动存在主键索引。

**删除索引**

```sql
DROP INDEX 索引名 ON 表名;
```

```sql
DROP INDEX emp3_name_index ON emp3;
```

注：原笔记写作 `drop emp3_name_index from emp3;`，语法不对。

### 三.唯一索引

唯一索引（UNIQUE）与普通索引类似，不同的就是：索引列的值必须唯一，但允许有空值。

这讲得像唯一性约束：当你为表中某一列分配唯一性约束时，该列自带唯一索引了。

**创建唯一索引（给不具备唯一性约束的列）**

```sql
CREATE UNIQUE INDEX 索引名 ON 表名(列名(length));
```

示例：为 emp 表中的 name 创建一个唯一索引，索引名为 emp_name_index。

```sql
CREATE UNIQUE INDEX emp_name_index ON employee(name);    -- 报错可能是因为这列有重复数据
```

**修改表添加唯一索引（以修改的方式添加索引）**

```sql
ALTER TABLE 表名 ADD UNIQUE 索引名 (列名(length));
```

示例：修改 emp 表，为 salary 列添加唯一索引，索引名为 emp_salary_index。

```sql
ALTER TABLE employee ADD UNIQUE emp_salary_index (salary);
```

**创建表时指定唯一索引**

```sql
CREATE TABLE 表名(
    列名 类型 PRIMARY KEY,
    列名 类型,
    UNIQUE 索引名 (列名(length))
);
```

示例：创建 emp5 表，包含 emp_id、name、address 列，同时为 name 列创建唯一索引，索引名为 emp5_name_index。

```sql
CREATE TABLE emp5 (
    emp_id INT PRIMARY KEY auto_increment,
    name VARCHAR(30),
    address VARCHAR(30),
    UNIQUE emp5_name_index (name)
);
```

```sql
SHOW INDEX FROM emp5;      -- 一个主键索引，一个唯一索引
```

### 四.主键索引

主键索引是一种特殊的唯一索引，一个表只能有一个主键，不允许有空值。一般是在建表的时候同时创建主键索引。

- 一个表只能有一个主键索引；
- 不能空，不能重复。

**修改表添加主键索引（就是给表中添加主键约束）**

```sql
ALTER TABLE 表名 ADD PRIMARY KEY (列名);
```

示例：修改 emp 表，为 employee_id 添加主键索引。

```sql
ALTER TABLE emp ADD PRIMARY KEY (employee_id);
```

**创建表时指定主键索引**

```sql
CREATE TABLE 表名(
    列名 类型 PRIMARY KEY
);
```

示例：创建 emp6 表，包含 emp_id、name、address 列，同时为 emp_id 列创建主键索引。

```sql
CREATE TABLE emp6 (
    emp_id INT PRIMARY KEY,
    address VARCHAR(30),
    name VARCHAR(30)
);

SHOW INDEX FROM emp6;
```

### 五.组合索引

组合索引是指使用多个字段创建的索引，只有在查询条件中使用了创建索引时的第一个字段，索引才会被使用（最左前缀原则）。

**最左前缀原则**

就是最左优先。例如我们使用表中的 name、address、salary 创建组合索引，那么想要组合索引生效，我们只能使用如下组合：

- name / address / salary
- name / address
- name

如果使用 address / salary 或者是 salary，则索引不会生效。最左边的一定要当前缀。

**添加组合索引（实际上是修改表）**

```sql
ALTER TABLE 表名 ADD INDEX 索引名 (列名(length), 列名(length) ...);    -- 要组合的列
```

示例：修改 emp6 表，为 name、address 列创建组合索引。

```sql
ALTER TABLE emp6 ADD INDEX groub_index (name, address);

SHOW INDEX FROM emp6;
```

**创建表时创建组合索引**

```sql
CREATE TABLE 表名(
    列名 类型,
    列名 类型,
    列名 类型,
    INDEX 索引名 (列名(length), 列名(length))
);
```

示例：创建 emp7 表，包含 emp_id、name、address 列，同时为 name、address 列创建组合索引。

```sql
CREATE TABLE emp7 (
    emp_id INT,
    name VARCHAR(30),
    address INT,
    INDEX group_index (name, address)
);

SHOW INDEX FROM emp7;
```

# 事务控制语言  TCL

### 一.事务简介

所有关系型数据库都支持事务。

事务是指作为单个逻辑工作单元执行的一系列操作，要么完全地执行，要么完全地不执行，像是一整个操作流程。

**事务定义（Transaction）**

1. 事务是一个最小的不可再分的工作单元；通常一个事务对应一个完整的业务。例如银行账户转账业务，该业务就是一个最小的工作单元：实际上转账是 A 账户减 100、B 账户加 100 两条 update 语句，这两条语句要么都成功，要么都失败，这就是事务的意思。
2. 一个完整的业务需要批量的 DML（insert、update、delete）语句共同联合完成。
3. 事务只和 DML 语句有关，或者说 DML 语句才有事务。这个和业务逻辑有关，业务逻辑不同，DML 语句的个数不同。

### 二.事务四大特征 ACID（面试可能问）

- **原子性（ATOMICITY）**：事务中的操作要么都不做，要么就全做。
- **一致性（CONSISTENCY）**：一个事务应该保护所有定义在数据上的不变的属性（例如完整性约束）。在完成了一个成功的事务时，数据应处于一致的状态。
- **隔离性（ISOLATION）**：一个事务的执行不能被其他事务干扰。
- **持久性（DURABILITY）**：一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。

### 三.事务类型

- **显式事务**：需要我们手动的提交或回滚（回滚：进行到一半取消）。DML 语言中的所有操作都是显式事务操作，需要人为操作。
- **隐式事务**：数据库自动提交，不需要我们做任何处理，同时也不具备回滚性。DDL、DCL 语言都是隐式事务操作，我们无法控制，数据库自己处理。

### 四.使用事务（TCL 语句：事务控制语句）

- `start transaction`：事务开启
- `commit`：事务提交
- `rollback`：事务回滚（撤回）

**示例：银行转账事务**

步骤一：创建银行 account 账户表，包含 id、卡号、用户名、余额。

```sql
CREATE TABLE account (
    people_id INT PRIMARY KEY auto_increment,
    card_id INT NOT NULL,
    username VARCHAR(30) NOT NULL UNIQUE,
    balance DOUBLE(10, 2)
);
```

步骤二：向 account 表中插入两条数据。

```sql
INSERT INTO account VALUES (001, 1001, 'jake', 10003.34);
INSERT INTO account VALUES (002, 1002, 'baby', 102.45);
```

这里的 insert-DML 语句都是自动提交、直接执行。如果想人为控制是否提交，要关闭 MySQL 对 DML 语句的自动提交，即开启事务，把 DML 语句放进事务当中。

开启事务：

```sql
START TRANSACTION;      -- 使用这句即开启一个事务

INSERT INTO account VALUES (003, 1003, 'xiuyi', 4303.34);
INSERT INTO account VALUES (040, 1004, 'homeleader', 187.40);

SELECT * FROM account;
```

这里能查到，其实是假象：数据库实际并没有真的插入其中，要提交才能插入。

步骤三：在一个事务中完成转账业务。

```sql
START TRANSACTION;

UPDATE account SET balance = balance - 200 WHERE people_id = 1;
UPDATE account SET balance = balance + 200 WHERE people_id = 2;

SELECT * FROM account;

COMMIT;      -- 提交事务，这时候数据表真的变化了
```

### 五.事务回滚

事务的回滚让数据库恢复到了执行事务操作前的状态。

需要注意的是事务的回滚必须在事务提交之前，因为事务一旦 commit 就不能再进行回滚操作，只能对未 commit 的操作进行 rollback 处理。

```sql
ROLLBACK;
```

### 六.事务并发问题

不仅出现在 MySQL 数据库中，所有关系型数据库都会出现。例如两个事务同时操作。

**问题一：脏读（读取未提交数据）**

指一个事务读取了另外一个事务未提交的数据。

A 事务读取 B 事务尚未提交的数据，此时如果 B 事务发生错误并执行回滚操作，那么 A 事务读取到的数据就是脏数据。（看图）

**问题二：不可重复读（前后多次读取，数据内容不一致）**

在一个事务内读取表中的某一行数据，多次读取结果不同。

事务 A 在执行读取操作，由于整个事务 A 比较大，前后读取同一条数据需要经历很长的时间。而事务 A 第一次读取数据，比如此时读取了小明的年龄为 20 岁，事务 B 执行更改操作，将小明的年龄更改为 30 岁；此时事务 A 第二次读取到小明的年龄时，发现其年龄是 30 岁，和之前的数据不一样了，也就是数据不重复了，系统不可以读取到重复的数据，称为不可重复读。

看图，因为事务未提交的话数据是不变的，但是两次查到的数据不一样。

**问题三：幻读（前后多次读取，数据总量不一致）**

是指在一个事务内读取到了别的事务插入的数据，导致前后读取数量总量不一致。事务 A 在执行读取操作，需要两次统计数据的总量，前一次查询数据总量后，此时事务 B 执行了新增数据的操作并提交后，这个时候事务 A 读取的数据总量和之前统计的不一样，就像产生了幻觉一样，平白无故的多了几条数据，成为幻读。

有点像不可重复读：不可重复读是数据的内容不一样，幻读是数据的数量不一样。

### 七.事务的隔离级别

怎么解决事务的并发问题：事务的隔离级别。

事务的隔离级别用于决定如何控制并发用户读写数据的操作。数据库是允许多用户并发访问的，如果多个用户同时开启事务并对同一数据进行读写操作的话，有可能会出现脏读、不可重复读和幻读问题，所以 MySQL 中提供了四种隔离级别来解决上述问题。

事务的隔离级别从低到高依次为：

| 隔离级别 | 说明 |
| --- | --- |
| READ UNCOMMITTED | 读未提交 |
| READ COMMITTED | 读已提交 |
| REPEATABLE READ | 重复读 |
| SERIALIZABLE | 串行化，一个一个执行，安全性最高，但是效率最低 |

隔离级别越低，越能支持高并发的数据库操作。

| 隔离级别 | 脏读 | 不可重复读 | 幻读 |
| --- | --- | --- | --- |
| READ UNCOMMITTED | 会出现 | 会出现 | 会出现 |
| READ COMMITTED | 解决 | 会出现 | 会出现 |
| REPEATABLE READ | 解决 | 解决 | 会出现 |
| SERIALIZABLE | 解决 | 解决 | 解决 |

提示：解决表示不会出现，会出现表示未解决。

MySQL 中默认的隔离级别是 repeatable read。

注：原笔记这张表的值记反了（写成 SERIALIZABLE 行 Y N N、READ UNCOMMITTED 行 N N N），这里按标准结论整理。

**查看 MySQL 事务隔离级别**

```sql
SELECT @@transaction_isolation;
```

**设置事务隔离级别**

只对当前 session（客户端）有效。

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

# 数据控制语言  DCL

### 一.MySQL 的用户管理

MySQL 是一个多用户的数据库系统。按权限，用户可以分为两种：root 用户（超级管理员）和由 root 用户创建的普通用户。

**创建用户**

```sql
CREATE USER username IDENTIFIED BY 'password';
```

**查看用户**

```sql
SELECT USER, HOST FROM mysql.user;
```

示例：创建一个 u_sxt 的用户，并查看创建是否成功。

```sql
CREATE USER u_sxt IDENTIFIED BY '123456';

SELECT USER, HOST FROM mysql.user;
```

创建命令在哪都可以用。

### 二.权限管理

新用户创建完后是无法登录的，需要分配权限。

```sql
GRANT 权限, 权限... ON 数据库.表 TO 用户名@登录主机 IDENTIFIED BY '密码';
```

这是 DCL（数据库控制语言）。

**用户名@登录主机**

| 字段（@后面加的） | 含义 |
| --- | --- |
| % | 匹配所有主机 |
| localhost | 本机登录，localhost 不会被解析成 IP 地址，直接通过 UNIX socket 连接，只能本机连接 |
| 127.0.0.1 | 会通过 TCP/IP 协议连接，并且只能在本机访问 |
| ::1 | 就是兼容支持 ipv6 的，表示同 ipv4 的 127.0.0.1 |

**权限 ON 数据库.表**

表示你这个用户可以对数据库进行什么操作（所有库的所有表是 `*.*`，哪边是所有，那边就是 `*`）。

权限列表看图片，太多了。

```sql
GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password';
```

含义：允许该用户对所有库的所有表有所有权限。

示例：为 u_sxt 用户分配只能查询 test1 库中的 employee 表，并且只能在本机登陆的权限。

```sql
GRANT SELECT ON test1.employee TO 'u_sxt'@localhost IDENTIFIED BY '123456';
```

允许在授权时创建用户，但 MySQL 8.0 以后版本不能用这个，要分开运行：先创建用户，再分配权限。下面这个 sxt 用户本来没有，我们在授权的同时创建用户：

```sql
GRANT ALL PRIVILEGES ON *.* TO 'sxt'@'%' IDENTIFIED BY '123456';

SELECT USER, HOST FROM mysql.user;
```

创建成功，并且拥有全部权限，而且可以全程连接（用了 %）。

**刷新权限**

```sql
FLUSH PRIVILEGES;
```

### 三.删除用户

```sql
DROP USER '用户名'@'连接权限';
```

示例：删除 sxt 用户。

```sql
DROP USER 'sxt'@'%';
```

### 四.Navicat 管理用户

- 新建用户：用户 → 新建用户 → 用户名（自己设）、主机（% 任何连，localhost/IP 本机连）、密码 → 保存，就创建好了，但是没权限。
- 分配权限：权限列 → 添加权限 → 可以选择某个库某个表或者某个库给某些权限，只需要点勾，然后就会出现这个权限列，还能继续设置。
- 使用别的用户连接：连接 → MySQL → 用户名 + 密码（连接名随便起，用户名是我们创建的那个名字）。
- 删除用户：用户 → 右键删除就行。

# Navicat 导入导出数据

**导出数据**

选中表 → 右键 → 导出向导 → 一般用 sql 脚本文件导出。

右键 → 转储 sql 文件也可以。

**导入数据**

- 方法 1：右键某个库 → 运行 sql 文件 → 选择位置找到 SQL 文件导入。
- 方法 2：找到 SQL 文件代码复制，在库中查询文件粘贴。

两种方法都可以，方法二在乱码时可以使用。
