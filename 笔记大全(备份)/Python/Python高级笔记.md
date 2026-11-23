# Python 高级笔记

> 来源:有道云笔记分享《02.Python高级》(共 5 章)

## 目录

- 第1章 面向对象
- 第2章 错误和异常
- 第3章 文件操作
- 第4章 Python高级进阶
- 第5章 Python操作MySQL数据库

---

## 第1章 面向对象

问题：如何在Python中描述一个学生的信息，学生的信息包含以下几个内容：

1. 学号
1. 姓名
1. 年龄
1. 分数

**方案1：**

```python
student_no = '20260819'
student_name = '张三'
student_age = 18
student_score = 80
```

面向过程,一步一步来

不足：使用变量存储学生的多条信息时，需要频繁的开辟空间，非常消耗计算机性能，如果有多个学生的信息需要存储时，也不便于数据管理

**方案2：**

```python
# 使用字典
student_dict = {'student_no':'20250117001','student_name':'张三','student_age':18,'student_score':20}
print(f'学生的信息是:{student_dict}')
```

通过使用字典存储学生信息，解决了学生信息存储频繁开辟内存空间的问题，但描述一个学生信息时，除了一些静态特征信息外，可能还需要描述学生能执行的一些操作，比如：吃饭，学习，睡觉等，字典则无法完成。因此，可以通过引入类来解决此问题

### 面向对象

面向过程是编年体;

面向对象是纪传体。

![图片](Python高级笔记_assets/ch1_img01.png)

对象：一个拥有『属性』和『行为』的个体，是构成现实世界和程序世界的基本单位。万物皆对象！任何一个具体存在的人或物，都可以看成一个对象。

一个人

一辆车

一部手机

面向对象：一种以对象为中心去思考和组织代码的方式，它更关注：“谁来做这件事”

**面向对象技术简介**

1. 类(class):** **用来描述具有相同的属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。
1. 类名建议用驼峰式
1. **对象是类的实例。---创建一个对象就是实例化的过程,实例你知道吧,**实例:类具体的一个东西,比如我家的泰迪,就是狗类的实例
1. 方法：类中定义的函数。
1. 方法是放在类里面的函数
1. 属性是放在类里面的变量

![图片](Python高级笔记_assets/ch1_img02.png)

和其它编程语言相比，Python 在尽可能不增加新的语法和语义的情况下加入了类机制。

#### 类定义

语法格式如下：

```python
class ClassName:   #如果要继承父类的话,括号加父类名
    <statement-1>
    .
    .
    .
    <statement-N>
```

类实例化后，可以使用其属性

bob = ClassName()   #实例化一个bob的对象

#### 对象创建

对象支持两种操作：属性引用和方法调用。

```python
class Student:
    student_no = '20250117001',   #这些直接写的是类属性(也可以理解成全局属性)
    student_name = 'tom',
    student_age = 18,
    student_score = 20
 
 bob = student()
# 类名.属性名   
Student.student_no = '123234' #类属性在外面可以分别被类名调用和实例调用修改
bob.student_no = '123234345'  #结果你就自己想吧,很简单
print(Student.student_no)   #类名调用属性
```

##### 构造方法

类有一个名为 \_\_init\_\_() 的特殊方法（**构造方法**），该方法在类实例化时会自动调用，像下面这样：

```python
class Student:
    student_no = '20250117001',
    student_name = 'tom',
    student_age = 18,
    student_score = 20

    def __init__(self):
        print("一个student实例被创建")
```

类定义了 \_\_init\_\_() 方法，**类的实例化操作后会自动调用 \_\_init\_\_() 方法**。如下实例化类 Student，对应的 \_\_init\_\_() 方法就会被调用:

```
# 创建实例对象
student = Student()
```

**结果输出**:一个student实例被创建

当然， \_\_init\_\_() 方法可以有参数，参数通过 \_\_init\_\_() 传递到类的实例化操作上

卧槽好像很好理解了因,因为实例的时候就会调用\_\_init\_\_方法,然后我们在\_\_init\_\_里面就让self.参数=括号的传参

是不是相当于也是个全局变量呢

**传入的参数开局就被\_\_init\_\_方法调用了,然后赋值给全局变量self.变量名  (因为\_init\_里面写了)**

那也好理解了,普通方法也可以写这种self.变量,调用的时候传参赋值,也就有了一个全局变量

**看懂这些叽里咕噜说的和下面的基础代码**

```python
class Student:

    # Python支持动态属性   提升了代码的简洁性
    def __init__(self, student_no, student_name, student_age, student_score):
        self.student_no = student_no
        self.student_name = student_name
        self.student_age = student_age   #后面在类里面用这种属性也是self.student_age
        self.student_score = student_score 


# 创建实例对象
student = Student('20250117001','张三','18','20')   #传入的参数开局就被__init__方法调用了,然后赋值给全局变量self.变量名  (因为_init_里面写了)
#  访问对象中的属性                             #那也好理解了,普通方法也可以写这种全局变量,调用的时候传参赋值,也就有了一个全局变量
print(student.student_name)
print(student.student_no)
print(student.student_score)
print(student.student_age)
```

属性进入构造方法后变成了**实例属性**,再用类直接调用name属性就报错了

![图片](Python高级笔记_assets/ch1_img03.png)

但是实例属性进构造方法之前还定义了类属性,所以这次直接调用name属性还是正常的

![图片](Python高级笔记_assets/ch1_img04.png)

也可以这样写不传参,直接定义值给实例属性,我感觉用来定义数据容器用的比较多,比如空字典空列表啥的

```python
def __init__(self):
    self.info = 'this is a instance info'
    self.items = {}
```

**self 代表类的实例，而非类(别管)**

类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的  **第一个参数名称**,

```python
class Test:
    def prt(self):
        print(self)
        print(self.__class__)
 
t = Test()
t.prt()
```

按照惯例它的名称是 self,在 Python中，self 是一个惯用的名称，用于表示类的实例（对象）自身。它是一个指向实例的引用，使得类的方法能够访问和操作实例的属性。

当你定义一个类，并在类中定义方法时，第一个参数通常被命名为 self，尽管你可以使用其他名称，但强烈建议使用 self，以保持代码的一致性和可读性

```python
class Test:
    def prt(runoob):
        print(runoob)
        print(runoob.__class__)
```

```python
t = Test()
t.prt()
```

##### 类的方法

在类的内部，使用 **def** 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 self, 且为第一个参数，self 代表的是类的实例。

```python
class people:
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print(f"{name} 说: 我 {age} 岁。体重是:{self.__weight}" )
 
# 实例化类
p = people('runoob',10,30)
p.speak()
```

**方法分类**

- **构造方法(self): \_\_init\_\_**
- **普通方法/实例方法****(self)**:  最常用

它最少也要包含一个 self 参数，用于绑定调用此方法的实例对象（Python 会自动完成绑定）。

实例方法通常会用类对象进行调用,最常写的

- **类方法(cls)**:   修改类属性

我们在调用类方法时，无需显式为 cls 参数传参。

**  类方法需要使用**`＠classmethod`**修饰符进行修饰 **

一般用来给全部的实例 **一个共同类属性或者修改类属性,也可以返回类属性**,起码我写第一题是这么想的

**调用时要用类名.方法名调用,不要用对象名**

**   ** 但是其实实例也能调用,实例可以调用类方法和实例方法,类只能调用类方法

不管是类调用还是实例调用,只要调用了就真实修改永久一次类属性

用实例调用普通方法修改类属性仅在当前实例生效,所以用类方法修改才能全局修改

- **静态方法(无)**:  #蒙多想去哪就去哪,类里面的普通函数

静态方法，其实就是我们学过的函数,就当作一个普通函数写,里面不能写类属性和类方法

和函数唯一的区别是，静态方法定义在类这个空间（类命名空间）中

而函数则定义在程序所在的空间（全局命名空间）中。

**要用**** @staticmethod修饰**

想在哪写就在哪写,想在哪用就在哪用

写了几个题目感觉可以用来返回bool值来用来判断

静态方法没有类似 self、cls 这样的特殊参数，因此 Python 解释器不会对它包含的参数做任何类或对象的绑定。也正因为如此，**类的静态方法中无法调用任何类属性和类方法。**

****

****

<u>***在实际编程中，几乎不会用到类方法和静态方法，因为我们完全可以使用函数代替它们实现想要的功能，但在一些特殊的场景中（例如工厂模式中），使用类方法和静态方法也是很不错的选择。***</u>

```python
class Aoo:
    class_attribute = 'this is a class attribute'

    def __init__(self):
        self.info = 'this is a instance info'

    #  修改实例属性 info
    def update_info(self, info):
        self.info = info

    def get_info(self):
        return self.info

    #  类函数 修改类属性
    @classmethod
    def update_class_attribute(cls, info):
        cls.class_attribute = info

    @classmethod
    def get_class_attribute(cls):
        return cls.class_attribute  #前面要加cls

    #  静态函数
    @staticmethod
    def sum_static_method(num1: int, num2: int):
        return num1 + num2


aoo = Aoo()
print(aoo.get_info())
aoo.update_info('this is a new instance info')
print(aoo.get_info())

Aoo.update_class_attribute('this is a new class attribute')
print(Aoo.get_class_attribute())


result = Aoo.sum_static_method(1,2)
print(result)
```

**作用域(神了又来)**

1. 成员变量：定义在类中函数体外的变量，也称之为全局变量，在整个类中均可以访问,比如类属性和实例属性
1. 局部变量：定义在函数中的变量，访问范围仅限在当前函数, 方法里面写死的???

```python

class Person:
    # 成员变量
    name = None
    age = None
    
    def __init__(self,name,age):
        self.name = name
        self.age = age
    def printInfo(self):
        # 局部变量  
        name = "lisi"
        age = 18
        # 访问方法中的变量 
        print(f'函数中打印name = {name}, age = {age}')
        # 函数中访问成员变量  
        print(f'实例对象中的name = {self.name},age = {self.age}')

person = Person('张三',20)
print(person.name)
print(person.age)
person.printInfo()
```

#### 类属性&实例属性

##### 1. 定义位置和作用域

- ** 直接添加的变量(****类属性):**
  - 定义在类的定义体中，但不在任何方法内。
  - 它属于类本身，而不属于某个具体的实例。
  - 类成员变量可以通过类名访问，也可以通过实例访问
  - 一般用类方法修改
  - 对象的公有值可以用类属性

```python
class MyClass:
    class_var = 10  # 类成员变量/类属性
```

- **self 动态添加的变量(实例属性)**：
  - 定义在类的方法中，通常通过self 关键字赋值。
  - 它属于类的某个具体实例，不同实例可以有不同的值。
  - 类里面都可以调用

```python
class MyClass:
    def __init__(self, value):
        self.instance_var = value  # 实例属性
```

![图片](Python高级笔记_assets/ch1_img05.png)

##### 2. 访问方式

- 类成员变量(类属性):
  - 通过类名访问：MyClass.class\_var。
  - 也可以通过实例访问：obj.class\_var，但这种方式实际上会先查找实例属性，如果没有找到，才会查找类属性。
  - 如果通过实例修改类属性，实际上会创建一个同名的实例属性，而不是修改类属性。
- 通过 self 动态添加的变量(实例属性)：
  - 只能通过实例访问：obj.instance\_var。
  - 每个实例的值独立，互不影响。

##### 3. 存储和内存

- 类成员变量：
  - 存储在类的命名空间中，所有实例共享。
  - 修改类属性会影响所有实例（除非实例中覆盖了同名属性）。
- 通过 self 动态添加的变量：
  - 存储在实例的命名空间中，每个实例独立。
  - 修改一个实例的实例属性不会影响其他实例。

**练习：**

**1.定义一个类SearchMaxNumber，定义函数max\_number,实现查找一个列表数据最大值  (不许用max)**

**2.定义一个Car类，定义一个函数update\_info,可以更新汽车的颜色，价格，型号，品牌信息**

**3.定义一个圆类，定义半径属性，定义函数实现周长和面积的计算并返回**

#### 析构函数(细狗函数)        __del__

析构方法\_\_del\_\_:是对象在被垃圾回收的时候起作用的一个方法

它的执行一般也就意味着对象不能够继续引用，**用来回收内存,结束该类实例的生命周期。**

def \_\_del\_\_(self):

print("我被销毁了")

person = Person()

del person  #手动调用析构函数提前回收内存

```python
#案例一:自动销毁
class Person:
    def __init__(self):
        print("我是构造方法")
    def __del__(self):   #当我们的实例在使用完之后会自动调用__del__
        print("我被销毁了")

person = Person()
print("我是第一行代码")

#观察打印的顺序

#手动销毁
class Person:
    def __init__(self):
        print("我是构造方法")
    def __del__(self):
        print("我被销毁了")

person = Person()
del person   #手动调用提前销毁
print("我是第一行代码")
print("我是第二行代码")

#观察打印的顺序
```

### 面向对象三大特性(面试高频)

#### 封装(私有化)

**为什么使用封装**

1.隐藏实现细节

2.实现数据验证

3.**保护数据隐私**（比如薪资），授权后的数据才能访问

不想让别人知道的属性和方法就封装起来

**如何实现类中的属性/方法的私有化(如何实现封装)**

1.使用**双下划线开头**命名的**属性或方法**是私有的

2.私有的属性或方法只能在该类中进行访问，类的外部无法使用

3.按照封装的思想，**一般将属性私有化，方法公开化**

4只需要前面两个下划线,后面不需要.,不然就变成魔术方法了

![图片](Python高级笔记_assets/ch1_img06.png)

强行访问(但不推荐使用)

![图片](Python高级笔记_assets/ch1_img07.png)

**如何访问私有属性或方法**

<u>想要访问私有属性必须写一个调用私有属性的方法</u>

提供公共的方法实现对私有成员的操作,声明属性的时候前面加上\_\_

案例：(看懂这个代码案例)

定义一个员工类，包含name，job，salary属性

1.不能随便查看员工的薪资.其实是害怕修改工资

2.提供公共的操作方法实现对职位和薪资的操作

**封装属性:属性前面加两下划线**

```python
class Employ(object):

    def __init__(self, name, job, salary):
        self.name = name
        self.job = job
        # 定义私有属性
        self.__salary = salary

    def show_info(self):
            # 类中访问私有属性,想要看信息只能调用这个方法,但是想直接打印私有属性还是会报错
            print(f"{self.job}{self.name}的薪资是{self.__salary}")
    
    def update_info(self,salary,job):  #更新工资,但实际场景中,这个方法需要权限校验才能调用
        self.__salary = salary
        self.job = job

emp = Employ('刘德华',salary='10000',job='歌手')  #实例化对象
print(emp.name)  #可以输出
print(emp.job)   #可以输出
print(emp.salary)  #salary已经被封装,直接打印会报错
emp.show_info()  #调用方法,才可以输出salary

#  更新数据
emp.update_info(salary=20000,job='演员')  #假设这里已经进行了权限校验,调用更新方法修改工资
emp.show_info()  #调用方法,输出salary

#  类外无法访问私有属性
print(emp.__salary)  #加下划线也不行,报错
```

**封装方法:方法前面加两下划线**

只能用公有的方法里面写私有方法才能调用私有的方法

**一般用的少****,方法写出来不就是让人用的吗,常常和权限有关**

```python
class MyClass:
    def __init__(self):
        self.__private_var = 42

    def __private_method(self):
        print("This is a private method.")
 
    def public_method(self):  #提供公有的方法才能调用私有的方法
        print("This is a public method.")
        self.__private_method()
```

在这个例子中，\_\_private\_var 和 \_\_private\_method 是私有变量和私有方法。它们不能直接从类的外部访问，但可以通过一些方式绕过这种限制:\_类名\_\_属性名/方法名

```python
obj = MyClass()
obj.public_method()   #正确调用
print(obj._MyClass__private_var)  # 可以直接访问私有属性,邪修,不要轻易使用
obj._MyClass__private_method()    # 可以直接调用私有方法,邪修,不要轻易使用
obj.private_method()  #直接调用私有方法,结果报错
```

结果:

This is a public method.

This is a private method.

42

This is a private method.

啦啦啦啦啦啦啦啦,报错啦

在 Python 中，私有化主要是通过命名约定来实现的，而不是通过严格的访问控制机制，当你定义一个以双下划线开头的变量或方法时，Python 会自动将其名称改写为 \_类名\_\_变量名 或 \_类名\_\_方法，Python 的设计理念是“程序员都是成年人”，即它不会强制限制某些操作，而是通过约定和文档来指导程序员正确使用。私有化更多是一种约定，提醒程序员不要直接访问这些属性或方法，而不是绝对禁止

案例：

定义Account类，

1.要求具有属性： 姓名（长度2-4位），余额（必须大于10），密码（长度6），如果不满足则给出提示信息，使用默认值

2.定义set\_xxx方法给属性赋值

3.编写show\_info方法，接收用户名和密码，正确则展示用户的余额信息

```python
class Account:
    
    __name = None,
    __money = None,
    __password = None

    def set_name(self, name):

        if len(name) < 2 or len(name) > 5:
            print("请输入2-5位长度的用户名")
        else:
            self.__name = name

    def set_money(self, money):
        if money < 10:
            print("请输入不小于10的账户余额")
        else:
            self.__money = money

    def set_password(self, password):
        if len(password) == 6:
            self.__password = password
        else:
            print("密码长度为6位")

    def getMoney(self, name, password):
        if password == self.__password and name == self.__name:
            return self.__money
        else:
            print('用户名密码不正确')


account = Account()
account.set_name("admin")
account.set_money(100)
account.set_password("123456")

money = account.getMoney('admin', '123456')
print(money)
```

自己写的方法,喵的题干也不讲清楚

```python
class Account(object):
    def __init__(self, name, balance, password):
        if len(name)>=2 and len(name)<=4:
            self.__name = name
            print("姓名录入成功")
        else:
            print("姓名长度输入错误，已设置成默认值' ' ")
            self.__name = ' '

        if balance>=10 :
            self.__balance = balance
            print("余额录入成功")
        else:
            print("余额输入错误，已设置成默认值0 ")
            self.__balance = 0

        if len(str(password))==6:
            self.__password = password
            print("密码录入成功")
        else:
            print("密码长度输入错误，已设置成默认值000000 ")
            self.__password = 000000
##nm,写错了,写成更新吧
    def update_info(self, name, balance, password):
        self.__name = name
        self.__balance = balance
        self.__password = password
#再写个输出封装属性
    def show_info(self):
        print(f"姓名:{self.__name}")
        print(f"余额:{self.__balance}")
        print(f"密码:{self.__password}")

user1 = Account("钟永超",31231231231221512511,123412)
user1.show_info()
```

补充:\_xxx:单下划线开头，声明私有属性/方法，如果定义在类中，外部可以使用，子类也可以继承

单下划线可以直接用print(\_属性名)输出,这个和双下划线不一样

一般是为了避免与Python关键字冲突而采用的命名方法

#### 继承(爸爸去哪了)

**案例：**

1. 定义两个类，Python初级工程师，Python高级工程师
1. 属性： name ，age ，私有属性 salary
1. 方法： 编写代码

<u>看下面这个案例,定义两个类的方法和属性就很冗余复杂了,所以我们用继承</u>

```python
#  初级工程师
class PythonJunior:
    name = None
    age = None
    __salary = None

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def setSalary(self, salary):
        self.__salary = salary

    # 编写代码
    def write_code(self):
        print(f'{self.name}是Python初级开发工程师,会编写简单的Python代码')


#  高级工程师
class PythonSenior:
    name = None
    age = None
    __salary = None

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def setSalary(self, salary):
        self.__salary = salary

    # 编写代码
    def write_code(self):
        print(f'{self.name}是Python高开发工程师,会编写复杂的Python代码')

#  创建初级工程师实例
python_junior = PythonJunior('xiaoming', '20')
python_junior.setSalary(5000)
print(f'{python_junior.name}:{python_junior.age}')

python_senior = PythonSenior('Tom', '45')
python_senior.setSalary(10000)
print(f'{python_senior.name}:{python_senior.age}')
```

**分析：**

1. 两个类中有很多相同的属性和方法
1. 代码的复用性差
1. 不利于代码的管理

**解决：**

使用类继承的方案，可以解决以上分析的问题

1. 继承可以解决代码复用
1. 当多个类存在相同属性和方法时，可以从这些类中抽取出父类，在父类中定义这些相同的属性和方法，所有的子类中则不需要在重复的定义

```python
class Engineer:

    name = None
    age = None
    __salary = None

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def setSalary(self, salary):
        self.__salary = salary


#  初级工程师
class PythonJunior(Engineer):

    # 编写代码
    def write_code(self):
        print(f'{self.name}是Python初级开发工程师,会编写简单的Python代码')


#  高级工程师
class PythonSenior(Engineer):

    # 编写代码
    def write_code(self):
        print(f'{self.name}是Python高开发工程师,会编写复杂的Python代码')

#  创建初级工程师实例
python_senior = PythonSenior('tom',25)
python_senior.write_code()

python_junior = PythonJunior('jack',20)
python_junior.write_code()
```

**继承的重写(多态的思想)**

对继承但要改变的方法重新在自己的类里面写一个新的

```python
class Pythongcs(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.__salary = 0          # 私有属性初始值

    def set_salary(self, salary):
        self.__salary = salary
        
    def get_salary(self):  #这里写一个调用私有属性的方法,才能让子类成功调用私有属性
        return self.__salary

    def write_code(self):
        print(f"{self.name}是一个初级Python工程师，今年{self.age}岁，工资是:{self.__salary}")

class Pythonloser(Pythongcs):
    pass

class Pythonheign(Pythongcs):
    def write_code(self):  #继承的重写,如果子类和父类有不同的具体方法,可以使用继承的重写
        print(f"{self.name}是一个高级Python工程师，今年{self.age}岁，工资是:{self.get_salary()}")   #调用父类提供的方法才能调用父类的私有属性

# 实例化并测试
loser = Pythonloser('张三', 24)
heign = Pythonheign('王五', 34)

loser.set_salary(8000)
heign.set_salary(20000)

loser.write_code()
heign.write_code()
```

输出:

张三是一个初级Python工程师，今年24岁，工资是:8000

王五是一个高级Python工程师，今年34岁，工资是:20000

****

****

**继承细节**

1.子类可以继承父类所有的属性和方法，非封装私有的属性和方法在子类中可以直接访问，

<u>***但私有的属性和方法子类想要使用的话需要父类提供公开的操作方法,子类调用父类提供的该方法才能访问调用父类的属性和方法,***</u>

<u>***写子类的时候调用用self.该方法名,实例对象调用的时候用对象名.该方法名***</u>

```python
class Base:
    # 公共属性
    public_attr = 'public_attr'
    # 私有属性
    __private_attr = '_private_attr'

    def public_method(self):
        print('这是一个公开的方法')
    def show_private_attr(self):  #调用私有属性的方法
        print(self.__private_attr)
        
    def __private_method(self):
        print('这是一个私有的方法')    
    def show_private_method(self):    #调用私有方法的方法
        self.__private_method()
class Aoo(Base):
    pass

aoo = Aoo()
#  访问公开属性
print(aoo.public_attr)
# 访问私有属性   AttributeError: 'Aoo' object has no attribute '__private_attr'. Did you mean: '_Base__private_attr'?
print(aoo.__private_attr)

# 访问公开方法
aoo.public_method()
# 访问私有方法   AttributeError: 'Aoo' object has no attribute '__private_method'. Did you mean: '_Base__private_method'?
aoo.__private_method()
#正确访问私有属性方式
aoo.show_private_attr()
#正确访问私有方法方式
aoo.show_private_method()
```

2.Python中Object类是所有其它类的基类 ctrl+h

![图片](Python高级笔记_assets/ch1_img08.png)

3.python支持多继承,两个爸爸666

```python
#  歌手 singer
class Singer:
    type1 = '流行'

    def sing(self, song):
        print(f'我会演唱的{self.type1}歌曲的名字是{song}')

# 演员  actor
class Actor:
    type2 = '喜剧'

    # 表演
    def performance(self, info):
        print(f'我是一个{self.type2}演员,出演的作品是<<{info}>>')


class Person(Singer,Actor):  #继承两个就用逗号连接就行
    pass


person = Person()
print(f'person.type1 = {person.type1}')
print(f'person.type2 = {person.type2}')

person.sing("大风吹")
person.performance("举起手来")
```

4.python多继承时，如果有同名成员，**遵循从左到右的优先级**(写在左边的父类的优先级高，右边的优先级低)

```python
#  歌手 singer
class Singer:
    type1 = '流行'

    def sing(self, song):
        print(f'我会演唱的{self.type1}歌曲的名字是{song}')


# 演员  actor
class Actor:
    type1 = '喜剧'

    # 表演
    def performance(self, info):
        print(f'我是一个{self.type1}演员,出演的作品是<<{info}>>')


class Person(Singer,Actor):
    pass


person = Person()

print(f'person.type1 = {person.type1}')   #两个都是type1

person.sing("大风吹")
person.performance("举起手来")
```

5.继承链

简单理解,优先继承父类,没有的话可以继承父类的父类

我他喵的还能继承爷爷,太爽了

```python
class Aoo:
    var1 = 'class AOO'
    var2 = "hello Aoo"

class Boo(Aoo):
    var1 = 'class Boo'
    var3 = "hello boo"

class Coo(Boo):
    var1 = 'class Coo'


coo = Coo()

print(f'coo.var1 = {coo.var1}')
print(f'coo.var2 = {coo.var2}')
print(f'coo.var3 = {coo.var3}')
```

6.子类中如何访问父类的成员( super()  )

如果子类中出现了和父类同名的成员，可以通过父类名或super()实现父类元素的访问

**访问父类的同名属性**

1. 父类名.属性名
1. super().属性名   #可以随着继承链一直往上找

**访问父类的同名方法**

1. 父类名.方法名（self）
1. super().方法名

```python
class Aoo:
    var1 = 'hello Aoo'

    def say_hello(self):
        print("hello Aoo")


class Boo(Aoo):
    var1 = 'hello Boo'   #和父类有同名变量

    def say_hello(self):  #和父类有同名方法
        print("hello Boo")

    def test_method1(self):
        print(self.var1)  #访问自己的同名变量
        print(Aoo.var1)   #用父类类名访问父类的同名变量

        self.say_hello()    #调用自己的同名方法
        Aoo.say_hello(self)    #用父类类名调用父类的同名变量

    def test_method2(self):
        print(self.var1)   #访问自己的同名变量
        print(super().var1)#用super()方法访问父类的同名变量

        self.say_hello() #调用自己的同名方法
        super().say_hello()#用super()方法调用父类的同名变量

boo = Boo()  #实例化对象
boo.test_method1()
boo.test_method2()
```

练习：

编写Car类，包含宽、高、重量等属性

1)get\_details方法用于返回Car的详细信息

2)编写Roadster子类，继承Car类，添加特有属性【品牌brand】

3)编写Bus子类，继承Car类，添加特有属性【color】

4)完成测试，创建Roadster和Bus对象，分别给对象中特有的属性赋值,以及从Car类继承的属性赋值，

5）使用打印方法print\_details输出信息

```python
# 汽车类
class Car:

    def __init__(self, width, height, weight):
        self.width = width
        self.height = height
        self.weight = weight

    # 获取车子详情
    def get_details(self):
        return f'车子的宽度是{self.width}, 高度是{self.height}, 重量是:{self.weight}'
```

```python
# 跑车类
class Roadster(Car):

    def __init__(self, width, height, weight, brand):  
        self.width = width   #也可以单独写一个set_brand方法,调用这个方法设置brand
        self.height = height
        self.weight = weight
        self.brand = brand

    def print_details(self):
        return f'{self.brand} {self.width} {self.height} {self.weight}'
```

```python
#  巴士类
class Bus(Car):

    def __init__(self, width, height, weight, color):
        self.width = width
        self.height = height
        self.weight = weight
        self.color = color

    # 使用super关键词 调用父类中的成员方法
    def print_details(self):
        return f'{self.color} {super().get_details()}'
```

```python
roadster = Roadster(10, 10, 10, '法拉利')
bus = Bus(20, 20, 20, '白色')

print(roadster.get_details())
print(bus.get_details())


print(roadster.print_details())
print(bus.print_details())
```

#### 多态(同一个方法不同对象,有不同结果)

多态是一种思想,为了优化代码方法,提高代码的可读性

**核心思想是**：同一个操作或方法，作用于不同的对象上，可以产生不同的行为结果

多态: 多种形态

实现的主要方式是重写方法和写多态函数

1. 子类方法重写（继承多态）—— 最经典

这是面向对象最标准的多态：父类定义接口，子类重写方法。调用时，传入不同的子类对象，执行各自的逻辑。

```python
class Animal:
    def speak(self):
        raise NotImplementedError("子类必须实现 speak 方法")

class Dog:
    def speak(self):
        return "汪汪"

class Cat:
    def speak(self):
        return "喵喵"

class Duck:
    def speak(self):
        return "嘎嘎"

# 多态函数：不关心对象是什么类型，只关心有没有 speak 方法
def animal_sound(animal: Animal):   # 类型注解仅提示，不强制,类也可以成为参数对象
    print(animal.speak())

dog = Dog()
cat = Cat()
duck = Duck()

animal_sound(dog)   # 输出: 汪汪
animal_sound(cat)   # 输出: 喵喵
animal_sound(duck)  # 输出: 嘎嘎
```

2. 鸭子类型（Duck Typing）—— Python 的精髓(不需要继承父类)

Python **不强制要求子类继承父类**。只要一个对象有 `speak()` 方法，它就能被 `animal_talk` 函数接受。这种“**如果它走起来像鸭子，叫起来像鸭子，那它就是鸭子**”的哲学，让多态变得极其灵活。

```python
#承接上面
class Person:
    def speak(self):
        return "你好，人类！"

class Robot:
    def speak(self):
        return "哔哔，正在执行语音模块..."

# 注意：Person 和 Robot 没有任何继承关系，但都实现了 speak(),也是不同的结果
animal_talk(Person())  # 输出：你好，人类！
animal_talk(Robot())   # 输出：哔哔，正在执行语音模块...
```

继承多态(相当于继承的方法重写)(同上面)

```python
#父类
class Animal:
    def speak(self):
        print("动物的叫声")
class Dog(Animal):
    def speak(self):
        print("小狗汪汪叫")

class Cat(Animal):
    def speak(self):
        print("小猫喵喵叫")

class Duck(Animal):
    def speak(self):
        print("小鸭嘎嘎叫")

#调用:
dog = Dog()
cat = Cat()
duck = Duck()
dog.speak()
cat.speak()
duck.speak()
```

<u>***练习***</u>

![图片](Python高级笔记_assets/ch1_img09.png)

请定义合适的类，来描述以上场景

1. 定义食物类（Food 及其子类）

```python
class Food:
    """食物基类"""
    def __init__(self, name: str):
        self.name = name

    def __str__(self):
        return self.name

class Fish(Food):
    """鱼肉类，继承 Food"""
    def __init__(self):
        super().__init__("鱼肉")

class Bone(Food):
    """骨头类，继承 Food"""
    def __init__(self):
        super().__init__("骨头")
```

1. 定义动物类（Animal 及其子类）

```python
class Animal:
    """动物基类"""
    def __init__(self, name: str):
        self.name = name

    # 这个方法由子类重写，体现多态
    def eat(self, food: Food):
        print(f"{self.name} 正在吃 {food}")   #随便写反正要重写

class Cat(Animal):
    def __init__(self, name: str):
        super().__init__(name)

    # 重写 eat 方法，猫只爱吃鱼（多态表现）
    def eat(self, food: Food):
        if isinstance(food, Fish):    #isinstance(food, Fish) 用于判断 food 这个对象是不是 Fish 类（或者 Fish 的子类）的实例
            print(f"🐱 {self.name} 开心地吃 {food}！")
        else:
            print(f"🐱 {self.name} 闻了闻 {food}，嫌弃地走开了...")

class Dog(Animal):
    def __init__(self, name: str):
        super().__init__(name)

    # 重写 eat 方法，狗只爱啃骨头（多态表现）
    def eat(self, food: Food):
        if isinstance(food, Bone):
            print(f"🐶 {self.name} 疯狂啃 {food}！真香！")
        else:
            print(f"🐶 {self.name} 舔了舔 {food}，不感兴趣。")
```

1. 定义主人类（Master） ** 实际上是写“工具人”函数（触发多态的地方）**

```python
class Master:
    """主人类，负责喂食"""
    def __init__(self, name: str):
        self.name = name

    # 多态函数：只要 animal 有 eat() 方法，不管它是什么子类  我喜欢叫工具人函数
    def feed(self, animal: Animal, food: Food):   #类型注解,以类为对象传递
        print(f"👨 {self.name} 把 {food} 递给 {animal.name}")
        animal.eat(food)   # 这里动态调用对应子类的 eat 方法
```

1. 使用示例（展示多态） **实例化并调用:****把具体对象丢进工具人函数(多态函数)里。**

```python
if __name__ == "__main__":
    # 创建食物
    fish = Fish()
    bone = Bone()

    # 创建动物
    cat = Cat("咪咪")
    dog = Dog("旺财")

    # 创建主人
    master = Master("老张")

    # 主人用不同食物喂不同动物 —— 多态的效果 ---调用工具人函数
    master.feed(cat, fish)   # 猫吃鱼 → 开心
    master.feed(cat, bone)   # 猫啃骨头 → 嫌弃
    master.feed(dog, bone)   # 狗啃骨头 → 疯狂
    master.feed(dog, fish)   # 狗吃鱼 → 不感兴趣
```

结果

```python
👨 老张把 鱼肉 递给 咪咪
🐱 咪咪 开心地吃 鱼肉！
👨 老张把 骨头 递给 咪咪
🐱 咪咪 闻了闻 骨头，嫌弃地走开了...
👨 老张把 骨头 递给 旺财
🐶 旺财 疯狂啃 骨头！真香！
👨 老张把 鱼肉 递给 旺财
🐶 旺财 舔了舔 鱼肉，不感兴趣。
```

代码分析：

以上代码实现了具体的需求，但是如果后续主人饲养的动物很多，则需要定义更多的方法，食物也是一样，会传递更多的参数，这样的问题可以通过python中的多态进行解决

什么是多态？

不同的对象调用相同的方法，表现出不同的状态，称之为多态。

一个重要的函数，判定数据的类型

`isinstance(`bone,Bone`)` 用于判断 `food` 这个对象**是不是** `Fish` 类（或者 `Fish` 的子类）的实例

```python
print(f'bone is bone {isinstance(bone,Bone)}')
print(f'bone is Food {isinstance(bone,Food)}')
print(f'bone is Animal {isinstance(bone,Animal)}')
```

![图片](Python高级笔记_assets/ch1_img10.png)

**多态辅助理解补充**

**1.**

**没有多态的世界（你只能这样写）：**

老板（函数）必须认识每一个员工，并且记住他们的特长：

python

```
def boss_command(animal):
    # 老板必须亲自检查：你是猫？还是狗？还是猪？
    if isinstance(animal, Cat):
        animal.catch_mouse()  # 猫抓老鼠
    elif isinstance(animal, Dog):
        animal.guard_house()  # 狗看门
    elif isinstance(animal, Pig):
        animal.eat_sleep()    # 猪吃了睡
    # 如果新来一个 Duck（鸭子），老板还得修改这个函数，加上 elif！
```

**有多态的世界（你只需要这样写）：**

python

```
def boss_command(animal):
    # 老板不管你是啥，只要你会执行“工作”指令就行
    animal.work()  # 这就是多态！统一的指令！
```

**2.**

用一个“付款”场景戳破你的错觉

假设你要写一个支付系统：

- 支付宝（AliPay）的支付方法是 `alipay.pay()`
- 微信（Wechat）的支付方法是 `wechat.scan()`
- 银行卡（Card）的支付方法是 `card.swipe()`

**如果你觉得多态是废话，你的代码会这样写（灾难）：**

python

```
def checkout(payment_type, amount):
    if payment_type == "alipay":
        alipay.pay(amount)
    elif payment_type == "wechat":
        wechat.scan(amount)  # 注意，方法名还不一样！
    elif payment_type == "card":
        card.swipe(amount)   # 方法名又不一样！
```

哪天接入“云闪付”，你是不是得改这个函数？改出 Bug 谁负责？

**如果你懂多态（真正的优雅）：**

我们强行规定，所有支付方式都必须有一个统一的接口叫 `pay()`（多态约束）。

python

```
def checkout(payment, amount):
    payment.pay(amount)  # 就这一行！永远不用改！
```

无论以后增加 100 种支付方式，`checkout` 函数**一个字都不用碰**！

**3.简单多态函数写法**

![图片](Python高级笔记_assets/ch1_img11.png)

### 抽象类

abc模块

1. **默认情况下，Python不提供抽象类(所以不重要)**，Python附带一个模块，该模块为定义抽象类提供了基础，该模块称之为abc
1. 当我们需要抽象基类时，让父类继承ABC（abc模块的类），使用@abstractmethod声明抽象方法
1. 抽象类的价值多用于设计，让子类继承并实现抽象类的抽象方法
1. 抽象类不能创建实例对象
1. 基本上不用,工厂模式中可能会用到

```python
from  abc  import  *
class Animal(ABC):  #正常情况类是默认继承基类object的,这里继承的是抽象类abc

    @abstractmethod   #装饰器,抽象方法
    def cry(self):
        # 动物的叫声有很多,具体的叫声需要看是什么动物，类似与该情况可以将方法设计为抽象方法
        pass
# 抽象类不能创建实例对象
# Can't instantiate abstract class Animal without an implementation for abstract method 'cry'
Animal = Animal()   
```

**创建子类**

如果继承了抽象类

子类必须重写继承父类的抽象方法  否则也无法实例化

继承并重写才能实例化

```python
class Dog(Animal):
    # 子类必须重写继承父类的抽象方法  否则也无法实例化
    def cry(self):  #子类一定要写父类的抽象方法,否则报错
        print('小狗汪汪叫')
dog = Dog()
dog.cry()
```

**总结：**

1. 抽象类中可以有普通方法
1. 子类要完全实现父类中的所有抽象方法

案例：定义一个工程师的抽象类 ，然后定义初级工程师和高级工程师两个子类，实现抽象方法 write\_code

```python
from abc import ABC, abstractmethod
class Engineer(ABC):
     @abstractmethod
     def write_code(self):
         pass
     
#  初级工程师
class PythonJunior(Engineer):

    # 编写代码
    def write_code(self):
        print(f'Python初级开发工程师,会编写简单的Python代码')


#  高级工程师
class PythonSenior(Engineer):

    # 编写代码
    def write_code(self):
        print(f'Python高开发工程师,会编写复杂的Python代码')
```

### 魔术方法

**什么是魔术方法**

1. 在Python中，所有以双下划线 包起来的方法，统称为Magic Method(魔术方法)，它是一种的特殊方法，
1. 普通方法需要调用，而魔术方法不需要调用就可以自动执行
1. 魔术方法在类或对象的某些事件发生时会自动执行，让类具有神奇的"魔力"。如果希望根据自己的程序定制特殊功能的类，那么就需要对这些方法进行重写
1. Python中常用的运算符、for循环、以及类操作等都是运行在魔术方法之上的

**常见的魔术方法：**

**1.\_\_init\_\_ (self)  构造函数**

**2.\_\_str\_\_(self) **打印实例对象返回的结果,正常会返回实例对象的内存地址,但是使用这个魔术方法,就会打印这个str方法的返回值

**3.大小比较函数 :**定义某值得大小比较,如果实例对象中比较符合函数的比较,则返回True\\

**4.\_\_del\_\_:细狗函数,你懂的**

**5.\_\_new\_\_:划分内存地址,单例模式那会说**

![图片](Python高级笔记_assets/ch1_img12.png)

```python
class Student:

    def __init__(self, student_no, student_name, student_age, student_score):
        self.student_no = student_no
        self.student_name = student_name
        self.student_age = student_age
        self.student_score = student_score

    def __str__(self):   #打印实例对象返回的结果,正常会返回实例对象的内存地址,但是使用这个魔术方法,就会打印这个方法的返回值
        return f'{self.student_no} {self.student_name} {self.student_age} {self.student_score},老铁666'

    def __eq__(self, other):  #eq,一定要加other
        return self.student_no == other.student_no  #定义这个值两个相等,记住返回值要和方法定义的相同,等就是等
        
    def __ne__(self, other):  #eq,一定要加other
        return self.student_age != other.student_age  #定义这个值两个不等

# 创建实例对象
student1 = Student('20250117001', '张三', '19', '20')
student2 = Student('20250117001', '张三', '18', '20')
print(student1 == student2)  #student1.no和student2.no确实相等,返回True,这里的符号还得和上面匹配上一模一样
print(student1 != student2)  #student1.age和student2.age确实不等,返回True
print(student1)
```

**结果**

```python
True   
True
20250117001 张三 18 20,老铁666   #如果没写str函数的话这里就是student1的内存地址
```

### 魔法属性(附加)

\_\_name\_\_:类或函数的名字

\_\_doc\_\_:类或属性的介绍(用"""  """框住的)

## 第2章 错误和异常

保证代码质量

错误可（至少）被分为两种：**语法错误** 和 **异常**

### 语法错误SyntaxError

```python
while True print('Hello world')
  File "<stdin>", line 1
    while True print('Hello world')
               ^^^^^
SyntaxError: invalid syntax
```

在这个例子中，错误是在函数 [print()](https://docs.python.org/zh-cn/3.12/library/functions.html#print) 上检测到的，原因是在它之前缺少一个冒号 (':')

看到这个就知道自己语法写错了,运行完后自己处理

### 异常

**除了语法错误以外的错误称为异常**[](https://docs.python.org/zh-cn/3.12/tutorial/errors.html#exceptions)

即使语句或表达式使用了正确的语法，执行时仍可能触发错误。执行时检测到的错误称为 **异常**，异常不一定导致严重的后果：很快我们就能学会如何处理 Python 的异常。大多数异常不会被程序处理，而是显示下列错误信息

```python
10 * (1/0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero  #0不能做除数的异常
```

### 异常分类

在 Python 中，所有异常必须为一个派生自 [BaseException](https://docs.python.org/zh-cn/3.12/library/exceptions.html#BaseException) 的类的实例，内置异常类可以被子类化以定义新的异常；鼓励程序员从 [Exception](https://docs.python.org/zh-cn/3.12/library/exceptions.html#Exception) 类或它的某个子类而不是从 [BaseException](https://docs.python.org/zh-cn/3.12/library/exceptions.html#BaseException) 来派生新的异常。

#### 基类

**下列异常主要被用作其他异常的基类**。

**exception ****BaseException   (基类的起始级)**

所有内置异常的基类。 它不应该被用户自定义类直接继承 (这种情况请使用 [Exception](https://docs.python.org/zh-cn/3.12/library/exceptions.html#Exception))

**exception ****Exception**[](https://docs.python.org/zh-cn/3.12/library/exceptions.html#Exception)

所有内置的非系统退出类异常都派生自此类。 所有用户自定义异常也应当派生自此类。

**exception ****ArithmeticError**[](https://docs.python.org/zh-cn/3.12/library/exceptions.html#ArithmeticError)

此基类用于派生针对各种算术类错误而引发的内置异常: <u>OverflowError</u>, <u>ZeroDivisionError</u>, <u>FloatingPointError</u>。

**exception ****BufferError    (和电脑性能有关)**[](https://docs.python.org/zh-cn/3.12/library/exceptions.html#BufferError)

当与 <u>缓冲区</u> 相关的操作无法执行时将被引发。

**exception ****LookupError	(常见报错为**<u>IndexError</u>, <u>KeyError)</u>[](https://docs.python.org/zh-cn/3.12/library/exceptions.html#LookupError)

此基类用于派生当映射或序列所使用的键或索引无效时引发的异常: <u>IndexError</u>, <u>KeyError</u>

#### 具体异常

**exception IndexError**

当序列抽取超出范围时将被引发，也就是**索引错误**

```python
info = "hello"
print(info[5])
```

**exception ****KeyError**[](https://docs.python.org/zh-cn/3.12/library/exceptions.html#KeyError)

当在现有键集合中**找不到指定的映射（字典）键**时将被引发

```python
student = {'name': 'lisi', 'age': 30, 'score': 90}
print(f'student[name] = {student['sex']}')
# KeyError: 'sex'
```

**exception AttributeError**

当在对象中**找不到指定的属性**时将被引发

```python
class Student:
    def __init__(self,name,age):
        self.name = name
        self.age = age
    def __str__(self):
        return f'name = {self.name},age = {self.age}'

student = Student('lisi',35)
print(student)

# 访问对象中不存在的属性  
print(student.sex)
```

**exception NameError**

当某个局部或全局名称没有被找到时将被引发，**比如使用了一个没有定义的变量名**

```python
# NameError: name 'name' is not defined
print(name)
```

**exception TypeError**

当一个操作或函数使用了类型不适当的对象时被引发

```python
num = 2
for i in num:  #要可迭代对象,num是2
    print("hello")
```

**exception ValueError**

当操作或函数具有正确类型但值不合适的参数时被引发

```python
num = int("1234")
print(numb)

# ValueError: invalid literal for int() with base 10: '123a'
num = int("123a")   #123a转不成整型
print(numb)
```

**exception ZeroDivisionErro**r

**当除法或取余运算时，除数为零时被引发**

```python
# ZeroDivisionError: division by zero
print(1/0)
```

**exception FileNotFoundError**

当操作的文件找不到时,将被引发

```python
# FileNotFoundError: [Errno 2] No such file or directory: 'D:/a.txt'
with open('E:/a.txt'):
    pass
```

### 异常的处理

四个关键字  try    except    else   finally

**基本语法结构:**

```python
try:
    可能出现异常的代码   #放Try里寻求帮助
except [异常 as 别名]：
    出现异常执行的代码
[else:]
    没有发生异常执行的代码
[finally:]
    无论是否出现异常均执行的代码
```

**示例：**

```python
num1 = 1
num2 = 0

print("-------程序开始执行---------")
print(num1/num2)
print("-------程序执行结束---------")

# ZeroDivisionError: division by zero
```

1. 出现异常，会直接导致程序崩溃或结束
1. 如何实现在出现异常时，对异常进行处理保证程序的正常执行，可以使用Python中异常处理机制来实现

****[**try**](https://docs.python.org/zh-cn/3.12/reference/compound_stmts.html#try)** 语句的工作原理如下**：

- 首先，执行 **try 子句** （[try](https://docs.python.org/zh-cn/3.12/reference/compound_stmts.html#try) 和 [except](https://docs.python.org/zh-cn/3.12/reference/compound_stmts.html#except) 关键字之间的（多行）语句）。
- 如果没有触发异常，则跳过**except 子句**，[try](https://docs.python.org/zh-cn/3.12/reference/compound_stmts.html#try) 语句执行完毕。
- 如果在执行 [try](https://docs.python.org/zh-cn/3.12/reference/compound_stmts.html#try) 子句时发生了异常，则跳过该子句中剩下的部分。 如果**异常的类型与 **[**except**](https://docs.python.org/zh-cn/3.12/reference/compound_stmts.html#except)** 关键字后指定的异常相匹配**，则会执行 **except 子句**，然后跳到try/except 代码块之后继续执行。
- 如果发生的异常与 **except 子句** 中指定的异常不匹配，则它会被传递到外层的 [try](https://docs.python.org/zh-cn/3.12/reference/compound_stmts.html#try) 语句中；如果没有找到处理器，则它是一个 **未处理异常** 且执行将停止并输出一条错误消息

```python
num1 = 1
num2 = 0

print("-------程序开始执行---------")

try:   #可能出现异常的语句
    print(num1 / num2)    #捕获到异常走下面expect
except BufferError as e: #可以写多个except猜测多个异常,这个捕获不到走下面的   as是给这个异常取别名
   print(f'程序执行期间报错了,异常信息是：{e}') 
except Exception as e: #推荐使用Exception,派生错误范围很大
   print(f'程序执行期间报错了,异常信息是：{e}')    #在Exception找到对应的错误描述并打印
   
print("-------程序执行结束---------")
```

异常处理其它方式

```python
num1 = int(input('请输入第一个数字:'))
num2 = int(input('请输入第二个数字:'))

print('-------------程序开始执行----------------')
try:
    result = num1 / num2
    print(f'{num1}/{num2} = {num1 / num2}')

except IndexError as e:   #判断是不是IndexError
    print(f'程序出现异常了,异常信息是：{e},异常的类型是{type(e)}')
except ZeroDivisionError as e:   #判断是不是ZeroDivisionError
    print(f'程序出现异常了,异常信息是：{e},异常的类型是{type(e)}')
except Exception as e:   #先判断前两个,如果没有捕获到,这个是大保底
    print(f' Exception 程序出现异常了,异常信息是：{e},异常的类型是{type(e)}')
else:   #没有异常走这里
    print('程序执行没有发生任何的异常信息，非常好')

finally:   #最终总结,一定会执行
    print('程序有没有发生异常信息，我都要执行')

print('-------------程序执行结束----------------')
```

注意： 异常处理罗列多个具体异常信息时，需要注意异常罗列的顺序，范围比较大的异常需要写在子类异常的后面

**主动抛出异常(raise)**

**执行效果**：程序运行到 `raise` 这一行会立即中断，并向上层调用者抛出**指定的异常**，除非被 `try...except` 捕获。

```python
score = int(input('请输入学生分数:'))
if score < 60:   #这里使用if判断,如果程序走到指定的if语句里会抛出异常
    raise Exception('考试成绩不合格')   #raise后面放的是异常类,后面就是可以自定义异常类
else:
    print('恭喜你考试通过了')
```

结果: 抛出(提出)异常

![图片](Python高级笔记_assets/ch2_img01.png)

**自定义异常**

```python
class ScoreException(Exception):   #自定义异常的类,一定要基于异常的基类,最好用Exception
    pass

score = int(input('请输入学生分数:'))
if score < 60:
    raise ScoreException('考试成绩不合格')  #抛出异常
else:
    print('恭喜你考试通过了')
```

结果:抛出异常

![图片](Python高级笔记_assets/ch2_img02.png)

**异常传递机制**

```python
def fn3():
    print('----------fn3开始执行------------')
    result = 1 / 0
    print('----------fn3执行结束------------')
```

```python
def fn2():
    print('----------fn2开始执行------------')
    fn3()
    print('----------fn2执行结束------------')
```

```python
def fn1():
    try:
        fn2()   #从内往外的抛出,但如果内部上来就报错了,那上来就寄寄了
    except:
        print('程序执行出现异常了')
#  调用fn1
fn1()
```

输出结果:

```python
----------fn2开始执行------------

----------fn3开始执行------------

程序执行出现异常了
```

理解结构:

```python
def fn3():
    print('---fn3开始执行---')
    result = 1 / 0          # 这里会触发 ZeroDivisionError
    print('---fn3执行结束---')

def fn2():
    print('---fn2开始执行---')
    try:
        fn3()               # 调用 fn3，异常在此被捕获
    except Exception as e:
        print("在fn2处发生了异常处理")   # 捕获后打印信息
    print('---fn2执行结束---')

def fn1():
    try:
        fn2()               # 调用 fn2，内部已处理异常，不会抛到外面
    except:                  # 这里不会执行，因为 fn2 已经处理了
        print('程序执行出现异常了')

# 调用 fn1
fn1()
```

输出结果

```python
---fn2开始执行---
---fn3开始执行---
在fn2处发生了异常处理
---fn2执行结束---
```

### 异常继承结构

```
BaseException
 ├── BaseExceptionGroup
 ├── GeneratorExit
 ├── KeyboardInterrupt
 ├── SystemExit
 └── Exception
      ├── ArithmeticError
      │    ├── FloatingPointError
      │    ├── OverflowError
      │    └── ZeroDivisionError
      ├── AssertionError
      ├── AttributeError
      ├── BufferError
      ├── EOFError
      ├── ExceptionGroup [BaseExceptionGroup]
      ├── ImportError
      │    └── ModuleNotFoundError
      ├── LookupError
      │    ├── IndexError
      │    └── KeyError
      ├── MemoryError
      ├── NameError
      │    └── UnboundLocalError
      ├── OSError
      │    ├── BlockingIOError
      │    ├── ChildProcessError
      │    ├── ConnectionError
      │    │    ├── BrokenPipeError
      │    │    ├── ConnectionAbortedError
      │    │    ├── ConnectionRefusedError
      │    │    └── ConnectionResetError
      │    ├── FileExistsError
      │    ├── FileNotFoundError
      │    ├── InterruptedError
      │    ├── IsADirectoryError
      │    ├── NotADirectoryError
      │    ├── PermissionError
      │    ├── ProcessLookupError
      │    └── TimeoutError
      ├── ReferenceError
      ├── RuntimeError
      │    ├── NotImplementedError
      │    └── RecursionError
      ├── StopAsyncIteration
      ├── StopIteration
      ├── SyntaxError
      │    └── IndentationError
      │         └── TabError
      ├── SystemError
      ├── TypeError
      ├── ValueError
      │    └── UnicodeError
      │         ├── UnicodeDecodeError
      │         ├── UnicodeEncodeError
      │         └── UnicodeTranslateError
      └── Warning
           ├── BytesWarning
           ├── DeprecationWarning
           ├── EncodingWarning
           ├── FutureWarning
           ├── ImportWarning
           ├── PendingDeprecationWarning
           ├── ResourceWarning
           ├── RuntimeWarning
           ├── SyntaxWarning
           ├── UnicodeWarning
           └── UserWarning
```

## 第3章 文件操作

### 本章目标

- 掌握os模块的使用
- 掌握读写文本文件的方法
- 掌握读写JSON文件的方法
- 掌握读写CSV文件的方法
- 了解Pandas处理数据的方法

### os模块(记忆)

**导入os模块才能对文件进行操作**

os模块函数太多了,记一些关键的,然后别的用的时候查

os.path.isdir('aa')    判断目录是否存在

os.mkdir('aa')          创建目录

os.path.exists('a.txt')   判断文件是否存在

os.remove('a.txt')   删除文件

f\_stat=os.stat('a.txt')     提取文件信息

f\_stat.st\_size  文件大小

f\_stat.st\_ctime  文件创建时间

f\_stat.st\_mtime 文件修改时间

f\_stat.st\_atime	文件访问时间

```python
import  os   #引入os模块才能对文件操作,os:windows
import  time  #后面的时间函数需要time模块

# 判定目录并创建
if os.path.isdir('aa'):   #相对路径绝对路径   判断是否存在
    print('aa目录存在')
else:
    print('文件不存在，文件夹已创建')
    os.mkdir('aa')   #创建目录

# 删除文件
if os.path.exists('a.txt'):
    os.remove('a.txt')
else:
    print('文件不存在')
    
# 查看文件信息
f_stat = os.stat('b.txt')   #这里写一个不一样的文件名
print(f'文件大小为:{f_stat.st_size}')
print(f'文件创建时间为:{time.ctime(f_stat.st_ctime)}')
print(f'文件修改时间为:{time.ctime(f_stat.st_mtime)}')
print(f'文件访问时间为:{time.ctime(f_stat.st_atime)}')
```

现在有一个文件（movie.txt），记录了漫威电影的故事线，现在我们想要读取这个文件获得所有信息：

![图片](Python高级笔记_assets/ch3_img01.png)

应该如何读取？是一次性读取所有信息？还是按行读取信息？不用担心，Python可以帮你将这两种方式统统实现

**Python打开文件实现方式  with  open**

### 读写txt文本文件

**实现方式  with  open**

**语法**：

```python
with open('文件路径', '读写模式', encoding='文件编码') as file:
```

> 💡 file.方法
>
> file是一个变量或者理解成一个别名,读进来之后实例成这个名字,后面再调用就用这个file名
>
> 可以用file,也可以随便取名
>
> with 语句会在包裹的代码块执行完成后自动关闭打开的文件,为了释放内存
>
> open() 函数用于打开指定文件
>
> as 将文件对象赋值给变量，用于在代码块中操作文件

**读写模式**

文件的读写有以下几种模式：

`'w'`** 模式的铁律**：只要用 `'w'` 打开文件，Python 会**立刻清空**这个文件（相当于把旧文件删了，新建一个空的）。

![图片](Python高级笔记_assets/ch3_img02.png)

其中，如果模式要求文件必须存在，而文件在我们的路径中不存在时，程序就会报错

**编码**

我们要知道，计算机的底层存储是01二进制码，所以文件存储在计算机里时，需要经过编码才能进行存储。

**文件编码**

![图片](Python高级笔记_assets/ch3_img03.png)

**注：当指定编码与文件实际编码不一致时，会造成乱码或数据丢失问题**

**在Python中，文件编码非必须，通常为UTF-8**。Python 3默认使用UTF-8编码，但在读取文件时，如果没有指定编码方式，可能会遇到SyntaxError 错误，特别是当文件包含非ASCII字符（如中文字符）且没有正确指定编码时

**Pyhton文件路径**

./同级,也可以省略

../上级

- `./` 代表**当前工作目录（Current Working Directory）**
- `../` 代表**当前工作目录的父级目录**

#### 读取文本文件file.read()

- ****一次性读取****

使用 file.read() 实现：这个file就是with open的file

```python
# 1.打开文件
with open('movie.txt', 'r', encoding='utf-8') as f:
 
# 2.读取整个文件内容
 content = f.read()
# 打印文件内容
print(content)
```

file.read()** 一次性拿到了所有的文件数据**运行以后，程序将会打印完整的文件内容

- ****按行读取****

<u>实际就是遍历file</u>

有些时候，我们会遇到很大的文件，如果一次性读取它们的内容，有可能导致内存溢出。而且如果我们不想一次性读取全部的信息，而是想完成一个筛选的工作，比如，我想找这个txt文件中，关于钢铁侠的信息。这时候，我们就要**使用循环，遍历信息并找到自己希望使用的信息**

```python
print('-------只输出钢铁侠电影信息-----------')
# 1.打开文件
with open('movie.txt', 'r', encoding='utf-8') as file:

# 2.迭代器逐行读取
for line in file:
# 3.处理每一行
    if "Iron Man" in line:
    # 打印包含关键词的行
    print(line)
    #每个输出的信息有空行处理
    # print(line.strip('\n'))   #去除字符串两边的东西,在字符串那讲过
```

我们使用 for 循环，一条一条地读取了文件中的每行信息，并检查行中是否有“ Iron Man ”（也可以检查是否有“ 钢铁侠 ”），如果有，说明是钢铁侠的信息，就打印

#### 写入文本文件file.write()

往**file.write() ** 括号里写内容

读和写的语法区别并不大，关键在于读写模式需要更改：r改成w或者a

不管是a还是w,如果文件不存在会自动创建

```python
with open('文件路径', '写/追加模式', encoding='文件编码') as file:
    file.write()
```

在写入文件时，我们会遇到两种情况：

- ****将信息写入文件:w****

通过例子来理解一下：

```python
#追加文件
with open("D:\\test.txt",mode="w",encoding="utf-8") as file:
    file.write("中秋快乐呀！")
```

在这里， mode="w" 。 **w 模式是只写模式，它会清空文件；当文件不存在时，会新建文件**。我们运行一下，现在去自己电脑的D盘查看是否多了一个test.txt文件

![图片](Python高级笔记_assets/ch3_img04.png)

打开文件，确实写入了“中秋快乐呀！”：

![图片](Python高级笔记_assets/ch3_img05.png)

- ****将信息追加到文件:a****

当不想直接覆盖文件，避免导致重要信息丢失时，我们就可以使用“追加”：

```python
#追加文件
with open("D:\\test.txt",mode="a",encoding="utf-8") as file:
    file.write("中秋快乐呀！")
    file.write("国庆快乐呀！")
```

在这里，设置了 mode="a" ，如果文件不存在，它会创建新文件，如果文件存在，它会在文件的末尾继续写文件。运行结果如下：

![图片](Python高级笔记_assets/ch3_img06.png)

可以看到，我们上一次使用 w 模式写的内容依然存在，使用 a 模式连续写入两次都没有影响之前写入的内容

<u>***案例1***</u>

```
'''
案例1
用户输入用户名和密码写入到文件中，实现用户注册的功能
用户名只能是英文或者中文符号并且必须大写字母开头,且必须包含@符号
密码不能少于10位且只能是数字
'''
def register():
    list1=[]
    list_name = []
    with open("Users.txt",'r',encoding='utf-8') as f:
        for i in f:
            i = i.strip('\n')
            list1.append(i)
            list_name.append(i[:i.find('-')])

    username = input("请输入你的用户名:")
    password = input("请输入你的密码:")
    users = username + '-'+password
    if username in list_name:
        raise "用户名已存在"
    elif username[0] != username[0].upper():
        raise "首字母必须大写"
    elif '@' not in username:
        raise "名字必须带有@符号"


    if len(password) <10:
        raise "密码长度不能少于10位"
    elif not password.isdigit():
        raise "密码只能是纯数字"

    with open("Users.txt",'a',encoding='utf-8') as f:
        f.write(users+"\n")


register()
```

### 读写JSON文件

#### JSON文件

JSON是一种**轻量级的数据交换格式**，它基于文本，易于人阅读和编写，同时也易于机器解析和生成。JSON 格式的数据以键值对的形式组织，结构清晰，格式简洁。同学们可以访问链接详细了解：

> 💡 [https://www.json.org/json-zh.html](https://www.json.org/json-zh.html)

![图片](Python高级笔记_assets/ch3_img07.png)

JSON中包含**两种数据结构：对象和数组**。不就是字典和列表吗

json里面就是字典列表

- **{} 保存对象， [ ] 保存数组。**
- **数组间的元素使用逗号 , 分隔。**
- **对象为键值对形式，键必须是字符串，必须使用双引号括起来，键值对间使用逗号 , 分隔。**
- **之前好像说是Python爬出来的文件格式存储方式主要就是json**

下例就是一个对象：

```json
{
 
    "title": "钢铁侠",
    "year": 2008,
    "score": 7.9，
    "showing": false
}
```

JSON中也可以有多个对象（下例是两个对象，不同对象之间用逗号进行了分隔,整体是一个数组(列表)）：

```json
[
    {
        "title": "钢铁侠",
        "year": 2008,
        "score": 7.9,
        "showing": false
    },
    {
        "title": "银河护卫队",
        "year": 2014,
        "score": 8.0,
        "showing": false
    }
]
```

**数组中包含对象，对象中也可以有数组**，通过下面这个示例，我们可以看到， [] （数组）包含了很多个 {} （对象），对象中包含的键值对“ cast ”，它的值是数组

```json
[
    {
        "title": "钢铁侠",
        "cast": [
            "小罗伯特·唐尼",
            "格温妮斯·帕特洛"
        ]
    },
    {
        "title": "复仇者联盟",
        "cast": [
            "小罗伯特·唐尼",
            "克里斯·埃文斯"
        ]
    },
    { 
        "title": "银河护卫队",
        "cast": [
            "克里斯·帕拉特",
            "索菲亚·布什"
        ]
    }
]
```

现在也有很多可以验证JSON格式的工具：

> 💡 [https://www.json.org.cn/tools/JSONEditorOnline2.0/index.htm](https://www.json.org.cn/tools/JSONEditorOnline2.0/index.htm)

> 💡 [https://www.json.cn/](https://www.json.cn/)

#### 读取JSON文件 ---json.load(file)

别写成loads了

依旧要先用with open打开

使用 open() 函数打开文件,通过json模块实现读写,读取JSON文件可以通过 **json.load(file)** 实现

比txt需要多导入一个json包

```python
import json
#读取json文件
with open("data.json",mode="r",encoding="utf-8") as input_file:
    data=json.load(input_file)
    print(type(data))
    print(data)
```

成功读取了一个UTF-8编码的data.json文件：

![图片](Python高级笔记_assets/ch3_img08.png)

使用for循环读取键值对的内容：

```python
#读取json文件
import json
with open("data.json","r",encoding="UTF-8") as file:
     data=json.load(file)
        for item in data:
            for key in item:
                print(f"{key}:{item[key]}")
```

运行结果如下：

![图片](Python高级笔记_assets/ch3_img09.png)

#### 写入JSON文件---json.dump(*,*,*,*)

别写成dumps了

JSON文件的写入通常使用 json.dump() 函数进行实现。其中：

JSON文件的写入内容要提前写好

- data ：**必须参数，需要写入的数据**
- output\_file ：**必须参数，指定数据写入的文件**
- indent ：非必须参数，用于美化输出,建议设置为2代表缩进为 2 个空格,如果不用，数据就是平铺显示不好看
- ensure\_ascii ：非必须参数，用于设置输出时的字符编码，默认为True,建议设置为False，以确保中文字符正确显示

```python
#新添加到列表中的元素
data=[]  #空列表,需要写入的内容放在里面,不写也行,后面都用new_data
new_data = {
    "title": "蜘蛛侠: 英雄归来",
    "year": 2017,
    "score": 7.5,
    "showing": False,
    "cast": [
        "汤姆·赫兰德",
        "小罗伯特·唐尼"
        ]
}
data.append(new_data)  #不写data的话也不用写这个
#JSON只能整体读入/写出
with open("data_new.json","w",encoding="utf-8") as output_file:
     #ensure_ascii=False表示允许输出非英文字母
     #indent=2使输出的JSON结构更好看
     json.dump(data,output_file,ensure_ascii=False,indent=2)
```

我们将new\_data添加到了原来的data中，并将数据写入到新文件data\_new.json。这里注意**JSON只能整体的读入/写出**

**输出结果：**

![图片](Python高级笔记_assets/ch3_img10.png)

如果我们不设置 ensure\_ascii=False,indent=2 ，输出的结果就不如刚刚美观，由于Jupyter会自动帮我们解析结构，所以可能看不出区别。如果我们使用VSCode、Pycharm或者其他软件打开：

- 设置 ensure\_ascii=False,indent=2

![图片](Python高级笔记_assets/ch3_img11.png)

- 不设置 ensure\_ascii=False,indent=2

![图片](Python高级笔记_assets/ch3_img12.png)

不仅看不出Json的结构，中文字符也无法正确显示

### 读写CSV文件

#### CSV文件

- csv是一种简单的文件格式，**用于存储表格数据**。
- csv文件**由纯文本组成，每行代表一个数据记录**。
- 每个**数据项之间用逗号或其他字符分隔**。
- 可用文本、Excel打开
- csv比xlsx更好看在Python中,分隔符是指定好的,

![图片](Python高级笔记_assets/ch3_img13.png)

#### Pandas读CSV文件---pd.read_csv()

with open也可以读取csv文件,但我们主要用的是pandas

使用 pd.read\_csv() 实现读CSV文件而不需要 with 和 open

```python
import pandas as pd    #导入pandas库
df=pd.read_csv("movie.csv")   #dataframe  , 怀念
```

> 💡 补充：如果遇到 No module named pandas 的报错，表明没有Pandas库，使用 pip install  pandas 安装。

![图片](Python高级笔记_assets/ch3_img14.png)

除此之外，Pandas也提供了大量数据处理相关的函数：

- 获取指定行的数据： df.iloc[pos]
- 获取指定列的数据： df[col\_name]
- 

使用csv模块读取文件(with  open方法,用的比较少)

```python
import csv
with open('data.csv', 'r') as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)
```

#### Pandas写CSV文件---df.to_csv(save_path)

**使用 df.to\_csv(save\_path) 进行文件的写入**：

```python
import  pandas as pd
data = {'Name': ['Tom', 'Jane', 'Alice', 'Bob'],
        'Age': [28, 34, 29, 32],
        'City': ['New York', 'Paris', 'London', 'Berlin']}
df = pd.DataFrame(data)  #把零散的 Python 数据，变成带有行和列的二维表格
df.to_csv('test.csv')  #保存成csv文件
```

运行结果：

![图片](Python高级笔记_assets/ch3_img15.png)

**使用标准库的csv模块将数据写入CSV文件(少用)**

```python
import csv

# 准备要写入的数据
data = [
    ["姓名", "年龄", "城市"],  # 表头
    ["张三", 25, "北京"],
    ["李四", 30, "上海"],
    ["王五", 28, "广州"]
]

# 打开一个文件用于写入（如果文件不存在会自动创建）
with open("example.csv", mode="w", encoding="utf-8") as file:
    writer = csv.writer(file)  # 创建一个csv.writer对象
    # 写入数据
    for row in data:
        writer.writerow(row)  # 写入一行数据

print("数据已写入到example.csv文件中")
```

**如果csv文件已经有表头或内容了,怎么写入文件**

```
name=input("请输入学生姓名")
age=float(input("请输入学生年龄"))
score=int(input("请输入学生成绩"))
new_data = {
    "姓名": [name],
    "年龄": [age],
    "成绩": [int]
}
df_new = pd.DataFrame(new_data)

和json,txt文件差不多,用mode a追加模式,定义一个新csv追加到原csv中
df_new.to_csv("students.csv", mode='a', header=False, index=False, encoding="utf-8-sig")
```

### 读写非文本文件

案例：

1.实现图片资源的拷贝

```python
f_src_path = 'tomcat.png'
f_dst_path = 'tomcat_copy.png'

f_src = open(f_src_path, 'rb')
f_dst = open(f_dst_path, 'wb')
# 读取图片资源
data = f_src.read()
# 写出图片资源
f_dst.write(data)

# 关闭资源
f_src.close()
f_dst.close()
```

使用循环优化

```python
f_src_path = 'tomcat.png'
f_dst_path = 'tomcat_copy.png'
#  rb   wb 二进制文件
f_src = open(f_src_path, 'rb')
f_dst = open(f_dst_path, 'wb')
# 读取图片资源
while True:
    data = f_src.read(1024)
    if not data:
        break
    # 写出图片资源
    f_dst.write(data)

# 关闭资源
f_src.close()
f_dst.close()
```

使用with格式优化

```python
f_src_path = 'tomcat.png'
f_dst_path: str = 'tomcat_copy.png'
#  rb   wb 二进制文件
with open(f_src_path, 'rb') as f_src:
    with open(f_dst_path, 'wb') as f_dst:
        while True:
            data = f_src.read(1024)
            if not data:
                break
            f_dst.write(data)
```

2.遍历文件夹，判定文件是文本文件还是目录

```python
import os
f_list = os.listdir('D:/maven3.9.4/')
for i in f_list:
    if os.path.isdir('D:/maven3.9.4/' + i):
        #    获取文件名字
        print(f'{os.path.basename(i)}是文件夹')
    else:
        print(f'{os.path.basename(i)}是文件')
```

![图片](Python高级笔记_assets/ch3_img16.png)

递归遍历文件

```python
def get_file_list(path):
    file_list = os.listdir(path)
    for i in file_list:
        if os.path.isdir(path + '/' + i):
            get_file_list(path + '/' + i)
        else:
            print(path + '/' + i)

get_file_list('D:/maven3.9.4')
```

删除非空文件

```python
import os
def clear_dir(path: str):
    """
    清空并删除指定目录。
    :param path: 要清空的目录路径
    """
    if not os.path.exists(path):
        print(f"目录 {path} 不存在，无需清空。")
        return

    # 遍历目录中的所有文件和子目录
    for item in os.listdir(path):
        item_path = os.path.join(path, item)  # 获取完整路径
        if os.path.isdir(item_path):  # 如果是子目录，递归清空
            clear_dir(item_path)
        else:  # 如果是文件，直接删除
            os.remove(item_path)

    # 删除当前目录
    try:
        os.rmdir(path)  # 只能删除空目录
        print(f"目录 {path} 已被清空并删除。")
    except OSError as e:
        print(f"无法删除目录 {path}，可能是因为目录不为空或路径错误。错误信息：")
```

### 练习

1.编写一个Python程序，实现以下功能：

- 创建一个名为employees.json的JSON文件，其中包含以下嵌套结构：

```json
{
    "department": "Engineering",
    "employees": [
        {"name": "Alice", "position": "Manager", "salary": 8000},
        {"name": "Bob", "position": "Developer", "salary": 6000}
    ]
}
```

- 提示用户输入一名员工的姓名、职位和薪资，并将其添加到employees.json文件中的employees列表中。
- 从employees.json文件中读取所有员工信息，并以格式化的形式打印到控制台

```python
import json

# 读取数据
with open('file/emp.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

# 获取员工信息
emps = data['employees']


# 格式化输出
from prettytable import PrettyTable
table = PrettyTable()
table.field_names = ['name', 'position', 'salary']
for emp in emps:
    table.add_row([emp['name'], emp['position'], emp['salary']])
print(table)


# 接收新员工数据
emp_name = input('请输入员工姓名:')
emp_position = input('请输入员工职位:')
emp_salary = input('请输入员工薪资:')

emp_info = {
    'name': emp_name,
    'position': emp_position,
    'salary': emp_salary
}

# 添加新员工信息到列表
emps.append(emp_info)
# 更新员工信息到字典
data['employees'] = emps

# 将新数据写出到文件
with open('file/emp.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, ensure_ascii=False, indent=4)
```

2.编写程序实现用户的注册和登录功能，要求用户名不能重复，密码为六位并且加密处理，用户可选择是登录还是注册

```python
import hashlib
while True:
    print('*' * 20, '欢迎使用xxx管理系统', '*' * 20)
    print('1.注册')
    print('2.登录')
    print('*' * 60)
    user_choice = input('请选择(Q退出):')
    if user_choice.upper() == 'Q':
        break
    match user_choice:
        case '1':
            print('----------------注册------------------')
            with open('file/user_data.txt', 'rt', encoding='UTF-8') as f:
                data_lines = f.readlines()
            while True:
                username = input('请输入用户名:')
                for data_line in data_lines:
                    if data_line.split('@')[0] == username:
                        print('用户名已存在')
                        break
                else:
                    break
            while True:
                password = input('请输入密码:')
                if len(password) == 6:
                    break
                else:
                    print('密码长度必须为6位')
            # 密码加密
            password_hash = hashlib.md5(password.encode()).hexdigest()
            user_info_data = username + '@' + password_hash
            with open('file/user_data.txt', 'a', encoding='UTF-8') as f:
                f.write(user_info_data + '\n')
            print('用户信息注册成功')
        case '2':
            print('----------------登录------------------')
            username = input('请输入用户名:')
            password = input('请输入密码:')
            # 密码加密
            password_hash = hashlib.md5(password.encode()).hexdigest()
            user_info_data = username + '@' + password_hash
            with open('file/user_data.txt', 'rt', encoding='UTF-8') as f:
                data_lines = f.readlines()
            for data_line in data_lines:
                if user_info_data == data_line.strip():
                    print('登录成功')
                    break
            else:
                print('用户名或密码错误')
        case _:
            break

print('----------------系统安全退出----------------------')
```

3.编写一个Python程序，实现以下功能

- 创建一个名为students.csv的CSV文件，其中包含以下列标题："Name", "Age", "Score"。
- 提示用户输入若干名学生的姓名、年龄和成绩，将这些信息逐行写入students.csv文件中。
- 从students.csv文件中读取所有学生信息，并以表格形式打印到控制台

```python
import csv
from prettytable import PrettyTable
with open('file/student.csv', newline='') as students:
    reader = csv.reader(students)
    table = PrettyTable()
    for index, row in enumerate(reader):
        if index == 0:
            table.field_names = row
        else:
            table.add_row(row)
    print(table)

students =[]
while True:
    print('---------开始录入学生信息-----------')
    stu_name = input('请输入学生姓名:')
    stu_age = input('请输入学生年龄:')
    stu_score = input('请输入学生分数:')
    students.append([stu_name, stu_age, stu_score])
    user_choice = input('是否继续:(Y/N):')
    if user_choice.upper() == 'Y':
        continue
    else:
        # 将学生信息写入csv文件
        with open('file/student.csv', 'a',newline='') as file:
            writer = csv.writer(file)
            writer.writerows(students)
        break
```

4.使用文件持久化的方式，改写之前编写的学生信息管理系统（面向对象的思路）

5.请读取一个包含以下信息的mi.csv文件，将文件中的图片下载到一个名为mi的文件加下，图片的名字为图片的comment字段

| comment | href |
| --- | --- |
| xiaomi15 | [https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/2e389157059c44d9352b42e04407cbb7.jpg](https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/2e389157059c44d9352b42e04407cbb7.jpg) |
| 小米空调 | [<u>https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/5d2e4ce0cc22c7d87981bac22d64e44d.jpg</u>](https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/5d2e4ce0cc22c7d87981bac22d64e44d.jpg) |
| 米家冰箱 | [<u>https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/d049eb6fb02d487bc99bac742821ba7b.jpg</u>](https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/d049eb6fb02d487bc99bac742821ba7b.jpg?thumb=1&w=1226&h=460&f=webp&q=90) |

```python
import csv
import requests

with open('file/mi.csv', 'r',encoding='utf-8') as f:
    dict_info = csv.DictReader(f)
    for row in dict_info:
        resp_b = requests.get(row['href']).content
        with open('file/' + row['comment'] + '.jpg', 'wb') as f:
            f.write(resp_b)
```

6.请将链接中的文件下载到本地

```
https://s1.xiaomiev.com/activity-outer-assets/0328/images/hyperfactory/chaojigongc_06_1.mp4
```

```python
import requests
with open('file/mi.mp4', mode='wb') as f:
    response_b = requests.get('https://s1.xiaomiev.com/activity-outer-assets/0328/images/hyperfactory/chaojigongc_06_1.mp4').content
    f.write(response_b)
```

## 第4章 Python高级进阶

### 课程目标

- 掌握闭包的使用
- 掌握装饰器的使用
- 掌握常见设计模式
- 掌握多线程使用
- 掌握网络编程Socket
- 可迭代对象,迭代器
- 生成器

---

### 可迭代对象,迭代器

┌可迭代对象 Iterable    #什么range,数据容器啥的

└迭代器 Iterator

#### 可迭代对象Iterable

**本质上:可迭代对象不具有迭代能力,是通过\_\_iter\_\_()方法使可迭代对象变成了迭代器**

遍历(迭代):依次从对象把一个个元素取出来的过程

数据类型:str,list,tuple,dict,set等

条件┌对象实现了\_\_iter\_\_()方法

└\_\_iter\_\_()方法返回了迭代器对象

例如:dict.values()和dict.index()他们都是返回了迭代器对象

有了这个概念我们来梳理一下**for循环的工作机制**

1.先默认执行\_\_iter\_\_()获取可迭代对象的迭代器

2.对获取到的迭代器**不断**调用\_\_next\_\_()方法来获取下一个值

注:

**\_\_next\_\_()方法,第一次用先获取迭代器的第一个值,第二次用接着获取第二个,一直到最后一个,再用就报错**

并将其赋值给临时变量

<u>***判断对象的类型是否是已知的类型:isinstanse:***</u>

语法:isinstance(object,type)

注意:type可以直接或者间接类型,基本类型或者元组

```python
from typing import Iterable

dict1 = {'name':'张三','age':24}
print(isinstance(dict1,Iterable))

#结果:True
```

#### 迭代器Iterator

他是一个可以记住遍历位置的对象,在上次停留的位置继续运行

相当于生活中的叫号器,一个一个叫号,**全部叫完就停止了**

┌iter():获取可迭代对象的迭代器

└next():一个个去取元素,取完元素后引发一个异常StopIteration

**迭代器依次取值用next,后面的生成器也是**

```python
#方法一:
list1 = [1,2,3,4,5]
iter1 = iter(list1)   #现在iter1就是一个迭代器
print(next(iter1))  #1
print(next(iter1))  #2
print(next(iter1))  #3
print(next(iter1))  #4
print(next(iter1))  #5
print(next(iter1))  #报错

#方法二:
list1 = [1,2,3,4,5]
iter1 = list1.__iter__()
print(iter1.__next__())
print(iter1.__next__())
print(iter1.__next__())
print(iter1.__next__())
print(iter1.__next__())
print(iter1.__next__())
```

### 生成器(generator)

**1**.生成器(generator):

生成器是一种**特殊的迭代器**，它允许你在迭代过程中**按需生成值**，而不是一次性把所有值都存储在内存中。

- 使用 **yield 关键字**而不是 return,看到yield就知道是生成器
- **惰性求值**（Lazy Evaluation）
- **节省内存**，适合处理大数据流
- 可以保留执行状态:next的工作原理,执行到某个值停下来,下次执行会接着,不会重新开始

**2.**yield的作用：

1.类似return，将指定值或者多个值返回给调用者   yield  参数

2.yield语句一次返回一个结果，在每个结果中间，挂起函数，执行next()，再重新从挂起点继续往下执行

3.生成器执行到 yield 时会暂停，返回当前值，并保存所有局部变量状态。下次调用 next() 时，从暂停处继续执行。

**3.**创建生成器的方法

1.生成器推导式

列表推导式,然后[]变成()就行了

for  in  if   ,前面写个式子

```python
#语法:
gen = (i*5 for i in range(5))   #类似于列表推导式,把外边的[]改成(),注意这不是元组推导式,元组没有推导式(元组不能修改)
#取值:
print(gen)
print(next(gen))
print(next(gen))
print(next(gen))
print(next(gen))
print(next(gen))
print(next(gen))
```

2.生成器函数

```python
#生成器函数1
def count_up_to(n):
    i = 0
    while i < n:
        yield i  #使用yield,便为生成器函数
        i += 1

# 使用生成器
gen = count_up_to(5)  #gen就成了一个生成器

#打印内容
print(next(gen))   #0
print("突然出现")
#或者
for num in gen:
    print(num)  # 输出: 0, 1, 2, 3, 4
    
#生成器函数2
def x():
    for i  in range(1,100):
        if i % 2 == 0:
            yield i   #如果是return,一次就结束了,gen就是2
gen = x()    
while True:
    print(next(gen))
```

结果:

```python
0
突然出现
1
2
3
4
```

**4**.yield和return的区别

想要获得内容里的多个值就可以用yield

![图片](Python高级笔记_assets/ch4_img01.png)

代码:

**普通取多个函数和生成器的区别**

```python
#1-普通函数
def get_numbers_return():
    result = []
    for i in range(5):
        result.append(i)
    return result  # 一次性返回 [0,1,2,3,4]
print(get_numbers_return())

#调用
get_numbers_return()

#2-生成器函数
def get_numbers_yield():
    for i in range(5):
        yield i  # 每次返回一个值，暂停等待下次调用

#调用
gen = get_numbers_yield()
print(next(gen))
print(next(gen))
print(next(gen))
print(next(gen))
print(next(gen))
```

结果

```python
[0, 1, 2, 3, 4]
0
1
2
3
4
```

**5.**生成器的方法

1. next() - 获取下一个值,也相当于启动生成器

2. send(value) - 向生成器发送值

3. throw() - 向生成器抛出异常

4. close() - 关闭生成器

- **动作B（入口）**：`send(10)` 把 `10` 塞进生成器，赋值给 `value`。**此时 **`value = 10`。
- 继续往下执行：`if value is None?` 不是，跳过。`total += value` → `total = 0 + 10 = 10`。
- 回到 `while` 循环开头，再次碰到 `yield total`。
- **动作A（出口）**：把当前的 `total`（现在是 `10`）扔出去。
- **暂停**，等待下一次 send。
- 外面的 `print(...)` 接收到扔出来的 `10`，所以打印 `10`。
- print(gen.send(20)),print(gen.send(30))同理

```python
def calculator():
    total = 0                #total初始值为0
    while True:              #无限循环
        value = yield total  #yield total 是“先把 total 扔出去给外面”，然后“等外面用 send 扔进来一个值，再把这个值赋给 value”。
                             #每次send发送过来的值都会赋值给value
        if value is None:    #给结束条件
            break            #退出无限循环
        total += value       #计算累加值

gen = calculator()               #创建生成器对象gen
next(gen)                        #启动生成器
print(gen.send(10))  # 输出: 10  #使用send把10赋值给value
                                 #注意yield是生成器,一次接收一个值并暂停当前状态,也就是如果我们不再调用gen对象,则total永远都是10
print(gen.send(20))  # 输出: 30  #这里send发送了个20给到value,通过total = total + value,上次total是10,则本次结果total变成30,最后通过yield输出30 
print(gen.send(30))  # 输出: 60  #同理,输出的一直都是yield total,所以结果是60
gen.close()  # 关闭生成器
print(gen.send(40)) # 报错:StopIteration #因为生成器已经关闭
```

<u>***yield***</u><u>***注意:***</u>

**yield 这个关键字很特殊，它既能向外输出数据，又能向内接收数据，就像一扇双向门**：

- 向左（向内接收）：yield total 这个整体表达式接收外部 send 进来的值 --- 入口
- 向右（向外输出）：yield total 把 total 的值返回给外部调用者 --- 出口

**6.**迭代对象,迭代器,生成器的关系

![图片](Python高级笔记_assets/ch4_img02.png)

### 闭包

代码分析：

如果我们想**0t**是行不通的

系统会为函数内部重新分配一个变量

```python
total = 0 
def sum_total(*args):
    total = 10  #这两个变量不一样
    for num in args:
        total += num

sum_total(1, 2, 3, 4, 5)
print(total)   #0
sum_total(1, 2, 3, 4, 5)
print(total)   #0
```

我们可以使用global修饰为全局变量这样就可以修改外部参数

这里只是演示分析,现实中全局变量一般不允许修改,只用来查询和使用

闭包主要是为了考虑内外层函数变量,有点像nonocal

```python
total = 0
def sum_total(*args):
    global total   #调用全局变量并修改
    for num in args:
        total += num

sum_total(1, 2, 3, 4, 5)
print(total)  #15
sum_total(1, 2, 3, 4, 5)
print(total)  #30
```

但是以上代码，为了能在外部使用函数内部计算的结果，提升了变量的作用域，但是该做法存在安全风险，也就是全局变量存在被手动修改的风险，如何解决？

在函数嵌套的前提下，内部函数使用了外部函数的变量，并且外部函数返回了内部函数，我们把这个使用了外部变量的内部函数叫做闭包,是一种思想

闭包的概念

必须满足以下 **3 个硬性条件**：

1. **函数嵌套**：存在一个外部函数（外层）和一个内部函数（内层）。
1. **变量捕获**：内部函数引用了外部函数的局部变量（非全局变量）。
1. **返回函数**：外部函数将内部函数作为返回值返回。

<u>***闭包三要素:外嵌内,内用外,外返内***</u>

```python
def parent_fn():
    message = 'hello'
    def inner_fn():  #嵌套函数
        print(message)  #内层函数使用外层变量,这里只是调用,不需要用nonlocal
    # 返回内层函数
    return inner_fn
print(parent_fn())   #直接打印函数会返回函数的地址
result = parent_fn()
result()
#这里虽然执行完了parent_fn后这个函数就会在内存中自动销毁,但是由于我们使用了闭包
#返回的inner_fn会保留message这个变量也就是会记住'hello'
#可以使用result()调用内部函数来输出这个hello
```

使用闭包，修改开始的案例

```python
def sum_total():  #外函数
    total = 0
    def inner(*args):  #内函数,接收不定长变量
        nonlocal total  # 需要修改外部函数参数时 需要添加nonlocal关键词进行修饰
        for arg in args:
            total += arg  #使用外函数的变量
        return total
    return inner  #返回内函数


fn_inner = sum_total()
print(fn_inner(1,2,3))   #结果6 --- taoal变成了6,依旧存活在内存中
print(fn_inner(4,5,6))   #结果21 --- 那我想,这个闭包改变的就是外函数的变量,并且延长该变量存活周期
```

内函数也叫闭包

**优点**

- 无需定义全局变量即可实现通过函数，持续的访问、修改某个值
- 闭包使用的变量的作用域在函数内，难以被错误的调用修改
- <u>***延续外函数变量生命周期,使下次调用函数的时候,变量保持和上次的结束时相同,延长变量生命周期***</u>

**缺点**

- 由于内部函数持续引用外部函数的值,所以会导致这一部分内存空间不被释放,一直占用内存

**闭包和普通函数对比:**

![图片](Python高级笔记_assets/ch4_img03.png)

---

**闭包练习:**

```python
1.编写一个函数 create_counter，它返回一个闭包函数 counter。
每次调用 counter 时，它会返回一个递增的整数序列，但这个序列的起始值
可以通过参数动态设置。
同时，提供一个方法来重置计数器


 def create_counter(start=0):
    num = start

    def counter():
        nonlocal num
        return num

    def reset():  #重置函数
        nonlocal num
        num = start

    # 将重置函数直接绑定在counter函数上一并返回
    #这是一个属性绑定的知识,把reset当作一个属性(功能)绑定在counter函数上,这个属性(功能)的返回值是reset(=号后面的)
    counter.abcd = reset  #这样用想重置的时候,就调用counter的这个属性(abcd),名字随便取
    return counter  #有counter的功能,又有reset的属性
    #正常使用就是counter,需要用reset的时候就当调用counter的abcd属性即可
     
 调用:
 counters = create_counter(10)
 print(counters())
 print(counters())
 print(counters())
 pass
 pass
 pass
 print(counters())
 counters.abcd()   #重置
 print(counters())   

```

![图片](Python高级笔记_assets/ch4_img04.png)

```python
2.编写一个函数
create_cached_function(创建缓存池功能) 接收一个函数 func 作为参数。

返回的闭包函数 cached_func 接收任意数量的参数。

cached_func 使用一个字典来缓存结果，其中键是参数的元组，值是函数 func 的结果。

如果 cached_func 被调用时，传入的参数已经在缓存中，则直接返回缓存的结果。

如果 cached_func 被调用时，传入的参数不在缓存中，则计算结果并将其存入缓存

 def create_cached_function(func):
    cache = {}  #想象中的缓存池
    def cached_func(*args, **kwargs):#这里为什么要写参数,因为这里是唯一的参数入口,只有这里能加参数,而且这里的参数要和下面的func函数里面的一样
                                    #这里接受到的*args会以一个元组的形式存储
                                    #为什么使用*args:由于不确定这个func函数的参数有几个
        nonlocal cache
        key = tuple(args)#这里是防御性代码,保证这个args是元组,虽然*args传进来就已经是元组了,这是为了保险
                         #开发过程中习惯性地确保键是可哈希类型
                         #在所有需要字典键的地方都用 tuple() 转换
        if key in cache:
            print("该缓存存在")
            return cache[key]
        else:
            print("开始缓存")
            cache[key] = func(*args, **kwargs)#接收的小函数
            return "成功缓存"
    return cached_func

#懂了,这里要弄一个新函数啊,简单点就行,因为要接收一个函数作为返回值
def max_num(*args):  #func来了
    return max(args)

cnm = create_cached_function(max_num)   #其实这题有点像装饰器了
print(cnm(2,4,21,4,1))
print(cnm(56,3,1,32,5))
print(cnm(1,2,3,4,5,6))
print(cnm(1,2,3,4,5,6))  #这次运行,上面已经运行过了,所以已经存在了

#再弄个小函数玩玩
def add(n1, n2):
    return n1 + n2

nmb = create_cached_function(add)
print(nmb(1, 2))
print(nmb(2, 2))
print(nmb(1, 2))


```

### 装饰器

装饰器本质也是一种闭包，其功能就是在**不破坏目标函数原有的代码和功能的前提下，为目标函数增加新功能**

比直接修改原函数更加安全

装饰器一般只是调用函数,并不修改变量

下面是直接修改原函数

```python
'''
  study函数可以输出 '正在上课中...'
  现在需要添加上下课的提醒功能，可以通过一下方式实现：
  1. 调用函数之间写输出语句实现
  2. 装饰器
'''
#原函数
def study():
    import  time
    print('正在上课中...')
    time.sleep(5)
    
#1.添加功能:直接修改原函数
def study():
    import  time
    print('上课时间到了...')
    print('正在上课中...')
    time.sleep(5)
    print('下课时间到了...')
study()    
#这样我们就能增加两个新功能进去,但是在项目中我们绝大多数情况都是不好或者不能修改原函数的内容与调用方式的,我们就需要使用到装饰器
```

**装饰器普通用法：**

理解成什么呢,上午说的闭包,外函数里面的参数变成了一个函数,这个函数就是原函数

```python
#装饰器其实就是闭包,我们把原函数当做参数输入到study_pro这个新函数中,在内函数中调用这个原函数
#这时我们就可以在这个内函数中加入我们需要新增的内容
#原函数
def study():
    import  time
    print('正在上课中...')
    time.sleep(5)

#装饰器:把上下课提醒装饰进去,后面我们调用新的这个函数就能够既包含原函数也包括新功能
def  study_pro(fuc):  括号里面是被装饰的原函数
     def inner():  #增加的新功能以及原函数
         print('上课时间到了...')
         fuc()   #执行被装饰的函数
         print('下课时间到了...')
     return inner

fnc = study_pro(study)
fnc()
```

**装饰器注解(语法糖)：**

结果和上面一样的

```python
def  study_pro(fn):
     def inner():
         print('上课时间到了...')
         fn() 
         print('下课时间到了...')
     return inner
#@这个叫装饰器语法糖,是要加的功能函数,下面写的函数才是被装饰的,是原函数(原功能)
@study_pro  #注解,把study当fn粘过去了
def study():  #study = study_pro(study)
    import  time
    print('正在上课中...')
    time.sleep(5)
    
study()  #实际上就是执行study_pro(study)
```

再举个例子

```python
def outer(func):  #这个outer函数就是装饰器
    def x():
        # 在执行原函数前触发
        print("准备开始")
        func()
        # 在执行原函数后触发
        print("执行结束")
    return x

@outer  #send_wechat = outer(send_wechat)
def send_wechat():
    print("微信")

@outer  
def send_email():
    print("邮件")

@outer  
def send_sms():
    print("短信")


send_wechat()  #执行send_wechat(),实际上就是执行outer(send_wechat)
```

**传递多个参数的通用装饰器(原函数需要传参且数量不固定)**

```python
def decorator(func):
    def inner(*args, **kwargs):#这里为什么要写参数,因为这里是唯一的参数入口,只有这里能加参数,而且这里的参数要和里面的func函数里面的一样
        print('--------增加的功能---------')
        total = func(*args, **kwargs)  #*args接收的是元组,一个一个的,**kwargs是字典形式,
        x = [i**2 for i in args]  #****由于闭包的原理,里面的函数也可以直接调用外函数的参数****
        return f"新结果是{x},原返回值是{total}"

    return inner

@decorator
def add_sum1(a,b):#这里参数想写多少写多少
    return a+b
@decorator   
def add_sum2(a,b,c):
    return a+b+c
    
#调用:
print(add_sum1(1,2))
print(add_sum2(1,2,3))
```

再举个例子

```python
#装饰器的参数/带参数的修饰器
#为了防止参数位置不够,用*args和**kwargs接收
def outer(func):
    def x(*args, **kwargs):
        # 在执行原函数前触发
        print("准备开始")
        func(*args, **kwargs)  # 被装饰的原函数以及需要传参
        # 在执行原函数后触发
        print("执行结束")

    return x

@outer
def send_wechat(body):#一个参数
    print("微信", body)

@outer
def send_email(to, body):#两个参数
    print("邮件", to, body)

@outer
def send_sms(to, body, info):#三个参数
    print("短信", to, body, info)

if __name__ == '__main__':
    send_wechat("你好")
    send_email("wupeqi@live.com", "hello")
    send_sms("18888888888", "hello","bybe")
```

**下面是最完整的装饰器的结构**

如果最终输出返回值是none,可能是装饰器函数里的原函数,没有用一个值接收,并在这个函数里返回这个值

```python
def outer(func):  # usage
    def x(*args, **kwargs):
        # 在执行原函数前触发
        # print("准备开始")
        value = func(*args, **kwargs)  # 被装饰的原函数,用一个值接受
        # 在执行原函数后触发
        # print("执行结束")
        return value  #返回这个值
    return x

@outer  # send_wechat = outer(send_wechat)  # usage
def send_wechat(body):
    print("微信", body)
    return 100

if __name__ == '__main__':
    res = send_wechat("你好")
    print(res)
```

多个装饰器

多个装饰器装饰时，是一个由内而外的过程，先执行内部装饰器，后执行外部装饰器

```python
def make_i(func):
    def inner():
        result = "<i>" + func() + "</i>"
        return result
    return inner

def make_b(func):
    def inner():
        result = "<b>" + func() + "</b>"
        return result
    return inner

@make_b  #最外面,最后执行
@make_i  #靠近的是内部装饰器
def content():  #相当于  make_i(make_b(content()))
    return '风雨送春归，飞雪迎春到'
    
#调用:
print(content())
```

结果

```python
<b><i>风雨送春归，飞雪迎春到</i></b>
```

带参数装饰器

使用装饰器的时候加上参数,给语法糖后面加个参数

注意事项:用于想再引入一个参数进入装饰器的时候

需要在最外层再包一层函数,用来接收装饰器的参数

如果把装饰器看作一次闭包,其实使用带参数的装饰器就是相当于两次闭包

```python
def return_decorator(flag): #最外层是为了获取参数,装饰器只能接受函数,所以需要专门写个函数来接收参数(也要用闭包才能传进去),这里的参数就叫flag
    def decorator(func):       #这里才是装饰器开始的地方,由于没有对外层参数重新赋值所以可以不用nonlocal直接使用flag
        def inner(a,b):        #这里是实际发生新增功能的地方
            if flag == '+':    #使用新参数,很秒啊
                print('----------正在努力执行加法运算----------') 
                return func(a,b)
            elif flag == '-':
                print('----------正在努力执行减法运算----------')
                return func(a,b)
        return inner  #正常返回的闭包函数
    return decorator  #返回装饰器的最外层函数

#原函数
#这样使用装饰器的时候,被装饰函数正常写,语法糖用最外层函数,同时括号内加上装饰器的参数
@return_decorator('+')
def add_num(a, b):
    return a + b

#原函数,不用的装饰器参数就写一个新的
@return_decorator('-')
def sub_num(a, b):
    return a - b

print(add_num(2, 3))
print(sub_num(2, 3))
```

类装饰器(用的较少,了解一下)

这他喵是个啥

```python
class MyDecorator:
    def __init__(self, func):
        self.__func = func

    def __call__(self, *args, **kwargs):
        print('-----------毛泽东诗词-------------')
        result = self.__func(*args, **kwargs)
        print(result)
@MyDecorator
def fn():
    return  '已是悬崖百丈冰，犹有花枝俏'
fn()
```

**练习**

**1.用装饰器的方式统计函数执行的时间**

```python
import time

def timer(func):
    def wrapper():  #wrapper:包装纸
        start = time.time()  # 开始时间,time.time(),返回时间
        result = func()
        end = time.time()    # 结束时间
        print(f"{func.__name__} 执行耗时: {end - start:.4f}秒")  #func.__name__可以返回函数名
        return result
    return wrapper

# 使用
@timer
def funa():
    return sum(range(100000))
@timer
def funb():
    return sum(range(1000000))

print(funa())
print(funb())
```

**2.创建一个名为 log\_it 的装饰器，它会在被装饰的函数执行前打印一条消息，内容为 "正在执行函数：[函数名]"，并在函数执行后打印 "函数 [函数名] 执行完成"**

```python
from functools import wraps
#这里的wraps是比较重要的装饰器,他能够帮助我们的装饰器函数获取原函数信息
#@wraps 解决的问题是：当装饰器返回的函数（inner）被当作原函数使用时，它的元信息（__name__、__doc__ 等）会覆盖原函数的信息。
#也就是说如果我没有使用wraps装饰器则执行print(add_nums.__name__)会返回inner,因为现在add_nums已经被装饰器中的inner替换了
#如果使用了wraps则执行print(add_nums.__name__)会返回add_nums,因为他能保留原函数的信息

def log_it(func):
    @wraps(func)  #获取func的信息,比如变量什么的
    def wrapper(*args, **kwargs):
        func_name = func.__name__   #获得函数名
        print(f'正在执行函数：[{func_name}]')
        result = func(*args, **kwargs)
        print(result)
        print(f'函数 [{func_name}] 执行完成')
        return result
    return wrapper



@log_it
def add_sum(a,b):
    return a+b
```

**3.权限检查装饰器**

要求: 编写一个带参数的装饰器 requires\_access\_level(access\_level)，其中 access\_level 是一个整数。如果用户权限不足，则不执行被装饰的函数，并打印错误信息 "权限不足"

这种要再在内部进行条件判断的,可以用带参数的装饰器

```python
from functools import wraps

user_level = 6        #这里先写死,假设用户等级就是6,实际开发中会从用户的登录信息中提取等级标签(web端数据包)

def requires_access_level(level):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            if user_level >= level:
                result = func(*args, **kwargs)
                print("可以访问")
                return result
            else:
                raise PermissionError('You are not allowed to access this function')
        return wrapper
    return decorator


@requires_access_level(5)
def add(a, b):
    return a + b
    
print(add(1, 2))
```

**4.重试机制装饰器**

要求: 创建一个带参数的装饰器 retry\_on\_failure(attempts)，其中 attempts 参数指定了尝试执行被装饰函数出现异常时可以重试的最大次数

```python
def retry_on_failure(attempts):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for i in range(attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    print(f'尝试 {i + 1}/{attempts} 失败: {e}')
            else:
                print(f"所有尝试均失败")
        return wrapper
    return decorator

@retry_on_failure(attempts=5)
def add(a, b):
    if a > b:
        return a + b
    else:
        raise ValueError('参数错误')

print(add(1, 2))
```

1. 核心定义（先记骨架）

- **闭包（Closure）**：在一个**内部函数**中，引用了**外部函数的变量**，并且外部函数返回了这个内部函数。此时，这个内部函数连同它引用的外部变量，就构成了一个“闭包”。   主要目的:在不修改外部变量的前提下调用并修改外部变量,而且调用后还存活在内存中
- **装饰器（Decorator）**：本质上是一个**接收函数作为参数**、**返回新函数**的可调用对象（通常是函数或类）。它的核心作用是**“在不修改原函数代码的前提下，给原函数增加额外功能”**。  主要目的:给函数增加功能,并不修改参数和原函数

---

2. 两者的本质区别（关键）

| 维度 | 闭包 | 装饰器 |
| --- | --- | --- |
| **关注点** | 关注<br>**数据的持久化**<br>（让外部函数的变量在返回后不被销毁） | 关注<br>**功能的增强**<br>（给现有函数包装新逻辑） |
| **参数** | 内部函数通常接收普通业务参数 | 装饰器接收的是<br>**被装饰的函数对象** |
| **返回值** | 返回一个<br>**函数**<br>（通常是为了后续调用） | 返回一个<br>**函数**<br>（通常是原函数的增强版） |
| **关系** | **手段**<br>（实现工具） | **目的**<br>（应用场景） |

打个生活比方：

- **闭包**像是**“带记忆的遥控器”**——按键（内部函数）记住了它属于哪台电视（外部变量），,下次换台,是从新台开始,即使电视不在了，按键依然知道该控制谁。
- **装饰器**像是**“手机贴膜”**——它不改变手机（原函数）的硬件，但给手机增加了防摔（日志）、防蓝光（计时）等功能。

---

### 设计模式

是一种程序设计思想,方法,不是具体的代码

设计模式是一种编程套路，可以极大的方便程序的开发，最常见、最经典的设计模式，就是我们所学习的面向对象了。除了面向对象外,在编程中也有很多既定的套路可以方便开发,我们称之为设计模式:

- 单例模式
- 工厂模式

#### 单例模式

单例模式(Singleton Pattern)是一种常用的软件设计模式,**单例模式的根本目的就是要让一个类只能制造出一个对象**,单例的需求一**般是针对需要共享数据的对象**

**就像电脑的回收站,只能打开一个回收站**

- 工厂对象
- 数据库连接池对象
- 任何其他想要所有人共享的对象

##### 方式一:使用装饰器实现单例模式:

通过闭包和字典缓存来确保一个类只有一个实例

如果给类加了装饰器语法糖,那么实例化不同的对象,这几个对象的内存地址是一样的,也就是说实际上只有一个实例,

只有当你使用的装饰器内部**主动拦截了实例化过程**（比如重写了`__call__`或`__new__`），才会强制返回同一个缓存实例。这种装饰器是**刻意**写出来实现“单例模式”的。

装饰器实际接受的时Dof类,而不是实例对象,这样;两个实例对象实际上是一样的,实现了单例模式

```python
#相当于在默认的内存分配函数上增加了判断如果没有创建实例则新建否则不做任何操作
def singleton(cls):  #cls是参数,这个参数是类,比如下面的Dog类,把Dog类传进去
    instances = {}  #实例缓存字典
    def get_instance(*args, **kwargs):
        if cls not in instances:  #查看实例对象是否在字典中,不在就加入
            instances[cls] = cls(*args, **kwargs) #cls(*args,**kwargs)固定搭配,调用类创建实例，返回的是实例对象的内存地址
        return instances[cls]   #返回缓存字典

    return get_instance  #返回内层函数,实现闭包

@singleton        
class Dog:
    pass

dog1 = Dog()#实例对象,同时使用了装饰器,加入缓存字典
dog2 = Dog()#同理
print(dog1,dog2) #两个一样的.因为装饰器实际接受的时Dof类,而不是实例对象,这样;两个实例对象实际上是一样的,实现了单例模式
print(id(dog1), id(dog2))  #两个一样的,同一个内存地址
print(dog1 is dog2)  #True,同一个对象,实现了单例模式,一个类只能创建一个对象

#不加语法糖,不用装饰器
calss Cat: 
    pass

cat1 = Cat()
cat2 = Cat()
print(id(cat1), id(cat2)) #这样内存地址栏不一样了,不是单例模式了
```

##### 方式二:使用导入模块实现单例(在模块或.py文件导入时,会生成.pyc文件(在__pycache文件及里面),不论导入多少次都是共用同一个内存空间的)

**模块是天生的单例模式,**只生成一个pyc文件,相对牵强

```python
#创建singleton_module.py文件
class Singleton:
    def __init__(self):
        self.value = "I am a singleton instance"

singleton_instance = Singleton()
```

导入实例对象

```python
#在新的文件里面导入singleton_module.py文件中的实例对象  
from singleton_module import singleton_instance as s01
from singleton_module import singleton_instance as s02
print(s01,"\n",s02)  #两个一样的,实现了单例模式,一个类只能创建一个对象
```

##### 方式三:__new__实现单例模式:最主要模式

\_\_new\_是Python中一个特殊方法，是object提供的**静态方法(staticmethod)**,**形参必须传递cls****,**类方法还记得吗,也是cls(但是是classmethod)

**它负责创建实例对象**，在\_init\_之前被调用。可以理解\_new\_是掌握Python对象生命周期和元编程的关键。

![图片](Python高级笔记_assets/ch4_img05.png)

```python
#这段代码只是介绍__new__的使用方法,而不是使用单例模式
class Person:
    def __new__(cls, name, age):  # __new__是固定搭配,这里的参数要和__init__一一对应
        print(f"1. __new__ 被调用，创建实例")
        instance = super().__new__(cls)  # 固定写法,真正创建实例化对象,这里super表示object
        return instance  # 必须返回实例,否则没有容器接受实例对象
    def __init__(self, name, age):
        print(f"2. __init__ 被调用，初始化实例")
        self.name = name
        self.age = age
p = Person("张三", 25)  # p就相当于instance这个实例
print(p.name)
# 输出:
# 1. __new__ 被调用，创建实例
# 2. __init__ 被调用，初始化实例
# 3. 张三
```

重写\_\_new\_\_实现单例模式:

这里\_\_new\_\_就像这样写死即可(直接复制都行),这样这个类就是单例模式

```python
class Singleton:
    _instance = None  #私有变量,但是是单下划线仅隐藏,没啥用
    
     #默认情况下__new__创建实例是系统自动完成的,这里为了单例模式才手动指定唯一
    def __new__(cls, *args, **kwargs):  #保险起见,写成这方式,这样不管类里面写多少参数都接的下
        if cls._instance is None:     #判断当前类是否创建了实例
            print("创建第一个实例")
            cls._instance = super().__new__(cls) #使用super调用父类进行内存分配创建当前类的实例
        else:
            print("返回已存在的实例")
        return cls._instance  #把创建好的实例返回
    
    def __init__(self, value):
        self.value = value

# 测试
s1 = Singleton("第一次")
s2 = Singleton("第二次")
print(s1 is s2)  # True，同一个对象,实现了单例模式,一个类只能创建一个对象
print(s1.value)  # 第一次
print(s2.value)  # 第一次（第二次的赋值没有生效，因为__init__被再次调用）
```

结果

```python
创建第一个实例
返回已存在的实例
True
第二次
第二次
```

单例模式的应用场景:

1.回收站

2.音乐播放器，一个音乐播放软件负责音乐播放的对象只有一个

3.开发游戏软件(场景,终端管理)

4.数据库配置、数据库连接池的设计,多个用户,但是只有一个连接池

#### 工厂模式

简单工厂模式：不直接向用户直接暴露对象的创建细节，而是通过一个工厂类创建对象实例

> 💡 ##### 为什么要学习工厂模式？
>
> 在面向对象编程中，对象的创建（使用 `new` 或直接实例化）与对象的使用往往是耦合在一起的。当业务复杂时，这种耦合会导致：
>
> - 代码重复：多处地方需要创建同一类对象
> - 难以维护：如果创建逻辑发生变化，需要修改所有实例化的地方
> - 难以扩展：新增产品类型时需要修改所有调用方
>
> **工厂模式的核心思想**：将“创建对象”的职责从“使用对象”的代码中剥离出来，统一由工厂负责。即：**让创建和使用分离**。
>
> ##### 工厂模式的三大分类
>
> | 模式 | 特点 | 复杂程度 |
> | --- | --- | --- |
> | **简单工厂模式** | 一个工厂类根据参数决定创建哪种产品 | ⭐ |
> | **工厂方法模式** | 每个产品对应一个工厂类，由子类决定具体创建哪个产品 | ⭐⭐ |
> | **抽象工厂模式** | 一个工厂可以创建一系列相关的产品（产品族） | ⭐⭐⭐ |
>
> > 本课件重点讲解 **简单工厂模式** 和 **工厂方法模式**。

##### 简单工厂模式

**工厂模式**的核心思想就是：**将“创建对象”的逻辑集中到一个地方（工厂），客户端只需要告诉工厂“我要什么”，工厂负责生产出来。**

代码:

```python
from abc import ABC, abstractmethod

"""
【知识点回顾】ABC（Abstract Base Class）是 Python 内置的抽象基类模块。
@abstractmethod 装饰器用于声明抽象方法，子类必须实现该方法才能实例化。
这里的 Payment 就是"抽象产品角色"——定义了所有具体产品必须遵守的接口规范。

案例:支付方式选择
┌支付宝┌余额
│     └花呗
└微信支付┌余额
        └银行卡
"""

class Payment(ABC): #抽象类
    """抽象产品角色：定义支付行为的统一接口"""
    @abstractmethod  #用这个装饰器,声明的为抽象方法
    def pay(self, money: float) -> None:  # 抽象类声明这个pay方法,则继承抽象类的子类一定要有这个pay方法,这就是抽象类
        """支付方法，所有具体支付类必须实现"""
        pass


class AliPay(Payment):  #继承抽象类
    """具体产品角色1：支付宝支付"""
    def __init__(self, use_huabei: bool = False):  #默认不适用花呗
        # 【知识点】双下划线 __ 开头的属性是私有属性，外部无法直接访问
        self.__use_huabei = use_huabei
        self._pay_method = "花呗" if use_huabei else "余额/信用卡"  #这属性不用传的,一种设计思路
        
    def pay(self, money: float) -> None:  #一定要有pay方法否则报错
        pay_way = "支付宝花呗" if self.__use_huabei else "支付宝余额"
        print(f'{pay_way}支付{money}元')
        
    # 【知识点】提供 getter 方法让外部获取私有属性  
    #主要是为了防止调用者不知道使用哪种方式支付的,使用者肯定知道用的哪种方式支付的,这方法真弱智
    def get_pay_method(self) -> str:  #约束这个函数的返回值是str类型(注解而已)
        return self._pay_method


class WechatPay(Payment):
    """具体产品角色2：微信支付"""
    
    def pay(self, money: float) -> None:  #写抽象类
        print(f'微信支付{money}元')


# ============================================================
# 第一部分：简单工厂模式
# ============================================================

class PaymentFactory:  #创建工厂类
    """
    简单工厂（Simple Factory）
    
    【职责】负责根据传入的参数，创建并返回对应的支付对象。
    【优点】调用方不需要关心对象是如何创建的，只需要告诉工厂"我要什么"。
    【缺点】如果新增一种支付方式（如 ApplePay），需要修改这个类的代码，
          违反了"开闭原则"中的"对修改封闭"原则。
    """
    #这里我们不能使用构造方法来获取method,如果使用构造方法那么这个工厂就只能生成一种支付方式了
    #例如:factory1 = PaymentFactory("AliPay"),那么这个factory1就只能使用支付宝支付了
    def create_payment(self, method: str) -> Payment:   #就相当于给前面的类做一个整合,像在一个工厂一样,就是工厂模式
        if method == 'alipay':
            return AliPay()  # 默认不使用花呗
        elif method == 'wechat':
            return WechatPay()
        elif method == 'huabei':
            return AliPay(use_huabei=True)  # 使用花呗模式
        else:
            # 【知识点】主动抛出异常，防止调用方使用不支持的支付方式
            raise TypeError(f'不支持的支付方式: {method}')


# ============================================================
# 测试代码
# ============================================================

if __name__ == '__main__':  #这个写不写没关系
    # 【知识点】客户端代码只与工厂交互，不直接实例化具体产品类
    factory = PaymentFactory()  #

#客户端不需要知道 AliPay 或 WechatPay 的存在，只需要告诉工厂 'alipay' 或 'wechat'。
#如果以后支付方式变了（比如支付宝接口改名），只需要修改工厂内部代码，客户端完全不受影响。
    # 使用支付宝支付
    payment_1 = factory.create_payment('alipay')
    payment_1.pay(100)  # 输出：支付宝余额支付100元
    #等同于 alipay1 = AliPay()
    
    # 使用微信支付
    payment_2 = factory.create_payment('wechat')
    payment_2.pay(200)  # 输出：微信支付200元
    
    # 使用花呗支付（本质是支付宝，但开启了花呗功能）
    payment_3 = factory.create_payment('huabei')
    payment_3.pay(300)  # 输出：支付宝花呗支付300元
    
    #如果不使用工厂模式就需要每个类都实例
    # 每次使用都要自己 new 对象
    #alipay1 = AliPay()
    #alipay2 = AliPay()
    #wechat1 = WechatPay()
```

> 💡 ##### 简单工厂模式的优缺点分析
>
> ###### ✅ 优势
>
> 1. **统一入口**：大批量创建对象时有统一的入口（工厂类），便于代码维护
> 1. **隔离变化**：当产品创建逻辑发生变化时，仅需修改工厂类的创建方法，调用方无需改动
> 1. **符合现实世界模型**：就像去餐厅点餐，你只需要告诉服务员（工厂）你要什么菜（产品），不需要自己进厨房炒菜
> 1. **隐藏实现细节**：调用方不需要知道 `AliPay(use_huabei=True)` 这样的构造细节
>
> ###### ❌ 局限性（痛点）
>
> 1. **违反开闭原则(闭包)**：如果要新增一种支付方式（如 ApplePay），必须修改 `PaymentFactory` 类的内部代码，这违反了"对修改封闭"的原则
> 1. **工厂类职责过重**：所有产品的创建逻辑都集中在一个类中，随着产品增多，这个类会越来越臃肿(支付方式越多,也越臃肿)
> 1. **扩展困难**：如果不同产品的构造参数差异很大，工厂方法的参数列表会变得非常复杂
>
> > 💡 **什么时候使用简单工厂？**
> >
> > - 需要创建的产品种类较少（如 3-5 种）
> > - 产品创建逻辑相对稳定，不会频繁变更
> > - 客户端只需要知道产品类型，不需要知道创建细节

##### 工厂方法模式

### 进阶：工厂方法模式（Factory Method）

为了解决简单工厂"违反开闭原则"的问题，工厂方法模式将工厂类也抽象化——\*\*每个具体产品都对应一个具体的工厂类\*\*。

```python
#### 代码实现
from abc import ABC, abstractmethod

# ---------- 产品层（与之前相同） ----------
#准备工厂的内容
class Payment(ABC):
    @abstractmethod
    def pay(self, money: float) -> None:
        pass

class AliPay(Payment):
    def __init__(self, use_huabei: bool = False):
        self._use_huabei = use_huabei
    def pay(self, money: float) -> None:
        pay_way = "花呗" if self._use_huabei else "支付宝"
        print(f'{pay_way}支付{money}元')

class WechatPay(Payment):
    def pay(self, money: float) -> None:
        print(f'微信支付{money}元')


# ---------- 工厂层：每个产品对应一个工厂 ----------
class PaymentFactory(ABC):
    """抽象工厂角色：声明创建产品的抽象方法"""
    @abstractmethod
    def create_payment(self) -> Payment:
        pass

class AliPayFactory(PaymentFactory):
    """具体工厂角色1：专门生产支付宝产品"""
    def __init__(self, use_huabei: bool = False):
        self._use_huabei = use_huabei
    def create_payment(self) -> Payment:
        return AliPay(use_huabei=self._use_huabei)

class WechatPayFactory(PaymentFactory):
    """具体工厂角色2：专门生产微信支付产品"""
    def create_payment(self) -> Payment:
        return WechatPay()


# ---------- 客户端调用 ----------
if __name__ == '__main__':
    # 【知识点】客户端需要决定使用哪个工厂，但不需要知道产品如何创建
    # 新增 ApplePay 时，只需新增 ApplePay 类和 ApplePayFactory 类，
    # 不需要修改任何已有代码 → 符合开闭原则 ✅
    
    ali_factory = AliPayFactory()
    payment = ali_factory.create_payment()
    payment.pay(100)
    
    wechat_factory = WechatPayFactory()
    payment = wechat_factory.create_payment()
    payment.pay(200)
    
    #什么玩意啊
```

![图片](Python高级笔记_assets/ch4_img06.png)

### 进程&线程&协程

不用这些程序也正常运行,依旧只是一个思想,,用了更好

> 💡 名词解释

**并发：**在一段时间内交替执行多个任务，例如：**在单核的cpu上执行多个任务**，操作系统会轮流执行多个任务，任务的数量大于cpu的核心数,一个锅炸一个鸡排,炸完一个炸另一个

**并行：**在一段时间内同时执行多个任务，例如:   **在多核cpu上执行多个任务**，操作系统会给cpu的每个核分别分配任务，多个任务同时执行,多个锅炸多个鸡排

> 例子:
>
> 假设你是一家火锅店的老板，店里只有**一口锅**，但有**两位客人**（任务A和任务B）。
>
> **并发（Concurrency）**：你只有一口锅，但你想同时服务两位客人。于是你**交替**操作：给A涮一片毛肚（1秒），然后捞起来，转头给B烫一片肥牛（1秒），再回头给A煮个虾滑……虽然**同一时刻**锅底只能煮一种菜，但因为你的切换速度极快，两位客人都感觉自己在“同时”被服务。这就是**并发**——**逻辑上的同时发生**（通过时间片轮转）。
>
> **并行（Parallelism）**：你生意做大了，买了**两口锅**。现在你可以让锅1专门给A煮毛肚，锅2专门给B煮肥牛。这两位客人是**真正在同一时刻**被服务的。这就是**并行**——**物理上的同时发生**（需要多个硬件核心）。

**进程：系统资源的最小分配单位**，通俗理解一个正在运行的程序就是一个进程，比如：QQ，微信

**线程:**程序执行(进程)的最小单位，实际**进程只负责分配资源，实际执行任务的还是线程**，也就是说**进程是线程的容器(进程里面有1到多个线程)**，一个进程中至少有一个线程来执行任务，线程自己不分配资源，它和同一个进程中的其它线程共享进程中的资源

> 例子:
>
> 假设你要开一家**“宇宙级火锅连锁店”**（这就像**一个程序**）。
>
> **进程（Process）**：就是你的**一家分店**。比如“合肥之心城店”和“北京三里屯店”。每家分店都**独立拥有**自己的店面、厨房、收银台、桌椅和员工宿舍（这就像独立的**内存空间、文件句柄等系统资源**）。两家店之间不共享厨房，水电费各自结算，**一家店倒闭了，完全不影响++另一家店营业**。
>
> **线程（Thread）**：就是一家分店里的**多个服务员**。比如“合肥店”里有配菜员、传菜员、收银员。这些服务员**共享**这家店的厨房、食材和收银台（共享**进程内的堆内存和资源**）。但每个服务员有自己的**任务清单**（比如传菜员只负责传菜，这就像线程独立的**栈和程序计数器**）。如果传菜员摔倒了（线程崩溃），可能会影响上菜速度，但厨房还能运转；但如果厨房着火了（进程崩溃），所有服务员都得下班。

##### 线程

> 💡 多线程完成多任务

**为什么使用多线程**

简单来说就是线程比进程需要的资源更少

- 进程是分配资源的最小单位，一旦创建一个进程就会分配一定的资源，就像跟两个人聊QQ就需要打开两个QQ软件一样,比较浪费资源
- 线程是程序执行的最小单位，实际上进程只负责分配资源，而利用这些资源执行程序的是线程，也就说进程是线程的容器，一个进程中最少有一个线程来负责执行程序，同时线程自己不拥有系统资源，只需要一点儿在运行中必不可少的资源，但它可与同属一个进程的其它线程共享进程所拥有的全部资源，这就像通过一个QQ软件（一个进程)打开两个窗口（两个线程）跟两个人聊天一样，实现多任务的同时也节省了资源

---

**多线程的作用**

主线程：在进程中默认提供的线程称之为主线程，用来执行程序

子线程：在进程中手动创建的线程称之为子线程,开启多线程并行

---

**创建线程的步骤**

1.导入线程模块   **import threading**

2.通过线程类创建线程对象  thread1 = threading.Thread(target='任务')   ,这个thread1就是执行这个'任务'的线程

3.开启线程 thread1.start()

---

**说明图示:**

```python
┌─────────────────────────────────────┐
│ 进程（Process） │
│ 操作系统分配资源的最小单位 │
│ 例如：QQ.exe 这个程序运行起来就是一个进程 │
│ │
│ ┌──────────────────────────────┐ │
│ │ 主线程（Main Thread） │ │
│ │ 进程启动时默认创建 │ │
│ │ 负责执行程序入口 │ │
│ └──────────────────────────────┘ │
│ │
│ ┌──────────────────────────────┐ │
│ │ 子线程（Child Thread） │ │
│ │ 手动创建，共享进程资源 │ │
│ │ 可以并发执行多个任务 │ │
│ └──────────────────────────────┘ │
└─────────────────────────────────────┘
```

**创建线程对象语法:**

```python
### threading.Thread 参数详解
import threading
#通过线程类创建线程对象
mythread = threading.Thread(
    target=None,        # 线程要执行的函数名（注意：不带括号）
    args=(),            # 元组方式传递位置参数，如 args=(3, '小苹果')
    kwargs={},          # 字典方式传递关键字参数，如 kwargs={'num':3, 'name':'街舞'}
    daemon=False,       # 是否为守护线程（True=是守护线程,主线程结束子线程立即销毁）
    name=None           # 线程名称，默认格式 "Thread-1",一般不指定
)
```

重要说明：

target 传入的是函数对象，不是函数调用结果。target=sing ✅ target=sing() ❌

args 元组中如果只有一个元素，后面必须加逗号：args=(3,)，否则会被识别为数字本身。

**创建线程代码演示：**

```python
import time
from threading import Thread
def sing():  #需要封装成线程的函数(原函数)
    for i in range(1,6):
        print(f'正在唱歌,第{i}遍')
        time.sleep(1)  #唱一次休眠一个

def dance(): #需要封装成线程的函数
    for i in range(1,6):
        print(f'正在跳舞,第{i}遍')


# 在同一个主线程中运行  按照顺序先后执行
# sing()    #正常执行
# dance()   #正常执行

if __name__ == '__main__': 
    #创建子线程，并发同时执行
    thread_1 = Thread(target=sing)  #手动赋予线程,函数不要加括号
    thread_2 = Thread(target=dance) #这里都不用接收参数
    #都不休眠的话就是执行完一个再执行下一个
    thread_1.start()  # 因为线程,他们如果只有一个休眠会互相争抢资源,dance在sing休眠的时候会运行
    thread_2.start()  # 启动线程,如果两个都休眠相同时间,就是a执行一次,b执行一次
```

---

自定义线程

代码比较复杂时有优势

```python
import threading
class MyThread(threading.Thread):  #继承线程的基类
#这样会自动分配线程编号
    def run(self):
        self.name = '自定义线程名'
        print(threading.current_thread().name)  # 线程名字    
        print(threading.current_thread().ident)  # 线程编号,会自动分配
        print('这是一个自定义线程')
        
    def ... :#新方法

my_thread = MyThread()
my_thread.start()
```

注意:

#这种写法在少代码的情况下比较鸡肋,但是如果代码量比较大则比直接调用函数要强大的多

#如果我们直接使用函数调用:run\_thread =  Thread(target=run),这样我们虽然能进行run函数的执行

#**但是如果我们想要加上别的属性就无从下手了,这种情况就要使用到自定义线程**

#代码如下:

**因为父类 **`threading.Thread`** 的原始 **`start()`** 方法里，有一句隐藏代码“自动调用”了当前对象的 **`run()`** 方法。** 你重写 `start()` 后，里面写了 `super().start()`，就是在“呼叫”父类去执行这个隐藏机制。

![图片](Python高级笔记_assets/ch4_img07.png)

```python
import threading

class MyThread(threading.Thread):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        print('1. 线程对象初始化完成')
    
    def run(self):  
        print('3. 开始执行任务...')
        # 这里是核心任务
        print('4. 任务执行中')
        print('5. 任务完成')
    
    def start(self):
        print('2. 准备启动线程...')
        super().start()  # 调用父类的 start，最终会触发 run()

t = MyThread()
t.start()
```

---

线程如何参数传递

**所有的传参要用args和kwargs属性**

就是Thread语句有属性时args和kwargs用来写参数,原函数该怎么写就怎么写

```python
import time
from threading import Thread

def sing(num,name):
    for i in range(num):
        print('正在唱{}歌{}遍'.format(name,i))
        time.sleep(1)


def dance(num,name):
    for i in range(num):
        print('正在跳{}舞{}遍'.format(name,i))
        time.sleep(1)

if __name__ == '__main__':
    # 创建线程
    sing_thread = Thread(target=sing,args=(3,'小苹果'))  #元组位置一一对应
    dance_thread = Thread(target=dance,kwargs={'num':3,'name':'街舞'})  #字典精准匹配参数

    # 开启线程
    sing_thread.start()
    dance_thread.start()
```

---

设置守护线程

**给某线程设置了守护线程,该线程的生命周期与主线程一致,主线程结束,该线程立马结束,**被强制销毁

非守护线程还没有结束

```markdown
### 守护线程（Daemon Thread）详解

**守护线程 vs 非守护线程：**

| 特性 | 守护线程 (daemon=True) | 非守护线程 (daemon=False，默认) |
|------|------------------------|-------------------------------|
| 主线程结束时 | 立即被强制销毁 | 主线程会等待它执行完毕 |
| 适用场景 | 后台监控、心跳检测、日志收集 | 重要的业务任务必须完成 |
| 资源清理 | 不保证资源正确释放 | 可以正常执行 finally 清理 |

**注意：** `daemon` 属性必须在 `start()` 之前设置，否则会报错。
```

代码如下:

```python
import time
import threading

"""
 守护线程的使用，设置守护线程后,主线程结束后，会直接销毁子线程的执行
 设置守护线程的方式:daemon属性为True
threading.Thread(target=task, daemon=True)
"""
def task():
    for i in range(10):
        print('子线程正在执行任务....')
        time.sleep(0.2)
        
if __name__ == '__main__':  #养成习惯,识别为当前用户,进程线程以后都写个这个
    task_thread = threading.Thread(target=task,daemon=True) #daemon属性必须在 start() 之前设置，否则会报错。
    # 启动线程
    task_thread.start()
    time.sleep(1)
    print('主线程任务执行结束了....')  #主线程结束,子线程立马停止(设置了守护线程)
```

```python
#不设置守护,主线程结束了,子线程还在执行,子线程一共2s,主线程1s
子线程正在执行任务....
子线程正在执行任务....
子线程正在执行任务....
子线程正在执行任务....
子线程正在执行任务....
主线程任务执行结束了....
子线程正在执行任务....
子线程正在执行任务....
子线程正在执行任务....
子线程正在执行任务....
子线程正在执行任务....

#设置守护,1s主线程结束,子线程也结束
子线程正在执行任务....
子线程正在执行任务....
子线程正在执行任务....
子线程正在执行任务....
子线程正在执行任务....
主线程任务执行结束了....
```

---

线程的执行顺序

百万抢占案例

```python
import time
import threading

"""
 多线程的执行顺序: 多个线程在执行时，是没有顺序的，谁拿到cpu的运行时间谁执行
"""
def add():
    global a  #升级全局变量
    for i in range(b):
    a += 1
    print(f"第一次a:{a}")

def add2():
    global a
    for i in range(b):
    a += 1
    # print(f"第二次a:{a}")

# add()  #1000000
# add2() #2000000

if __name__ == '__main__':
    t1 = threading.Thread(target=add)
    t2 = threading.Thread(target=add2)

    t1.start()
    t2.start()
```

```python
第一次a:1368956  #不是1000000,这就体现出来线程同时运行,分别抢占资源
第二次a:2000000  #但总结果还是2000000
```

---

join(),等待当前线程执行完毕在向下执行(阻塞机制)

还是用百万撤离案例

```python
import time
import threading

"""
 多线程的执行顺序: 多个线程在执行时，是没有顺序的，谁拿到cpu的运行时间谁执行
"""
def add():
    global a  #升级全局变量
    for i in range(b):
    a += 1
    print(f"第一次a:{a}")

def add2():
    global a
    for i in range(b):
    a += 1
    # print(f"第二次a:{a}")

# add()  #1000000
# add2() #2000000

if __name__ == '__main__': #创建并启动 200 个线程，每个线程都执行 worker()，然后等待所有线程结束，最后打印 count。
    t1 = threading.Thread(target=add)
    t2 = threading.Thread(target=add2)

    t1.start()
    t1.join()  #t1处开启阻塞,等待t1执行结束才向下执行
    t2.start()
    t2.join()  #其实和直接调用add1,add2是一样的,但是这里是因为函数太少了,以后函数很多,这个join就很有用
```

```python
第一次a:1000000
第二次a:2000000
```

---

线程安全问题

安全问题主要在进程中,这里看看就行

程序执行的结果小于200，有不确定性

根本原因：`temp = count; count = temp + 1`** 不是一个原子操作**，而是由三个独立的步骤组成：

1. **读取（Read）**：从内存中取出 `count` 的当前值存入 `temp`。
1. **计算（Compute）**：`temp + 1`。
1. **写入（Write）**：将计算结果写回 `count`。

**这三个步骤之间可能被其他线程插入执行，导致“丢失更新”**

```python
import threading
import time

""
    #1.非原子操作拆分
    #count = temp + 1  
    #被拆分为三个非原子步骤：
    #读取count值到temp
    #计算temp + 1
    #写回count这三个步骤可能被线程调度打断
    #2.count += 1  # CPython解释器中对小整数是原子操作      


""
count = 0
def worker():
    global count
    temp = count
    time.sleep(0.001)
    count = temp + 1

if __name__ == "__main__":
    threads = []  #线程池
    for i in range(200):
        t = threading.Thread(target=worker)
        threads.append(t)
        t.start()
        #t.join()  #这里阻塞也能正常输出200
    for thread in threads:
        thread.join()

    print(count)
```

程序执行的结果小于200，有不确定性

分析:

| 时间点 | 线程A | 线程B | count值 |
| --- | --- | --- | --- |
| t1 | 读取count=0→temp=0 | - | 0 |
| t2 | 进入sleep | 读取count=0→temp=0 | 0 |
| t3 | 唤醒，写入count=1 | - | 1 |
| t4 | - | 写入count=1 | 1 |

所以这个时候我们就需要使用到线程锁了

**线程锁（Lock）**

为什么需要锁？

- 多个线程同时修改同一个变量时，会出现"数据竞争"（Race Condition）
- 例如 `count += 1` 在底层会拆分为：读取 → 计算 → 写入，这三个步骤可能被其他线程插入

**Lock 的两种用法对比：**

**在函数开始处写个**`with lock:`

| 方式 | 代码示例 | 优缺点 |
| --- | --- | --- |
| 手动加解锁 | `lock.acquire()`<br>/<br>`lock.release()` | 灵活但容易忘记释放导致死锁 |
| with 上下文 | `with lock:` | 自动释放，推荐使用 |

**死锁的典型场景：**

```python
lock.acquire()  # 第一次上锁
lock.acquire()  # 第二次上锁 → 永远阻塞，因为第一次锁还没释放
# 程序卡死在这里
```

线程锁解决安全问题

```python
import threading
import time

count = 0
lock = threading.Lock()  #Lock同步锁,实例锁,才能使用with lock:

def worker_with_lock():
    with lock:  # 上锁!!!锁生效了！
        global count
        temp = count
        time.sleep(0.001)  # 虽然sleep，但锁不让别人进来，保证了原子性
        count = temp + 1

if __name__ == "__main__":
    threads = []
    start_time = time.time()
    
    # 1. 并发启动 200 个线程
    for i in range(200):
        t = threading.Thread(target=worker_with_lock)
        threads.append(t)
        t.start()
    
    # 2. 统一等待
    for t in threads:
        t.join()
    
    print(f"加上锁的结果：{count}，耗时：{time.time() - start_time:.4f}秒")
```

```python
结果:
加上锁的结果：200，耗时：0.2344秒
把锁的部分去掉,又是争抢,不正常
```

执行结果，稳定输出200

---

死锁现象

在锁使用完毕之后，没有及时的释放，导致其它线程无法执行程序

用with lock就行

```python
import threading

"""
死锁: 在锁使用完毕之后，没有及时的释放，导致其它线程无法执行程序
"""
lock = threading.Lock()

def get_value(index):
    # 上锁
    lock.acquire()
    num_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    try:
        print(num_list[index])
    except Exception:
        lock.release()
    else:
        # 释放锁
        lock.release()
        
if __name__ == '__main__':
    for i in range(11):
        threading.Thread(target=get_value, args=(i,)).start()
```

---

GIL锁

简单了解

**什么是GIL锁: --- 这是Python的一个机制**

GIL是CPython解释器（官方标准Python）中的一个互斥锁，它保证同一时刻，只有一个线程能执行Python字节码。

这意味着，即使你的电脑有8核CPU，使用threading模块开启8个线程做计算，也无法实现并行，效果类似于单核串行。

**为什么会有GIL？**

内存管理简单：CPython使用引用计数来管理内存。如果没有GIL，多个线程同时操作一个对象的引用计数时，必须加大量细粒度锁，这极易导致死锁，且降低单线程性能。

C扩展兼容：大量现有的C语言扩展库（如NumPy早期版本）**假设没有线程竞争，GIL让它们无需修改就能安全地在多线程环境中工作**。

决策：Python之父Guido van Rossum在1992年选择了GIL，是为了牺牲多核性能，换取单线程的简单和高效。

**GIL带来的实际影响**

| 场景 | 影响 | 示例 |
| --- | --- | --- |
| **CPU密集型任务**<br>（循环、数学计算） | **严重拖累**<br>。多线程反而比单线程慢（因为线程切换要抢锁、释放锁）。 | 用8线程做斐波那契数列，耗时反而是单线程的1.5倍。 |
| **IO密集型任务**<br>（网络请求、文件读写、数据库查询） | **几乎无影响**<br>。线程在等待IO响应时，会主动释放GIL，其他线程可以趁机执行。 | 爬虫开启100个线程并发请求网页，效率远高于单线程。 |

所以虽然进程和线程都能开启并发且进程所消耗的内存资源更大,我们有些情况也必须使用多进程来实现并发,

就是因为GIL锁规定的一个进程同一时间只能有一个线程

---

**线程池**

在 Python 中，线程池是一种用于管理和复用线程的**机制**，能够高效地执行并发任务。它通过**预先创建一定数量的线程**，并将它们存储在池中，当有任务需要执行时，直接从池中分配线程来运行任务，任务完成后线程会返回到池中等待下一个任务，从而避免了频繁地创建和销毁线程所带来的开销

**线程池的优势**

- 减少线程创建和销毁的开销：线程的创建和销毁是相对耗时的操作，线程池通过复用线程，避免了频繁的线程创建和销毁，从而提高了程序的性能
- 限制并发数量：线程池可以限制同时运行的线程数量，避免系统资源被过多的线程耗尽，从而提高系统的稳定性。
- 提高响应速度：任务提交后可以直接从线程池中获取线程执行，减少了任务的等待时间，提高了响应速度

**线程池实现**

线程池主要是通过 **concurrent.futures** 模块中的  **ThreadPoolExecutor** 类来实现

创建线程池是固定写法,函数写自己需要的

```python
"""
线程池的使用
"""
from concurrent.futures import ThreadPoolExecutor

# 创建线程池对象  thread_name_prefix 线程名前缀,可以不写
thread_pool = ThreadPoolExecutor(max_workers=3,thread_name_prefix="WorkerThread")   #定义线程池的线程数量（即3个工人）
#主要参数是max_workers,前缀名无所谓

#线程池三个线程并发执行
# 定义一个任务函数
def task(n):
    print(f"Task {n} started")
    time.sleep(2)  # 模拟耗时操作
    return f"Task {n} completed"


if __name__ == '__main__':
    futures = []
    for i in range(5):  #创建5个任务（即5件待处理的活）。
        # 提交任务，传递多个参数
        # future = executor.submit(task, 1, 2, 3)
        future = thread_pool.submit(task, i)  # submit提交任务给线程池 并接收返回结果,
                                              # 线程池对象.submit(函数名,参数1,参数2...)
                                              #这里的future相当于submit创建任务的返回值,他是个特殊的对象,代表了这个任务
                                              #注意他不是结果
        futures.append(future)

    for future in futures:
        print(future.result())

    # 关闭线程池
    thread_pool.shutdown()  #一定要关闭
```

![图片](Python高级笔记_assets/ch4_img08.png)

结果

```python
Task 0 started
Task 1 started
Task 2 started
Task 3 started
Task 4 started
Task 0 completed
Task 1 completed
Task 2 completed
Task 3 completed
Task 4 completed
```

##### 进程

---

**创建进程步骤**

- 导入模块  import **multiprocessing  (真长)**
- 创建进程对象 multiprocessing.Process(target='任务')   #线程不是什么Trea啥的吗
- 开启进程

```python
### multiprocessing.Process 参数详解

processing=multiprocessing.Process(
    target=None,        # 进程要执行的函数
    args=(),            # 位置参数（元组）
    kwargs={},          # 关键字参数（字典）
    daemon=False        # 守护进程（True=主进程结束立即终止子进程）
)
```

---

**进程与线程的重要区别：**

- 进程之间**不共享内存**，每个进程有独立的变量副本(内存)
- 多进程可以绕过 Python 的 GIL 限制，真正利用多核 CPU,高并发的实现
- 进程创建和销毁的开销比线程大得多

Windows 特殊注意：

- **Windows 下必须把进程创建代码放在 if \_\_name\_\_ == '\_\_main\_\_': 里面  #线程可放可不放,但我们养成习惯都放**
- 否则会引发递归创建进程的错误

> 💡 多进程完成多任务

导包不一样,类名不一样,其他和线程一样

```python
import time
import multiprocessing

def sing():
    for i in range(3):
        print('唱歌...')
        time.sleep(0.5)

def dance():
    for i in range(3):
        print('跳舞...')
        time.sleep(1)


if __name__ == '__main__':
    """
        # 单进程实现函数调用
        sing()
        dance()
    """
    # 多进程实现函数调用
    sing_processing = multiprocessing.Process(target=sing)
    dance_processing = multiprocessing.Process(target=dance)
    sing_processing.start()
    dance_processing.start()
    

#如果我们调整一下他们sleep时间,你会发现他们会根据休眠时间各自执行互不干扰,进程之间不会抢了
```

轮流运行,怎样都不会抢

这里看着是一前一后,但实际上是同步运行的,同时运行两个进程

```python
唱歌...
跳舞...
唱歌...
跳舞...
唱歌...
跳舞...
```

进程是单独的个体,不会像线程一样抢资源了,能会正常的执行完,甚至调用全局参数时都是独立的结果,所以才是用来做核心并发功能的前提

---

**进程完成带有参数的任务,同样的**

**也是\*args和\*\*kwargs参数**

```python
"""
    进程执行有参数的任务，可以通过一下两种方式进行参数的传递
    1.元组:多个数据需要注意参数的顺序,按照位置进行参数赋值
    2.字典:字典中的key要保持和函数中的形式参数名字一致 
"""
import time
import multiprocessing


def sing(*args):
    for i in range(3):
        print('唱歌...')
        time.sleep(0.3)


def dance(**kwargs):
    for i in range(3):
        print('跳舞...')
        time.sleep(0.5)


if __name__ == '__main__':
    """
        # 单进程实现函数调用
        sing(arg)
        dance(kwargs)
    """
    # 多进程实现函数调用
    sing_processing = multiprocessing.Process(target=sing, args=(2,))
    dance_processing = multiprocessing.Process(target=dance, kwargs={'num': 3})
    sing_processing.start()
    dance_processing.start()#这里看着是一前一后,但实际上是同步运行的,同时运行两个进程
```

---

**进程编号获取**

当程序中进程的数量越来越多时，如果没有办法区分主进程和子进程还有不同的子进程，那么就无法进行有效的进程管理，

为了方便管理实际上每个进程都是有自己的编号

```markdown
### os 模块进程编号函数说明
import os  
os.getpid()   # 获取当前进程的ID（Process ID）
os.getppid()  # 获取父进程的ID（Parent Process ID）
```

使用场景：

- 调试时确认哪个进程在执行哪个任务
- 进程间通信时标识发送方/接收方
- 监控和管理子进程的生命周期

代码:

```python
import os
import time
import multiprocessing

def sing(num):
    print(f'执行唱歌任务进程的编号是:{os.getpid()},父进程的ID:{os.getppid()}')
    for i in range(num):
        print('唱歌...')
        time.sleep(0.5)
        
def dance(num):
    print(f'执行跳舞任务进程的编号是:{os.getpid()},父进程的ID:{os.getppid()}')
    for i in range(num):
        print('跳舞...')
        time.sleep(0.5)


if __name__ == '__main__':
    print(f'主进程的编号是:{os.getpid()}')  #很好理解,这里的两个紫荆城的父进程都是.py文件,所以父进程是一样的
    sing_processing = multiprocessing.Process(target=sing, args=(2,))
    dance_processing = multiprocessing.Process(target=dance, kwargs={'num': 3})
    sing_processing.start()
    dance_processing.start()
```

结果,每次运行不一样,父进程是同一个

```python
主进程的编号是:34240
执行唱歌任务进程的编号是:38072,父进程的ID:34240
唱歌...
执行跳舞任务进程的编号是:34152,父进程的ID:34240
跳舞...
唱歌...
跳舞...
跳舞...
```

---

守护进程

和守护线程一样样的

注意：

- 主进程会等待所有的子进程执行结束再结束
- 设置守护主进程，可以实现在主进程退出时，子进程全部销毁

```markdown
### 守护进程（daemon）详解

# 设置守护进程的方式
# 构造时设置
p = multiprocessing.Process(target=task, daemon=True)
```

**守护进程的行为：**

- 当主进程执行完毕时，所有守护的子进程会被强制终止
- 守护进程中打开的文件、网络连接可能不会被正确关闭
- 适合执行"不重要"的后台任务

**守护进程 vs 正常进程：**

|  | 守护进程 | 正常进程 |
| --- | --- | --- |
| 主进程结束后 | 立即杀死 | 等待完成 |
| 适合任务 | 辅助性任务 | 核心业务任务 |
| 资源清理 | 不保证 | 正常清理 |

代码:

```python
import os
import time
import multiprocessing

def task():
    for i in range(10):
        print(f'子进程[{os.getpid()}]正在执行一个任务...')
        time.sleep(0.2)

if __name__ == '__main__':
    task_processing = multiprocessing.Process(target=task, daemon=True)   
    task_processing.start()
    time.sleep(2.6)
    print(f'主进程[{os.getpid()}]任务执行结束....')
```

结果

正常子进程应该每0.2秒执行一次,但是进程2.6s就结束了,这里只执行5次子进程

```python
子进程[34684]正在执行一个任务...
子进程[34684]正在执行一个任务...
子进程[34684]正在执行一个任务...
子进程[34684]正在执行一个任务...
子进程[34684]正在执行一个任务...
主进程[33468]任务执行结束....
```

---

案例: 多进程拷贝器(先放着,活着回来再学)

需求:把文件夹 AI大模型实战 里的所有文件，逐个复制到 AI模型 文件夹里。

具体做法：

- 遍历 AI大模型实战 文件夹里的每个文件
- 每遇到一个文件，就创建一个新的子进程，让这个子进程去执行 copy\_task 函数
- copy\_task 函数负责：打开源文件 → 逐块读取（1024字节）→ 写入目标文件 → 直到读完

一次读完,适合内存小的文件

![图片](Python高级笔记_assets/ch4_img09.png)

```python
import multiprocessing
import os
import time

def copy_task(file_name, source_name, target_name):
    file_path_source = source_name + '\\' + file_name   #源文件地址  只有文件夹名字不同
    file_path_target = target_name + '\\' + file_name   #目标文件地址 只有文件夹名字不同
    with open(file_path_source, 'rb') as f_source:
        with open(file_path_target, 'wb') as f_target:
            while True:
                data = f_source.read(1024)
                if data:
                    f_target.write(data)
                else:
                    break

# 创建多进程时 windows环境下 记得加__name__ == '__main__' 否则可能出错
if __name__ == '__main__':
    # 资源目录
    source = 'AI大模型实战'
    # 目标目录
    target = 'AI模型'

    # 判定目标目录是否存在 不存在则创建
    if not os.path.exists(target):
        os.mkdir(target)

    # 获取资源目录中的所有文件
    file_list = os.listdir(source)
    print(f'开始拷贝{source} ===>  {target}')
    start = time.time()
    for file in file_list:
        # 遍历文件 每遍历一个文件则创建一个进程
        #由于file是从file_list列表中循环读取的,
        #所以每次值都不一样,也就说明每次生成的sub_process都不是同一个进程
        sub_process = multiprocessing.Process(target=copy_task, args=(file, source, target))  
        sub_process.start()
        # copy_task(file, source, target)
    end = time.time()
    print(f'{source} ===>  {target} 拷贝结束，耗时 {(end - start):.2f}s
```

代码详细解释:

```markdown
def copy_task(file_name, source_name, target_name):
    # 拼接源文件完整路径：源文件夹 + 文件名
    file_path_source = source_name + '\\' + file_name
    # 拼接目标文件完整路径：目标文件夹 + 文件名
    file_path_target = target_name + '\\' + file_name
    
    # 以二进制读取模式打开源文件
    with open(file_path_source, 'rb') as f_source:
        # 以二进制写入模式打开目标文件（不存在会自动创建）
        with open(file_path_target, 'wb') as f_target:
            while True:
                # 每次读取1024字节，读到文件末尾时返回空字节串 b''
                data = f_source.read(1024)
                if data:          # 如果读到了数据
                    f_target.write(data)  # 写入目标文件
                else:             # 读到文件末尾
                    break         # 跳出循环
```

为什么用 1024 字节？

- 一次性读整个文件可能占用大量内存
- 分块读取适合大文件，内存占用恒定
- 1024 是一个平衡性能与内存的常见选择

##### 协程(了解)

为什么需要协程？

1. 并发困境：
  1. 传统同步代码（串行执行）在执行网络请求、数据库查询或文件读写（I/O操作）时，CPU会空闲等待，造成资源浪费。
  1. 多线程虽然能解决阻塞问题，但在Python中受限于GIL（全局解释器锁），且线程切换开销大，不适用于高并发场景。
1. 核心目标：
  1. **用单线程实现高并发。**
  1. 将同步阻塞的代码，写成异步非阻塞的形式，提升程序执行效率。

协程是什么？

1. 定义：
  1. **协程是一种用户态的轻量级线程(是一种特殊的线程)**。它的调度不由操作系统控制，而是由Python解释器（事件循环）控制。
1. **核心机制：**
  1. 协作式调度：任务遇到I/O等待时，主动让出（await）CPU，让事件循环去执行其他任务；I/O其他任务完成后，再恢复执行。
  1. 单线程内并发：同一时刻只有一个协程在运行，不存在竞争条件（无需加锁），但需要主动交出控制权。
1. 演进历史（了解即可）：
  1. yield（生成器） -> @asyncio.coroutine + yield from -> async/<u>***await***</u>（Python 3.5+，现代标准）。

可以把协程想象成用户态下的“轻量级线程”。

- 线程，像是公司里的正式员工。招一个员工（创建线程）成本高，给他分配任务后，如果他去等一个漫长的快递（I/O等待），这个人就站在那里干等着，什么活也干不了。如果公司有100个这样的任务，就得招100个员工，人力成本（内存和切换开销）巨大。
- 协程，则像是快递站里的“超级分拣员”。整个快递站可能就只有几个这样的分拣员（对应少量线程）。他们手里有几百个包裹要处理（对应几百个协程任务）。当一个分拣员发现某个包裹需要等电话确认（I/O操作）时，他不会傻等，而是把这个包裹暂放一边，立刻拿起下一个包裹继续处理。等电话来了，他再回过头来处理这个包裹。(利用等待时间做别的任务)

```markdown
### 核心概念速查表

|           概念            |                     解释                          |         类比                 |
|---------------------------|---------------------------------------------------|------------------------------|
| **事件循环（Event Loop）** | 管理所有协程的调度器，负责决定下一个执行哪个协程     | 餐厅的排号系统，叫到号的去取餐 |
| **协程函数**               | 用 `async def` 定义的函数，调用时返回协程对象      | 一张"任务单"，还没开始做 |
| **协程对象**               | 调用协程函数得到的对象，需要放入事件循环执行         | 排了号的顾客，正在等待叫号 |
| **Task（任务）**           | 用 `create_task()` 包装后的协程，已加入事件循环     | 已经被叫到号、正在准备取餐的顾客 |
| **Future（未来对象）**     | 一个"占位符"，代表将来会有的结果                    | 取餐小票，代表你点的餐将来会做好 |

### await 的详细说明,
在协程函数内
await 可等待对象  #加一个await就行,用户态的很简单
```

作用：

1. 暂停当前协程的执行
1. 让出 CPU 控制权给事件循环
1. 等待"可等待对象"完成后，恢复执行

可等待对象（Awaitable）包括：

- 另一个协程（await other\_coro()）
- Task 对象（await task）
- Future 对象
- 任何实现了 \_\_await\_\_ 方法的对象

具体操作

asyncio:异步i/o

1. 定义协程函数  async def

```python
import asyncio  #导入协程类

# 使用 async def 定义协程函数
async def say_hello():
    print("Hello")
     # 模拟耗时操作，await 会主动交出控制权
     #注意虽然他的效果和time.sleep(1)一样都是休眠一秒,但await会让出控制权,让系统去执行其他的代码
    await asyncio.sleep(1)   #主要协程处,休眠一秒
    print("World")
    
# 方式一：直接运行（简单场景）
asyncio.run(say_hello())  ***************

# 方式二：获取事件循环运行（更底层的旧写法，了解即可）
# loop = asyncio.get_event_loop()
# loop.run_until_complete(say_hello())
```

根据上面的代码我们可以再具体的看看await的功能

创建任务create\_task

```python
import asyncio

async def say_hello():
    print("Hello")
    await asyncio.sleep(1)   # 挂起，交出控制权
    print("World")

async def main():
    # 同时创建两个任务create_task
    task1 = asyncio.create_task(say_hello())
    task2 = asyncio.create_task(say_hello())
    
    await task1  #设置协程任务,交出控制权后执行task2
    await task2

asyncio.run(main())
```

结果

很好理解啊

```python
Hello
Hello
World
World
```

核心语法详解

1. async def：定义协程函数

调用时不会立即执行，而是返回一个协程对象。

必须放入事件循环中执行。

2. await：等待可等待对象

功能：挂起当前协程，让出控制权，直到后面的可等待对象执行完毕。

他不需要用户指定等待时长,系统会自动根据情况进行分配资源

**只需要在可能高并发的地方加上await即可(例如网络请求,数据库I/O)**

对象类型：

另一个协程（await other\_coroutine()）。

异步操作（如 asyncio.sleep()）。

asyncio.Future / Task。

理解： Future 是“一个装着未来结果的盒子”，Task 是“正在跑的一个协程”。await 的作用就是“等这个盒子里的结果

被填进去”。

Task（任务）：用 asyncio.create\_task() 把你写的协程包起来，它就变成一个“任务对象”，扔进事件循环开

始跑。你 await 它，就是等它跑完。

Future（未来对象）：更底层，可以理解为“Task 的爹”。通常你写业务代码不会直接接触它，

但一些底层库会用。

3. asyncio.run()：启动入口

创建新的事件循环，运行传入的协程，结束后关闭循环。

注意：一个程序里通常只用这一个入口。

##### 并发执行

场景：同时执行多个任务，而不是排队等待。

```markdown
### 三种并发方式的对比
| 方式 | 代码 | 特点 | 适用场景 |
|------|------|------|---------|
| `create_task()` | `task = asyncio.create_task(coro())` | 立即开始调度，需要手动 await | 单个任务的精细控制 |
| `gather()` | `await asyncio.gather(coro1(), coro2())` | 并发执行，返回所有结果列表 | 批量执行同类任务 |
| `wait()` | `await asyncio.wait(tasks, timeout=2)` | 可设置超时和返回条件 | 需要控制超时或部分完成的场景 |

**示例对比：**
# 方式1：create_task - 分步控制
task1 = asyncio.create_task(fetch(1))
task2 = asyncio.create_task(fetch(2))
await task1  # 等第一个完成
await task2  # 等第二个完成

# 方式2：gather - 批量并发
results = await asyncio.gather(fetch(1), fetch(2), fetch(3))
# 全部完成，results 是按顺序的结果列表

# 方式3：wait - 带超时
tasks = [asyncio.create_task(fetch(i)) for i in range(5)]
done, pending = await asyncio.wait(tasks, timeout=3)
# done: 3秒内完成的任务  pending: 未完成的任务（可以取消或继续等待）
```

详细案例

<u>***1. asyncio.create\_task()（推荐）***</u>

将协程包装成一个Task对象，加入事件循环，立即开始调度（不必等待await）。

```python
import asyncio

async def say_hello():
    print("Hello")
    await asyncio.sleep(1)   # 挂起，交出控制权
    print("World")

async def main():
    # 创建任务，让它们立即开始准备执行
    task1 = asyncio.create_task(say_hello())
    task2 = asyncio.create_task(say_hello())
    
    # 必须手动 await 任务，确保它们完成后再退出 main()
    await task1
    await task2
```

2. asyncio.gather()

用于并发运行多个协程，并返回所有结果。

```python
async def main():
    # 同时执行 say_hello() 两次，总耗时约1秒，而不是2秒
    results = await asyncio.gather(
        say_hello(),
        say_hello()
    )
    print(results)
```

3. asyncio.wait()

更灵活地控制任务，可以设置超时或返回条件（如FIRST\_COMPLETED）。

```python
async def main():
    tasks = [asyncio.create_task(say_hello()) for i in range(5)]
    done, pending = await asyncio.wait(tasks, timeout=2)
```

实战案例（爬虫模拟）(不看)

需求：模拟并发请求3个网站，对比同步与异步的耗时。

```python
import asyncio
import aiohttp  # 第三方库，支持异步HTTP请求
import time

async def fetch_url(session, url):
    print(f"开始请求: {url}")
    async with session.get(url) as response:
        # 异步等待响应
        await asyncio.sleep(2)  # 模拟网络延迟
        print(f"完成请求: {url}")
        return response.status

async def main():
    urls = ["http://example.com"] * 5
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_url(session, url) for url in urls]
        # 并发执行
        results = await asyncio.gather(*tasks)
        print(f"所有状态码: {results}")
 
# 总耗时约为 2 秒，而不是 10 秒
start = time.time()
asyncio.run(main())
print(f"总耗时: {time.time() - start}")
```

异步上下文管理器 与 异步迭代器(不看)

```markdown
### 异步语法对照表

| 同步写法 | 异步写法 | 说明 |
|---------|---------|------|
| `with open() as f:` | `async with aiohttp.ClientSession() as session:` | 异步上下文管理器，进入/退出时可能发生 I/O |
| `for i in range(10):` | `async for data in async_generator():` | 异步迭代器，每次取值可能需要异步等待 |
| `def func():` | `async def func():` | 协程函数定义 |
| `return` | `return` 或 `await` | 协程中返回结果 |
```

1. 异步上下文管理器 (async with)

用于管理需要异步操作的资源（如数据库连接、网络会话）。

必须实现 \_\_aenter\_\_ 和 \_\_aexit\_\_。

示例：async with aiohttp.ClientSession() as session:

2. 异步迭代器 (async for)

用于遍历需要异步获取的数据流。

必须实现 \_\_aiter\_\_ 和 \_\_anext\_\_。

```python
# 示例：异步生成器（Python 3.6+）
async def async_counter():
    for i in range(3):
        await asyncio.sleep(1)
        yield i

async def main():
    async for num in async_counter():
        print(num)  # 每隔1秒打印一个数
```

##### 三种区别

**三种任务方式区别:**

| 特性 | 协程 (Coroutine) | 线程 (Thread) | 进程 (Process) |
| --- | --- | --- | --- |
| **调度方式** | **用户态**<br>，由程序员或运行时（如Go的调度器）控制 | **内核态(程序态)**<br>，由操作系统抢占式调度 | **内核态**<br>，由操作系统调度 |
| **创建/切换成本** | **极低**<br>（只需保存几个寄存器状态，内存占用KB级别） | 较高（需要内核模式切换，内存占用MB级别） | 很高（涉及独立内存空间分配） |
| **并发数量** | 可以轻松创建成千上万个 | 受系统资源限制，通常几百到几千个 | 更少，通常几十个 |
| **适用场景** | **I/O密集型**<br>（网络请求、数据库读写、文件操作） | CPU密集型 或 需要利用多核计算 | 隔离性要求高的独立服务 |

**使用场景:**

| 技术 | 优势 | 短板 |
| --- | --- | --- |
| **协程 (asyncio)** | **高并发 I/O**，轻量切换 | **不能执行 CPU 密集型计算**<br>（会阻塞事件循环） |
| **线程 (Thread)** | 能处理阻塞操作，共享内存方便 | **有 GIL 限制，**大量线程切换开销大 |
| **进程 (马蹄Process)** | 真正利用多核 CPU，绕过 GIL | 创建成本高，内存隔离，通信复杂 |

**组合的原因**：在一个异步程序中，你不可避免地会遇到**三种类型的任务**：

1. **高并发I/O 密集** → 用协程（网络请求、数据库查询）
1. **CPU 密集** → 用进程（图像处理、数据分析、加密解密）
1. **阻塞式 I/O** → 用线程（某些没有异步支持的第三方库，比如旧的数据库驱动）

##### 综合练习(先不看了,看三程定义)

**作业:  **

**题目1：假设你有一个文件列表[****可使用小米轮播图图片作为目标资源****]，需要从网络上下载这些文件。使用threading 模块实现多线程下载功能**

要求如下：**哇塞 ,再看一遍**

1. 创建一个函数 download\_file(url)
1. 创建多个线程，同时下载多个文件
1. 确保主线程等待所有子线程完成后再退出

```python
import threading
import requests

img_paths = [
    'https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/2e389157059c44d9352b42e04407cbb7.jpg',
    'https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/c71581f232c12eb988a565c368364930.jpg',
    'https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/5d2e4ce0cc22c7d87981bac22d64e44d.jpg'
]

def download(url:str):
    r = requests.get(url)
    with open(url.split('/')[-1], 'wb') as f:
        f.write(r.content)

for img_path in img_paths:
    threading.Thread(target=download, args=(img_path,)).start()
```

**题目2：使用 Python 的 concurrent.futures.ThreadPoolExecutor 实现一个批量任务处理程序**

要求如下：nm,没听  16.00

1. 创建一个任务列表，每个任务是一个简单的函数调用（例如：计算一个数的平方）
1. 使用 ThreadPoolExecutor 创建一个线程池，最大线程数为 5。
1. 将任务提交到线程池中，并收集每个任务的返回值。
1. 打印所有任务的返回值

```python
def calculator(num):
    return num ** 2

from concurrent.futures import ThreadPoolExecutor

thread_pool = ThreadPoolExecutor(max_workers=5)

futures = []
for i in range(10):
    futures.append(thread_pool.submit(calculator, i))

for future in futures:
    print(future.result())
```

**题目3: 分别使用多进程和多线程范式实现****文件拷贝器，完成某文件夹下文件资源的拷贝**

```python
import time, os
import threading

def copy_task(file_name, source_name, target_name):
    file_path_source = source_name + '\\' + file_name
    file_path_target = target_name + '\\' + file_name
    with open(file_path_source, 'rb') as f_source:
        with open(file_path_target, 'wb') as f_target:
            while True:
                data = f_source.read(1024)
                if data:
                    f_target.write(data)
                else:
                    break


if __name__ == '__main__':
    # 资源目录
    source = '../python_day07'
    # 目标目录
    target = '../python_day07_cp'

    # 判定目标目录是否存在 不存在则创建
    if not os.path.exists(target):
        os.mkdir(target)

    # 获取资源目录中的所有文件
    file_list = os.listdir(source)
    print(f'开始拷贝{source} ===>  {target}')
    start = time.time()
    for file in file_list:
        # 遍历文件 每遍历一个文件则创建一个进程
        sub_thread = threading.Thread(target=copy_task, args=(file, source, target))
        sub_thread.start()
    end = time.time()
    print(f'{source} ===>  {target} 拷贝结束，耗时 {(end - start):.2f}s')
```

---

### 网络编程

#### 什么是 Socket?

Socket又称套接字，应用程序通常通过"套接字"向网络发出请求或者应答网络请求，使主机间或者一台计算机上的进程间可以通讯

<u>***Python 网络编程的本质是：通过 ***</u>`socket()`<u>*** 申请系统资源，用 ***</u>`bind()`<u>*** 和 ***</u>`listen()`<u>*** 设置服务端“门面”，利用内核的缓冲区和排队机制，经过 三次握手 后由 ***</u>`accept()`<u>*** 生成专属通信管道，最后通过操作缓冲区来收发数据，结束时用 四次挥手 释放资源。***</u>

> 💡 **1. 什么是 Socket（套接字）？**
>
> - **官方定义**：Socket 是应用层与 TCP/IP 协议族通信的中间软件抽象层，它是一组接口。在设计模式中，Socket 其实就是一个门面模式，它把复杂的 TCP/IP 协议族隐藏在 Socket 接口后面，对用户来说，一组简单的接口就是全部。
> - **通信四元组**：一个网络中的 Socket 连接，由 `源IP`、`源端口`(port)、`目的IP`、`目的端口` 这四个元素唯一确定,起点和终点。(ipconfig查看ip)
> - **缓冲区**：每个 Socket 在操作系统内核中都有**发送缓冲区**和**接收缓冲区**。**就像邮局作为一个中间站存信寄信**,`send()` 方法是将数据写入发送缓冲区，由操作系统负责真正发送；`recv()` 方法是从接收缓冲区读取数据。
>
> **2. 为什么需要 **`bind()`** 和 **`listen()`**？**
>
> - `bind()`：相当于给 Socket 绑定一个“门牌号”（IP + 端口），让操作系统知道这个 Socket 在哪个地址上提供服务。
> - `listen()`：将主动连接套接字变为被动连接套接字，告诉操作系统内核：“我准备好了，可以接受客户端的连接请求了”。参数（如 `5`）表示内核为该 Socket 建立的**半连接队列**的最大长度。
>
> **3. TCP Socket 的“三次握手”与 **`accept()`
>
> - 当客户端调用 `connect()` 时，底层开始三次握手。(TCP三次握手,四次挥手)
> - 握手完成后，连接会进入内核的**已完成连接队列**。
> - 服务端调用 `accept()`，就是从该队列中取出一个已经建立好的连接，并返回一个新的 Socket 对象（`conn`）专门用于和这个客户端通信，而原来的监听 Socket（`phone`）则继续监听新的连接。

**socket()函数**

**导入socket模块**

Python 中，我们用 socket（）函数来创建套接字，语法格式如下：

```
import scoket
socket.socket(family, type, proto)#一般只写前两个
```

**参数**

- family: 套接字家族可以使 AF\_UNIX 或者 AF\_INET
- type: 套接字类型可以根据是面向连接的还是非连接分为 SOCK\_STREAM(一般是这个) 或 SOCK\_DGRAM
- protocol: 一般不填默认为 0,协议参数用于指定套接字使用的具体传输协议,在大多数情况下，协议参数可以省略或设置为0。这是因为：默认协议：当协议参数为0时，操作系统会根据套接字的家族和类型自动选择一个默认协议。例如：
  - 如果套接字类型是SOCK\_STREAM，默认协议通常是**TCP**。
  - 如果套接字类型是SOCK\_DGRAM，默认协议通常是**UDP**。
  - 如果套接字家族是AF\_INET，默认协议族是**IPv4**。
  - 如果套接字家族是AF\_INET6，默认协议族是**IPv6**

![图片](Python高级笔记_assets/ch4_img10.png)

****

---

下面这些都是可以写死的

#### 简单实例

基本这些代码都是写死的,主要还是理解概念

一对一连接,对话一次结束

服务端(先搭建),接收响应

我们使用 socket 模块的 **socket** 函数来创建一个 socket 对象。socket 对象可以通过调用其他函数来设置一个 socket 服务。

现在我们可以通过调用 **bind(hostname, port)** 函数来指定服务的 IP + port(端口号)。接着，我们调用 socket 对象的 *accept* 方法。该方法等待客户端的连接，并返回 *connection* 对象，表示已连接到客户端。

完整代码如下：

```python
import socket
"""
为了便于理解socket的通信流程，可以将socket的通信类比成手机通信
1.创建基站
2.设置基站位置
3.开机
4.监听来电
"""
# 创建基站 socket.AF_INET 表示网络套接字   socket.SOCK_STREAM 使用tcp协议
phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)   #IPv4,TCP
# 设置基站位置  port  0-65535  0-1024 操作系统  
phone.bind(('127.0.0.1', 8080))  #127.0.0.1  或者localhost本机,门牌号
# 开机监听来电  最大挂起的个数
phone.listen(5)  #同时监听5个

# 等待连接
print('服务端启动,等待连接....')
# 【知识点】accept() 默认是阻塞的：如果没有客户端连接，程序会停在这里，直到有连接到来
conn, accept = phone.accept()  # 阻塞等待连接,停在这里,等着连接
print('一个客户端连接了....')
# 通过conn套接字接收，发送数据  1024字节
# 【知识点】recv() 默认也是阻塞的：如果客户端没有发送数据，程序会停在这里
# 当 recv() 返回空字节串 b'' 时，代表客户端已经正常关闭连接（发送了 FIN 包）
data = conn.recv(1024)  # 阻塞等待数据,1024是缓存大小,默认也是1024
print('receive client message:',data.decode('utf-8')) #修改字符集
# 【知识点】send() 是将数据写入内核缓冲区，并不保证数据已到达对方网卡
conn.send(data.upper())  #写入发送缓冲区并大写
```

![图片](Python高级笔记_assets/ch4_img11.png)

客户端

接下来我们写一个简单的客户端实例连接到以上创建的服务。端口号为 9999。

**socket.connect(hostname, port )** 方法打开一个 TCP 连接到主机为 *hostname* 端口为 *port* 的服务商。连接后我们就可以从服务端获取数据，记住，操作完成后需要关闭连接。

完整代码如下：

```python
import socket
# socket.AF_INET 表示网络套接字   socket.SOCK_STREAM 使用tcp协议
phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM) #和服务端必须保持一致
# 连接服务器  port  0-65535  0-1024 操作系统
phone.connect(('127.0.0.1', 8080))  #目标端的ip地址和网段
user_input = input(">>")#用户输入区
phone.send(user_input.encode('utf-8'))  #发送到缓冲区
data = phone.recv(1024)
print('receive sever response:',data.decode('utf-8'))
# 释放资源
phone.close()  #生命周期目前就一次,连接完一定要释放资源
```

![图片](Python高级笔记_assets/ch4_img12.png)

![图片](Python高级笔记_assets/ch4_img13.png)

#### 连续会话

上面那个发送一次就结束了,这个加了个while true,能多次发送和接收

服务端

```python
import socket

# socket.AF_INET 表示网络套接字   socket.SOCK_STREAM 使用tcp协议
phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 绑定手机卡  port  0-65535  0-1024 操作系统
phone.bind(('127.0.0.1', 8080))

# 开机监听来电  最大挂起的个数
phone.listen(5)

# 等待连接
print('服务端启动,等待连接....')
conn, accept = phone.accept()
print('一个客户端连接了....')
while True:
   # 通过conn套接字接收，发送数据  1024字节
    # 【知识点】TCP 是流式协议，没有消息边界，这里可能遇到"粘包/拆包"问题：
    # recv(1024) 指定最多读取 1024 字节，但可能一次读取到多条消息（粘包），
    # 也可能一条消息需要多次读取才能完整接收（拆包）。
    # 解决思路：自定义协议，如使用固定头（header）声明消息体长度。
    data = conn.recv(1024)  #这他喵是啥
    if not data:  # 客户端断开连接，recv 返回空数据，跳出循环
        break
    print('receive client message:', data.decode('utf-8'))
    conn.send(data.upper())

# 释放资源
conn.close()
phone.close()
```

客户端

```python
import socket

# socket.AF_INET 表示网络套接字   socket.SOCK_STREAM 使用tcp协议
phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 连接服务器  port  0-65535  0-1024 操作系统
phone.connect(('127.0.0.1', 8080))
while True:
    user_input = input(">>")
    # 输入空值问题解决
    if not user_input:
        break
    phone.send(user_input.encode('utf-8'))
    data = phone.recv(1024)
    print('receive sever response:',data.decode('utf-8'))
# 释放资源
phone.close()
```

#### 循环连接

一个用户连接退出后,又一次连接,服务端并不结束,再套一层while True

```python
import socket

# socket.AF_INET 表示网络套接字   socket.SOCK_STREAM 使用tcp协议
phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 绑定手机卡  port  0-65535  0-1024 操作系统
phone.bind(('127.0.0.1', 8080))

# 开机监听来电  最大挂起的个数
phone.listen(2)

# 等待连接
print('服务端启动,等待连接....')
while True:
    conn, accept = phone.accept()
    while True:
        # 通过conn套接字接收，发送数据  1024字节
        data = conn.recv(1024)
        if not data:
            break
        print('receive client message:', data.decode('utf-8'))
        conn.send(data.upper())
    # 释放资源
    conn.close()
    
phone.close()
```

#### 并发连接

> 💡 **【为什么需要并发？】**
>
> 在之前的“连续会话”示例中，服务端是单线程的。当 `handle_client` 中的 `recv()` 阻塞时，整个服务端都无法接受新的客户端连接(多个连接一个不行)。如果有多个客户端同时连接，后到的客户端将永远等待。
>
> **解决方案**：主线程只负责 `accept()` 接受新连接，连接建立后立即创建一个新的子线程来处理该客户端的会话，主线程则继续循环接受下一个连接。这样就能实现“一对多”的并发通信。

服务端:

```python
import socket ,threading

# socket.AF_INET 表示网络套接字   socket.SOCK_STREAM 使用tcp协议
phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 绑定手机卡  port  0-65535  0-1024 操作系统
phone.bind(('127.0.0.1', 8080))

# 开机监听来电  最大挂起的个数
phone.listen(2)
def handle_client(conn):
    while True:
        # 通过conn套接字接收，发送数据  1024字节
        data = conn.recv(1024)
        if not data:
            break
        print('receive client message:', data.decode('utf-8'))
        conn.send(data.upper())
    # 释放资源
    conn.close()

# 等待连接
print('服务端启动,等待连接....')
while True:
    conn, accept = phone.accept()
    threading.Thread(target=handle_client, args=(conn,)).start()
phone.close()
```

客户端

```python
import socket

# socket.AF_INET 表示网络套接字   socket.SOCK_STREAM 使用tcp协议
phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 连接服务器  port  0-65535  0-1024 操作系统
phone.connect(('127.0.0.1', 8080))
while True:
    user_input = input(">>")
    if not user_input:
        continue
    if user_input == "exit":
        break
    phone.send(user_input.encode('utf-8'))
    data = phone.recv(1024)
    print('receive sever response:',data.decode('utf-8'))
# 释放资源
phone.close()
```

#### 简易版聊天室

消息群发

服务端

```python
import socket
from concurrent.futures import ThreadPoolExecutor
# 创建连接池对象
connection_pool = ThreadPoolExecutor(max_workers=5)
"""
1. 初始化服务端
2. 启动服务端
3. 实现消息群发  收集客户端生成的socket   遍历socket
参数说明:
   host: ip地址
   port: 端口
   max_connections：最大挂起数量
"""
class ChatServer:
    def __init__(self, host, port, max_connection):
        self.host = host
        self.port = port
        self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.server_socket.bind((self.host, self.port))
        self.server_socket.listen(max_connection)
        self.client_sockets = []

    def handle_connection(self, c_socket, client_address):
        try:
            while True:
                client_message = c_socket.recv(1024).decode('utf-8')
                if client_message:
                    print(f'{client_address}消息:{client_message}')
                    # 遍历所有socket群发
                    for client_socket in self.client_sockets:
                        if client_socket != c_socket:
                            client_socket.send(client_message.encode('utf-8'))
        except ConnectionResetError:
            self.client_sockets.remove(c_socket)
            print(f'{client_address}离开聊天室...')

    def start_server(self):
        print('服务端启动，等待客户端连接...')
        while True:
            client_socket, client_address = self.server_socket.accept()
            print(f'{client_address}进入聊天室...')
            self.client_sockets.append(client_socket)
            threading.Thread(target=self.handle_connection, args=(client_socket, client_address)).start()


if __name__ == '__main__':
    try:
        chat_server = ChatServer('127.0.0.1', 9999, 5)
        chat_server.start_server()
    except Exception as e:
        print(f'服务端异常:{e}')
```

客户端

```python
import socket
import threading

"""
1.初始化客户端
2.发送信息
3.接收信息
4.启动客户端
"""
class ChatClient:
    def __init__(self, host, port):
        self.host = host
        self.port = port
        self.client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.client_socket.connect((self.host, self.port))
        print('服务器连接成功,开始聊天吧...')

    # 发送信息
    def send_msg(self, chat_socket: socket):
        while True:
            message = input('>>')
            if message == 'exit':
                print(f'{self.host}:退出')
                break
            if message:
                self.client_socket.send(message.encode('utf-8'))
            else:
                continue
        self.client_socket.close()

    # 接收信息
    def recve_msg(self, chat_socket: socket):
        try:
            while True:
                data = chat_socket.recv(1024).decode('utf-8')
                print(f'收到信息:{data}')
        except ConnectionResetError as e:
            print('远程主机强迫关闭了一个现有的连接')
        except ConnectionAbortedError as e:
            print('客户端连接已断开！！！')

    # 启动服务
    def start_service(self):
        threading.Thread(target=self.send_msg, args=(self.client_socket,)).start()
        threading.Thread(target=self.recve_msg, args=(self.client_socket,)).start()


if __name__ == '__main__':
    try:
        chat_client = ChatClient('127.0.0.1', 9999)
        chat_client.start_service()
    except ConnectionRefusedError:
        print('客户端启动失败,连接被拒绝,请重试...')
```

---

#### 窗口版聊天室(不看,太low了)

**使用wxPython实现聊天室**

**wxPython** 是一个开源的 Python 扩展库，用于创建跨平台的图形用户界面（GUI）应用程序。它基于 **wxWidgets**（一个用 C++ 编写的跨平台 GUI 工具包），通过 Python 的简洁语法和强大的功能，使得开发者能够快速构建具有本地外观和感觉的桌面应用程序

> 💡 **核心特点**

1. **跨平台性**

wxPython 支持多种操作系统，包括 Windows、macOS 和 Linux。开发者只需编写一次代码，即可在不同平台上运行，同时保持本地化的外观和体验。

1. **丰富的控件库**

提供了大量标准的 GUI 控件，如按钮（Button）、文本框（TextCtrl）、列表框（ListBox）、菜单（Menu）、对话框（Dialog）等，满足大多数应用程序的需求。

1. **本地化外观**

wxPython 生成的应用程序在不同平台上会自动适配本地的 UI 风格。例如，在 Windows 上看起来像 Windows 应用程序，在 macOS 上看起来像 macOS 应用程序。

1. **强大的事件处理机制**

通过事件绑定，可以轻松处理用户的交互操作，如点击按钮、输入文本、鼠标移动等。

1. **与 Python 深度集成**

作为 Python 的扩展库，wxPython 充分利用了 Python 的简洁语法和强大的功能，降低了开发难度，提高了开发效率

> 💡 **基本架构**

1. 应用程序对象（wx.App）每个 wxPython 程序都需要一个 wx.App 对象，它是应用程序的入口点，负责管理应用程序的生命周期。
1. 窗口（wx.Frame）窗口是用户可以看到的主界面，通常包含菜单栏、工具栏、状态栏等。wx.Frame 是窗口的基类。
1. 面板（wx.Panel）面板是窗口内的一个容器，用于放置其他控件（如按钮、文本框等）。它可以帮助组织界面布局。
1. 控件（Widgets）控件是用户可以直接交互的界面元素，如按钮、文本框、列表框等。
1. 布局管理器（Sizers）布局管理器用于自动排列控件，确保界面在不同分辨率和窗口大小下保持良好的布局。
1. 事件处理用户的操作（如点击按钮、输入文本）会触发事件，开发者可以通过绑定事件处理函数来响应这些操作。

> 💡 **【为什么 GUI 编程必须用多线程？】**
>
> - `wxPython` 的 `MainLoop()` 运行在主线程，负责处理界面的绘制和用户点击等事件，这个线程也叫 **UI 线程**。
> - 如果我们在 UI 线程中执行 `socket.recv()` 这种**阻塞操作**，整个界面会“卡死”，无法响应任何操作。
> - 因此，必须将网络收发的逻辑放在**子线程**中执行。
>
> **【更新 UI 的安全方式】**
>
> 在子线程中直接操作 UI 控件（如 `self.show_info.AppendText()`）是**不安全**的，可能导致程序崩溃。`wxPython` 提供了 `wx.CallAfter` 方法，可以安全地将 UI 更新操作交由主线程执行：
>
> ```python
> # 在子线程中安全更新 UI 的写法
wx.CallAfter(self.show_info.AppendText, '收到消息: ' + message)
> ```

##### 快速上手

安装wxPython库

```python
pip install wxPython
```

```python
import wx
def clear_info(event):
    print(f'清空信息')
    input_info.Clear()
    
def send_info(event):
    print(f'发送信息',{input_info.GetValue()})
    show_info.AppendText(input_info.GetValue()+'\n')
    input_info.Clear()

if __name__ == '__main__':
    app = wx.App()
    # 参数None表示顶级父窗口
    frame = wx.Frame(None, title='测试窗口', size=(500, 600), pos=(500, 50))
    frame.Show()
    # 创建面板
    panel = wx.Panel(frame, size=(500, 600))
    panel.SetBackgroundColour('white')
    # 显示面板
    panel.Show()
    # 创建普通文本
    panel_text = wx.StaticText(panel, label='hello python', pos=(150, 200))
    # 设置字体颜色
    panel_text.SetForegroundColour('#ccc')
    # 创建字体
    font = wx.Font(20, wx.FONTFAMILY_DEFAULT, wx.FONTSTYLE_NORMAL, wx.FONTWEIGHT_BOLD)
    # 设置字体大小
    panel_text.SetFont(font)
    
    # 创建文本框
    show_info = wx.TextCtrl(panel, pos=(10, 50), size=(460, 300), style=wx.TE_READONLY | wx.TE_MULTILINE)
    input_info = wx.TextCtrl(panel, pos=(10, 380), size=(460, 100), style=wx.TE_MULTILINE)
    # 创建按钮
    clear_btn = wx.Button(panel, label='清空信息', pos=(10, 500), size=(100, 40))
    send_btn = wx.Button(panel, label='发送信息', pos=(360, 500), size=(100, 40))
    
    # 绑定按钮事件
    clear_btn.Bind(wx.EVT_BUTTON, clear_info)
    send_btn.Bind(wx.EVT_BUTTON, send_info)
    app.MainLoop()
```

![图片](Python高级笔记_assets/ch4_img14.png)

其它组件使用

```python
def choice(event):
    print(select.GetStringSelection())
    print(event.GetString())


def comboBox_fn(event):
    print(event.GetString())


def on_checkbox(event):
    # 获取事件对象（即复选框）
    checkbox = event.GetEventObject()
    # 获取复选框的标签和选中状态
    label = checkbox.GetLabel()
    is_checked = checkbox.IsChecked()
    print(f"复选框 '{label}' 的状态：{'选中' if is_checked else '未选中'}")


def on_radio_button(event):
    # 获取事件对象（即单选框）
    radio_button = event.GetEventObject()
    # 获取单选框的标签
    label = radio_button.GetLabel()
    print(f"选中的单选框是：{label}")

#  创建下拉选  不可自定义
select = wx.Choice(panel, pos=(20, 20), choices=["选项1", "选项2", "选项3"])
# 绑定事件
select.Bind(wx.EVT_CHOICE, choice)

# 创建下拉框  可自定义
comboBox = wx.ComboBox(panel, pos=(140, 20), choices=["选项1", "选项2", "选项3"])
# 绑定事件
comboBox.Bind(wx.EVT_COMBOBOX, comboBox_fn)

# 创建复选框
checkBox1 = wx.CheckBox(panel, label="选项1", pos=(50, 70))
checkBox2 = wx.CheckBox(panel, label="选项2", pos=(170, 70))
checkBox3 = wx.CheckBox(panel, label="选项3", pos=(290, 70))
# 绑定事件
checkBox1.Bind(wx.EVT_CHECKBOX, on_checkbox)

"""
创建单选框
第一个单选框需要指定样式 wx.RB_GROUP，表示开始一个新的组
后续的单选框会自动加入到这个组中
"""

radioButton1 = wx.RadioButton(panel, label="选项1", pos=(50, 90), style=wx.RB_GROUP)
radioButton2 = wx.RadioButton(panel, label="选项2", pos=(150, 90))
#事件绑定
# 绑定事件
radioButton1.Bind(wx.EVT_RADIOBUTTON, on_radio_button)
radioButton2.Bind(wx.EVT_RADIOBUTTON, on_radio_button)
```

![图片](Python高级笔记_assets/ch4_img15.png)

##### 类方式创建窗口

**客户端**

```python
import wx
import socket
import threading

class Client(wx.Frame):
    def __init__(self):
        # 初始化socket
        self.clientSocket = None
        # 连接状态
        self.is_connected = False
        # 实例属性
        self.name = '2511'  # 客户端名字
        # 界面布局
        wx.Frame.__init__(self, None, title=self.name + '聊天室客户端', size=(480, 680))
        self.panel = wx.Panel(self)
        """
            创建面板按钮
                1.加入聊天室
                2.离开聊天室
                3.清空信息
                4.发送信息
        """
        # 加入聊天室
        self.connect_button = wx.Button(self.panel, label='加入聊天室', pos=(10, 10), size=(200, 40))
        # 离开聊天室
        self.dis_connect_button = wx.Button(self.panel, label='离开聊天室', pos=(250, 10), size=(200, 40))
        # 清空信息
        self.clear_button = wx.Button(self.panel, label='清空消息', pos=(10, 580), size=(200, 40))
        # 发送信息
        self.send_button = wx.Button(self.panel, label='发送消息', pos=(220, 580), size=(200, 40))
        """
            绑定文本框
            1. 消息显示
            2.文本输入
        """
        # 创建聊天内容文本框  多行信息和只读
        self.text = wx.TextCtrl(self.panel, size=(440, 400), pos=(10, 60), style=wx.TE_READONLY | wx.TE_MULTILINE)
        # 创建输入内容文本框
        self.input_text = wx.TextCtrl(self.panel, size=(440, 80), pos=(10, 470), style=wx.TE_MULTILINE)
        """
            按钮事件绑定
        """
        self.connect_button.Bind(wx.EVT_BUTTON, self.add_chat_fn)
        self.dis_connect_button.Bind(wx.EVT_BUTTON, self.exit_chat_fn)
        self.clear_button.Bind(wx.EVT_BUTTON, self.clear_message_fn)
        self.send_button.Bind(wx.EVT_BUTTON, self.send_message_fn)
        
    #  加入
    def add_chat_fn(self, event):
        if not self.is_connected:
            self.text.AppendText('开始连接服务器...\n')
            try:
                self.clientSocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                self.clientSocket.connect(('127.0.0.1', 8080))
                self.text.AppendText('服务器连接成功，开始聊天吧...\n')
                self.is_connected = True
                # 【知识点】daemon=True 确保主程序退出时子线程自动终止
                threading.Thread(target=self.recive_message, daemon=True).start()
            except ConnectionRefusedError:
                self.text.AppendText('服务器异常,连接失败，请重试...\n')
    
    # 退出
    def exit_chat_fn(self, event):
        # 释放资源
        if self.clientSocket:
            self.clientSocket.close()
        # 改变客户端状态
        self.is_connected = False
        self.text.AppendText('已离开聊天室\n')
    
    # 清空
    def clear_message_fn(self, event):
        self.text.Clear()
    
    # 发送
    def send_message_fn(self, event):
        if self.is_connected and self.clientSocket:
            message = self.input_text.GetValue()
            if message:
                try:
                    self.clientSocket.send(message.encode('utf-8'))
                    self.input_text.Clear()
                except Exception as e:
                    self.text.AppendText(f'发送失败: {e}\n')
    
    # 接收服务器端的信息
    def recive_message(self):
        """【知识点】在子线程中接收数据，使用 wx.CallAfter 安全更新 UI"""
        try:
            while self.is_connected:
                message = self.clientSocket.recv(1024).decode('utf-8')
                if not message:  # 服务端正常关闭连接
                    break
                # 【重点】wx.CallAfter 将 UI 更新操作交由主线程执行，避免线程安全问题
                wx.CallAfter(self.text.AppendText, message + '\n')
        except ConnectionAbortedError:
            wx.CallAfter(self.text.AppendText, '连接已断开...\n')
        except ConnectionResetError:
            wx.CallAfter(self.text.AppendText, '服务器强制关闭了连接...\n')
        except Exception as e:
            wx.CallAfter(self.text.AppendText, f'接收异常: {e}\n')
        finally:
            self.is_connected = False
            if self.clientSocket:
                self.clientSocket.close()

if __name__ == '__main__':
    # 创建应用程序对象
    app = wx.App()
    # 创建客户端窗口
    frame = Client()
    # 显示客户端窗口
    frame.Show()
    # 一直运行程序
    app.MainLoop()
```

![图片](Python高级笔记_assets/ch4_img16.png)

**服务端**

```python
class ServerFrame(wx.Frame):

    def __init__(self):
        # 初始化socket
        self.server_state = False
        self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.server_socket.bind(('127.0.0.1', 8080))
        self.server_socket.listen(5)
        self.all_client_socket = []
        """
         界面布局
        """
        wx.Frame.__init__(self, None, title='多人聊天室服务端', pos=(0, 50), size=(450, 550))
        self.panel = wx.Panel(self)

        """
            按钮布局
        """
        # 启动服务
        self.start_server_btn = wx.Button(self.panel, label='启动服务端', size=(200, 40), pos=(10, 10))
        # 保存聊天记录
        self.save_text_btn = wx.Button(self.panel, label='保存记录', size=(200, 40), pos=(220, 10))
        # 聊天内容文本框
        self.text = wx.TextCtrl(self.panel, size=(400, 400), pos=(10, 60), style=wx.TE_MULTILINE | wx.TE_READONLY)

        """
            绑定按钮事件
        """
        self.start_server_btn.Bind(wx.EVT_BUTTON, self.start_server_fn)
        self.save_message_btn.Bind(wx.EVT_BUTTON, self.save_message_fn)

    #  启动服务端
    def start_server_fn(self, event):
        if not self.server_state:
            self.server_state = True
            self.show_info.AppendText('服务器启动,等待客户端连接...\n')
            threading.Thread(target=self.connect_accept).start()

    # 处理用户的连接任务
    def connect_accept(self):
        while self.server_state:
            client_socket, client_address = self.server_socket.accept()
            # 存储客户端socket
            self.all_client_socket.append(client_socket)
            str_message = f'【系统通知】{client_address[0]}:{client_address[1]}进入聊天室...\n'
            self.show_info.AppendText(str_message)
            # 群发进群通知
            for one_socket in self.all_client_socket:
                one_socket.send(str_message.encode('utf-8'))
            # 创建线程处理客户端任务
            threading.Thread(target=self.handle_all_client, args=(client_socket, client_address)).start()
    
    #  处理客户端会话
    def handle_all_client(self, client_socket, client_address):
        try:
            while self.server_state:
                message = client_socket.recv(1024).decode('utf-8')
                str_message = f'【{client_address[0]}:{client_address[1]}】: {message}\n'
                self.show_info.AppendText(str_message)
                # 群发消息
                for one_socket in self.all_client_socket:
                    one_socket.send(str_message.encode('utf-8'))
        except ConnectionResetError:
            # 剔除下线用户
            self.all_client_socket.remove(client_socket)
            str_message = f'【系统通知】{client_address[0]}:{client_address[1]}退出聊天室...\n'
            # 显示用户退出聊天室信息
            self.show_info.AppendText(str_message)
            #  群发用户退出消息
            for one_socket in self.all_client_socket:
                one_socket.send(str_message.encode('utf-8'))
    
    # 保存聊天记录     将聊天的信息写入本地文件  做好每次保存信息前先记录一次保存时间
    def save_message_fn(self, event):
        with open('message.txt', 'a+', encoding='utf-8') as f:
            time_str = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())
            f.write(f'*******************{time_str}*************************\n')
            f.write(self.show_info.GetValue())


# 程序入口
if __name__ == '__main__':
    app = wx.App()
    server = ServerFrame()
    server.Show()
    # 循环展示
    app.MainLoop()
```

![图片](Python高级笔记_assets/ch4_img17.png)

---

#### 总结:

> 💡 ---
>
> #### 📚 总结与扩展
>
> ##### 核心知识点回顾
>
> | 知识点 | 关键要点 | 进阶方向 |
> | --- | --- | --- |
> | **TCP 与 UDP** | TCP 面向连接、可靠；UDP 无连接、不可靠但高效 | 视频会议/游戏通常用 UDP + 上层纠错 |
> | **阻塞与非阻塞** | 默认<br>`recv`<br>/<br>`accept`<br>是阻塞的，可设置<br>`setblocking(False)`<br>变为非阻塞 | 非阻塞 IO 通常配合<br>`select`<br>/<br>`poll`<br>/<br>`epoll`<br>使用 |
> | **粘包与拆包** | TCP 流式传输导致的经典问题 | 常见解决：定长消息、分隔符、长度字段（Header+Body） |
> | **并发模型** | 多线程（适合 IO 密集型）、多进程（适合 CPU 密集型） | 高并发场景建议使用<br>`asyncio`<br>+ 协程 |
> | **GUI 线程安全** | 子线程不能直接操作 UI 控件，需用<br>`wx.CallAfter`<br>或类似机制 | 更复杂的场景可使用<br>`queue.Queue`<br>进行线程间通信 |
>
> ##### 调试小技巧
>
> 1. **观察 TCP 连接状态**：在命令行运行 `netstat -an`，可以看到 `LISTEN`、`ESTABLISHED`、`TIME_WAIT` 等状态，帮助理解三次握手和四次挥手。
> 1. **抓包分析**：使用 Wireshark 或 Fiddler 抓取本地 127.0.0.1:8080 的流量，直观看到 TCP 包的交互过程。
> 1. **异常处理**：网络编程中，异常是常态（对方突然断网、超时等），务必在关键位置添加 `try-except`。
>
> ##### 扩展学习建议
>
> - 学习使用 `selectors` 模块实现 IO 多路复用，替代多线程模型
> - 了解 HTTP 协议底层是如何基于 Socket 实现的
> - 学习 WebSocket 协议，实现真正的双向实时通信

---

### 正则表达式

正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。在 Python 中，使用 **re** 模块来处理正则表达式。re 模块提供了一组函数，允许你在字符串中进行模式匹配、搜索和替换操作，接下来主要介绍Python中常用的正则表达式处理函数

> 💡 ##### 为什么要学习正则表达式？
>
> | 场景 | 没有正则表达式 | 有正则表达式 |
> | --- | --- | --- |
> | 验证邮箱 | 几十行字符串判断代码 | `r'\w+@\w+\.\w+'` |
> | 提取手机号 | 遍历字符判断 | `r'1[3-9]\d{9}'` |
> | 替换敏感词 | 循环替换 | `re.sub(r'敏感词', '***', text)` |
> | 解析日志 | 复杂的 split 操作 | `re.findall(r'\d+\.\d+\.\d+\.\d+', log)` |
>
> > 💡 **一句话总结**：正则表达式是用一种"模式"来描述你想要的字符串，让计算机帮你快速找到它们。

**示例**

```
import re  #正则模块的包

str_info = 'zhangsan lisi wangwu zhaoliu'
# 【知识点】普通的字符串查找方式（不涉及正则）
print(str_info.find('lisi'))      # 输出：9（lisi 的起始索引）
print('lisi' in str_info)         # 输出：True

str_info = 'zhangsan 11 lisi 22 wangwu 33 zhaoliu'
# 【知识点】\d 表示数字，+ 表示至少1个(尽可能多的)，re.findall 提取所有匹配项
import re
result = re.findall(r'\d+', str_info)  # \d 表示数字，+ 表示至少1个
print(result)  # 输出：['11', '22', '33'],不加+号,拿一个就结束拿下一个
```

re.findall  查找全部

re.sub  替换

re.findall(r' ',字符串)

#### 正则表达式实例

**正则语句要用r'  '   包起来**

**字符匹配**

| 实例 | 描述 |
| --- | --- |
| python | 精准匹配 "python". |

**字符类**

一个中括号只代表一个字符

| 实例 | 描述 |
| --- | --- |
| [Pp]ython  一个中括号代表一个字符 | 匹配 "Python" 或 "python" |
| rub[ye] | 匹配 "ruby" 或 "rube" |
| [aeiou] | 匹配中括号内的任意一个字母 |
| [0-9] | 匹配任何数字。类似于 [0123456789] |
| [a-z] | 匹配任何小写字母 |
| [A-Z] | 匹配任何A到Z中的大写字母 |
| [a-zA-Z0-9] | 匹配任何字母及数字 |
| [^aeiou]    (^在括号内是取反，在外是行首) | 除了aeiou字母以外的所有字符 |
| [^0-9] | 匹配除了数字外的字符 |

**特殊字符类**

**单独一个也只表示一个字符**

| 实例 | 描述 |
| --- | --- |
| .   点表示单个字符,除了\\n | 匹配除 "\\n" 之外的任何单个字符。要匹配包括 '\\n' 在内的任何字符，请使用像 '[.\\n]' 的模式。 |
| \\d | 匹配一个数字字符。等价于 [0-9]。 |
| \\D | 匹配一个非数字字符。等价于 [^0-9]。<br>(在内是取反，在外是行首) |
| \\s | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \\f\\n\\r\\t\\v]。 |
| \\S | 匹配任何非空白字符。等价于 [^ \\f\\n\\r\\t\\v]。 |
| \\w | 匹配包括下划线的任何单词数字字符。等价于'[A-Za-z0-9\_]'。 |
| \\W | 匹配任何非单词数字字符。等价于 '[^A-Za-z0-9\_]'。 |

#### 通配符   .  点

**元字符  通配符   .  匹配一个除换行符之外\\n的所有符号,注意一个点匹配一个**

```
str_info = 'apple ape agree age amaze animate advertise a\ne'
# result = re.findall(r'apple', str_info)

# 【知识点1】. 匹配除 \n 外的任意单个字符
result = re.findall(r'a.e', str_info)    # 寻找  'a + 任意1个字符 + e'
print(result)  # 输出：['ape', 'age', 'aze', 'ate']    

# 【知识点2】.. 匹配任意2个字符
result = re.findall(r'a..e', str_info)   # a + 任意2个字符 + e
print(result)  # 输出：['agre','adve]

# 【知识点3】... 匹配任意3个字符
result = re.findall(r'a...e', str_info)  # a + 任意3个字符 + e
print(result)  # 输出：['apple', 'agree', 'amaze']
```

#### 字符集 []

一个中括号只代表一个字符

**匹配一个中括号中出现的任意原子字符**

```python
str_info = 'apple ape agree age amaze animate advertise a\ne  a@e a$e a%e a0e a1e'
result = re.findall(r'a.e', str_info)
print(result)   #['ape', 'age', 'aze', 'ate', 'a@e', 'a$e', 'a%e', 'a0e', 'a1e']
# [] 其中的一个字符
result = re.findall(r'a[pg]e', str_info)   #一个[]只表示一个字符
print(result)   #['ape', 'age']
# 字符
result = re.findall(r'a[a-zA-Z]e', str_info)
print(result)   #['ape', 'age', 'aze', 'ate']
# 数字
result = re.findall(r'a[0-9]e', str_info)
print(result)  #['a0e', 'a1e']
# 非数字
result = re.findall(r'a[^0-9]e', str_info)
print(result)  #['ape', 'age', 'aze', 'ate', 'a\ne', 'a@e', 'a$e', 'a%e']
# 筛选特殊符号
result = re.findall(r'a[^0-9a-zA-z]e', str_info)
print(result)   #['a\ne', 'a@e', 'a$e', 'a%e']
```

#### 重复元字符

常与[ ]字符集以及\\d等特殊字符组合使用

{}  ：范围,数量范围贪婪符号，指定左边原子的数量范围，有{n},{n,}{,m}{n,m}  其中n和m必须是非负整数

\*  ： 出现零次或无穷次,尽可能多的  等同于{0，}

+  ：出现一次或无穷次,尽可能多的 等同于{1，}

?  ：出现一次或零次 {0,1}

> 💡 量词用来指定**前面的字符或字符集出现多少次**：
>
> 常与[ ]字符集以及\\d等特殊字符组合使用 ,也有的某个字符后面会跟一个,仅对这一个字符有效
>
> 如:
>
> text = "this is a url http://www.baidu.com"
>
> pattern = r"https?://www.[a-z0-9]+.com"  #这个是s?,表示0或1个s,http和https都能取到
>
> | 量词 | 含义 | 等价写法 | 示例 |
> | --- | --- | --- | --- |
> | `{n}` | 恰好出现 n 次,n个点 | — | `\d{3}`<br>匹配 3 位数字<br>等于\\d\\d\\d |
> | `{n,}` | 至少出现 n 次,起码n个点 | — | `\d{2,}`<br>匹配 2 位及以上数字 |
> | `{n,m}` | 出现 n 到 m 次 | — | `\d{2,4}`<br>匹配 2-4 位数字 |
> | `{0,}` | 出现 0 次或多次 | `*` | `a*`<br>匹配 ""、"a"、"aa"... |
> | `{1,}` | 出现 1 次或多次 | `+` | `a+`<br>匹配 "a"、"aa"..." |
> | `{0,1}` | 出现 0 次或 1 次 | `?` | `a?`<br>匹配 "" 或 "a" |
>
> ###### 默认贪婪模式 vs 非贪婪模式
>
> **【重要知识点】**
>
> - **贪婪模式**（默认）：量词会尽可能多地匹配字符，如 `{2,3}` 优先匹配 3 个
> - **非贪婪模式**（最后加个`?` 后缀）：量词会尽可能少地匹配字符，如 `{2,3}?` 优先匹配 2 个
> - .\*（贪婪）：吃进去就绝不吐出来，匹配最长的。
> - .\*?（非贪婪/懒惰）：中间可以有无数个,但是不贪,能不吃就不吃，匹配最短的。
>
> ![图片](Python高级笔记_assets/ch4_img18.png)
>
> ![图片](Python高级笔记_assets/ch4_img19.png)

```python
str_info = 'aeeee apple aPe agree age amaze animate advertise a\ne apge  a@e a$e a%e a0e a2e'
# {} 左边元字符重复的次数
result = re.findall(r'a...e', str_info)  #['aeeee', 'apple', 'agree', 'amaze']
result = re.findall(r'a.{3}e', str_info) #同样的

# 1-3字符  默认贪婪模式  按照最大范围匹配
result = re.findall(r'a.{2,3}e', str_info)  #['aeeee', 'apple', 'agree', 'amaze', 'adve', 'apge']
# ? 取消贪婪匹配 按照最小范围优先匹配
result = re.findall(r'a.{2,3}?e', str_info) #['aeee', 'apple', 'agre', 'amaze', 'adve', 'apge']

# {2,} 正无穷
result = re.findall(r'a.{2,}e', str_info)  #贪婪取值
# print(result)  #['aeeee apple aPe agree age amaze animate advertise', 'apge  a@e a$e a%e a0e a2e']
# re.S .可以匹配\n
result = re.findall(r'a.{2,}e', str_info,re.S) #可以匹配\n
# print(result) #['aeeee apple aPe agree age amaze animate advertise a\ne apge  a@e a$e a%e a0e a2e']
result = re.findall(r'a[a-z]{2,3}e', str_info) #中间是2-3位的小写字母
# print(result)  #['aeeee', 'apple', 'agree', 'amaze', 'adve', 'apge'
result = re.findall(r'a[^a-z\n]{2,3}e', str_info) #匹配任何不是小写字母且不是换行符的2-3位字符
# print(result) #[],没有
result = re.findall(r'a[0-9]{1,3}e', str_info) #中间是1-3位的数字
# print(result) #['a0e', 'a2e']

# * 的使用

str_info = 'ae aeeee apple aPe agree age amaze animate advertise a\ne apge  a@e a$e a%e a0e a2e'
result = re.findall(r'a.{0,3}e', str_info)  #贪心,这个太多了
# print(result) #['ae ae', 'apple', 'aPe', 'agree', 'age', 'amaze', 'ate', 'adve', 'apge', 'a@e', 'a$e', 'a%e', 'a0e', 'a2e']
result = re.findall(r'a.{0,3}?e', str_info)
#print(result)  #['ae', 'ae', 'apple', 'aPe', 'agre', 'age', 'amaze', 'ate', 'adve', 'apge', 'a@e', 'a$e', 'a%e', 'a0e', 'a2e']
result = re.findall(r'a.{0,}e', str_info)  #贪心,每次都往多了取,一直到取不了为止
# print(result) #['ae aeeee apple aPe agree age amaze animate advertise', 'apge  a@e a$e a%e a0e a2e']
result = re.findall(r'a.{0,}?e', str_info)
print(result)
result = re.findall(r'a.*e', str_info)  #主要就是\n识别不了停顿一下
print(result)
result = re.findall(r'a.*?e', str_info)
print(result)

# +
result = re.findall(r'a.+e', str_info) #+是ae之间起码得有一个,*是可以没有,?还是一样
print(result)
result = re.findall(r'a.+?e', str_info) 
print(result)
```

#### 开始和结束匹配

^ 开始边界符号,匹配一行开头位置**(在括号内是取反，没有括号包着是行首)**

$  结束边界符号,匹配一行结束位置

找行首的内容,行尾的内容

```python
import re

text = "甲 乙 丙\n丁 戊 己"

# 1. 没加 ^ 和 $，找所有字
print(re.findall(r'.', text))  
# 输出：['甲','乙','丙','丁','戊','己']（不含空格和换行，只找字）

# 2. 默认模式（不加 re.M）：找“行首的字”
print(re.findall(r'^.', text))  
# 输出：['甲']  → 因为整段文字的绝对开头只有“甲”的前面，所以只找到甲！

# 3. 多行模式（加 re.M）：找“每一行行首的字”
print(re.findall(r'^.', text, re.M))  
# 输出：['甲', '丁']  → 因为换行符把丁变成了第二行的行首！
```

```python
text = "aaa\nbbb"  # 中间有换行 
print(text)
# 找以 'a' 开头的  #要满足行首是a,不然也找不到
print(re.findall(r'^a', text))  # 输出: ['a'] （只匹配第一行的开头）

# 找以 'b' 结尾的
print(re.findall(r'b$', text))  # 输出: ['b'] （匹配第二行的 b，因为它在字符串末尾）
```

```python
# url 地址匹配
path = 'blog/2025/12/xxx'

reg = '^blog/[0-9]{4}/[0-9]{1,2}'

ret = re.findall(reg, path)
print(ret)  #['blog/2025/12']
```

#### 正则常用函数

##### re.search(pattern, text)

- 功能：扫描整个字符串，返回**第一个**符合正则表达式***的匹配对象***。如果没有找到匹配项，则返回 None。

```python
import re

text = "this is a url http://www.baidu.com"
pattern = r"https?://www.[a-z0-9]+.com"  #这个是s?,表示0或1个s,http和https都能取到

match = re.search(pattern, text)
if match:
    print("Found url:", match.group())  # 输出：Found url: http://www.baidu.com
else:
    print("No email found")
```

在这个例子中，re.search() 用于查找字符串中第一个符合电子邮件格式的部分

##### re.match(pattern, text)

- 功能：**从字符串的起始位置**开始匹配正则表达式，如果起始位置匹配成功，则**返回匹配对象**，否则返回 None。

```python
import re

text = "12345 is a number"
pattern = r"\d+"

match = re.match(pattern, text)
if match:
    print("Matched at the beginning:", match.group())  # 输出：Matched at the beginning: 12345
else:
    print("No match at the beginning")
```

如果将 text 改为 "is a number 12345"，re.match() 将返回 None，因为它只从字符串开头匹配。

##### re.findall(pattern, text)

- 功能：返回字符串中所有符合正则表达式的非重叠匹配项，结果<u>***以列表形式返回***</u>。

```python
import re
text = "The numbers are 123, 456, and 789"
pattern = r"\d+"

matches = re.findall(pattern, text)
print(matches)  # 输出：['123', '456', '789']
```

这个函数非常适合用于提取字符串中所有符合某种模式的内容。

##### re.sub(pattern, replacement, text,count)

**功能**：将字符串中所有符合正则表达式 pattern 的部分替换为 repl，count 参数用于指定最大替换次数，默认为 0，表示替换所有匹配项

```python
import re

text = "I love apples, apples are great"
pattern = r"apples"
replacement = "oranges"

new_text = re.sub(pattern, replacement, text)
print(new_text)  # 输出：I love oranges, oranges are great
```

如果指定 count=1，则只会替换第一个匹配的 "apples"

##### re.split(pattern, text,maxsplit=0, flags=0)

**功能**：根据正则表达式 pattern 将字符串分割为多个子字符串，返回一个列表。maxsplit 参数用于指定最大分割次数，默认为 0，表示不限制分割次数

```python
import re

text = "one,two,three,four"
pattern = r","

result = re.split(pattern, text)
print(result)  # 输出：['one', 'two', 'three', 'four']
```

如果指定 maxsplit=1，则只会分割第一个逗号，结果为 ['one', 'two,three,four']

---

**作业**：

1.编写一个正则表达式，提取字符串中所有以大写字母开头的单词

```python
import re

pattern = r"\b[A-Z]\w*\b"
text = "Hello world, This is a Python Example."
matches = re.findall(pattern, text)
print(matches)
```

2.编写一个正则表达式，匹配一个有效的电子邮件地址

```python
import re

pattern = r"[a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z0-9.]+"
text = "Contact us at support@example.com or info@test-domain.org."
matches = re.findall(pattern, text)
print(matches)
```

3.编写一个正则表达式，提取字符串中所有以“#”开头的标签（例如“#python”或“#regex”）

```python
import re

pattern = r"#\w+"
text = "I love #python and #regex. They are #awesome!"
matches = re.findall(pattern, text)
print(matches)
```

4.编写一个正则表达式，匹配一个有效的手机号码

```python
import re

# 正则表达式
pattern = r"1[3-9]\d{9}"

# 测试文本
text = """
我的手机号码是13812345678，欢迎联系！
另一个号码是15987654321。
无效号码：12345678901和9876543210。
"""

# 查找匹配的手机号码
matches = re.findall(pattern, text)
print("匹配的手机号码：", matches)
```

5.编写一个正则表达式，匹配并提取字符串中符合“YYYY-MM-DD”格式的日期

```python
import re

pattern = r"\b\d{4}-\d{2}-\d{2}\b"
text = "The event will take place on 2025-10-01 and end on 2025-10-10."
matches = re.findall(pattern, text)
print(matches)
```

6.编写一个正则表达式，匹配字符串中表示数字范围的部分，格式为“最小值-最大值”（例如“10-20”或“100-200”）。

```python
import re

pattern = r"\b\d+-\d+\b"
text = "The range is 10-20 or 100-200, but not 10 or 200-300."
matches = re.findall(pattern, text)
print(matches)
```

7.编写一个正则表达式，匹配并提取字符串中符合IPv4地址格式的IP地址 192.168.1.1

```python
import re

pattern = r"\b(?:\d{1,3}\.){3}\d{1,3}\b"
text = "The server IP is 192.168.1.1 and the gateway is 10.0.0.1."
matches = re.findall(pattern, text)
print(matches)
```

8.编写一个正则表达式，匹配并提取字符串中的HTML标签（例如<div>、<a href="...">等）

```python
import re

pattern = r"<[^>]+>"
text = "<html><head><title>Example</title></head><body><p>Hello, <a href='https://example.com'>world</a>!</p></body></html>"
matches = re.findall(pattern, text)
print(matches)
```

## 第5章 Python操作MySQL数据库

### 标准数据库接口

---

Python 标准数据库接口为 Python DB-API，Python DB-API为开发人员提供了数据库应用编程接口。Python 数据库接口支持非常多的数据库，你可以选择适合你项目的数据库：

- MySQL
- SQL Server
- Oracle
- Sybase

不同的数据库你需要下载不同的DB API模块，例如你需要访问Oracle数据库和Mysql数据，你需要下载Oracle和MySQL数据库模块。DB-API 是一个规范. 它定义了一系列必须的对象和数据库存取方式, 以便为各种各样的底层数据库系统和多种多样的数据库接口程序提供一致的访问接口 。Python的DB-API，为大多数的数据库实现了接口，使用它连接各数据库后，就可以用相同的方式操作各数据库。

---

**Python DB-API使用流程：**

- 引入 API 模块
- 获取与数据库的连接
- 执行SQL语句
- 关闭数据库连接

---

### 常见数据库的接口

#### MySQLdb

MySQLdb 是用于Python链接Mysql数据库的接口，它实现了 Python 数据库 API 规范 V2.0，基于 MySQL C API 上建立的,已经不在维护不推荐使用

#### mysqlclient

mysqlclient 是 MySQLdb 的一个分支，它在 Python 3 中仍然被维护，并且提供了与 MySQLdb 类似的 API

**安装方式**

```python
pip install mysqlclient
```

**获取连接**

```python
import MySQLdb

db_connection = MySQLdb.connect(
    host='localhost',
    port=3306,
    database='vue',
    user='root',
    passwd='root',
    charset='utf8'
)
print(db_connection)
```

#### PyMySQL

PyMySQL 是一个纯 Python 实现的 MySQL 客户端库，不需要安装 MySQL 的 C 扩展。它也可以作为 MySQLdb 的替代品。

**安装方式**

```python
pip install pymysql
```

**获取连接：**

```python
import pymysql

db_connection = pymysql.connect(
    host='localhost',
    port=3306,
    database='vue',
    user='root',
    passwd='root',
    charset='utf8'
)
print(db_connection)
```

#### cx_Oracle

在 Python 中连接 Oracle 数据库，通常使用 cx\_Oracle 库，需要注意python解释器的版本，高版本存在兼容问题，可使用python3.9

安装：

```python
pip install cx_Oracle
```

获取连接

```python
import  cx_Oracle

connection = cx_Oracle.connect('SCOTT', 'scott', '127.0.0.1:1521/ORCL')
# 创建游标  发送sql语句 接收执行结果
cursor = connection.cursor()

# 执行查询
query = "SELECT * FROM emp"  # 查询语句
cursor.execute(query)

# 获取查询结果
rows = cursor.fetchall()
for row in rows:
    print(row)
```

---

### 接口操作

#### 创建数据表

```python
# 获取游标对象
curser = db_connection.cursor()
# 执行sql
str_sql = "drop table if exists user"
curser.execute(str_sql)

str_sql = 'create table user ('\
          'id INT AUTO_INCREMENT PRIMARY KEY,'\
          'username VARCHAR(20) UNIQUE KEY,'\
          'password VARCHAR(20) ,'\
          'create_time datetime)character set utf8'
curser.execute(str_sql)
# 关闭连接
curser.close()
db_connection.close()
```

#### 新增数据

```python
# 获取游标对象
curser = db_connection.cursor()
# 执行sql
str_sql = "insert into user(username,password,create_time) values('admin','123456','2019-08-01')"
result = curser.execute(str_sql)
print('影响行数:',result)
# 手动提交事务  否则数据不会持久化
db_connection.commit()
# 关闭连接
curser.close()
db_connection.close()


# 批量插入数据
sql = "INSERT INTO user (username, password) VALUES (%s, %s)"
data = [
    ('Alice', '111111'),
    ('Bob', '111111'),
    ('Charlie', '1111111')
]
result = cur.executemany(sql, data)
```

#### 查询数据

获取所有数据

```python
# 获取游标对象
curser = db_connection.cursor()
# 执行sql
str_sql = "select * from user"
# 返回影响的行数
rows = curser.execute(str_sql)
print(rows)
# 获取结果
rows_data = curser.fetchall()
for row in rows_data:
    print(row)
# 关闭连接
curser.close()
db_connection.close()
```

获取指定数据

```python
# 获取游标对象
curser = db_connection.cursor()
# 执行sql
str_sql = "select * from user"
# 返回影响的行数
rows = curser.execute(str_sql)
print(rows)
# 获取5条结果
rows_data = curser.fetchmany(5)
for row in rows_data:
    print(row)
# 关闭连接
curser.close()
db_connection.close()
```

获取一条数据

```python
# 获取游标对象
#curser = db_connection.cursor(pymysql.cursors.DictCursor)  # 数据以字典的形式返回
curser = db_connection.cursor()
# 执行sql
str_sql = "select * from user"
# 返回影响的行数
rows = curser.execute(str_sql)
print(rows)
# 获取3条结果
rows_data = curser.fetchone()
print(rows_data)
rows_data = curser.fetchone()
print(rows_data)
# 关闭连接
curser.close()
db_connection.close()
```

**调整游标**

```python
# 获取游标对象
curser = db_connection.cursor()
# 执行sql
str_sql = "select * from user"
# 返回影响的行数
rows = curser.execute(str_sql)
print(rows)
# 绝对位置  从第一条开始滚动
curser.scroll(2,mode='absolute')
print(curser.fetchone())
# 相对位置  从当前位置开始滚动
curser.scroll(2,mode='relative')
print(curser.fetchone())

# 关闭连接
curser.close()
db_connection.close()
```

#### 删除数据

```python
# 获取游标对象
curser = db_connection.cursor()
# 执行sql
str_sql = "delete  from user where id in (4,5,6)"
# 返回影响的行数
rows = curser.execute(str_sql)
print(rows)
# 手动提交事务
db_connection.commit()
# 关闭连接
curser.close()
db_connection.close()
```

#### 修改数据

```python
# 获取游标对象
curser = db_connection.cursor()
# 执行sql
str_sql = "update user set username = 'root',password = '666666' where id = 7"
# 返回影响的行数
rows = curser.execute(str_sql)
print(rows)
# 手动提交事务
db_connection.commit()
# 关闭连接
curser.close()
db_connection.close()
```

**案例:**

连接mysql数据库，实现用户的登录和注册操作

```python
# 获取游标对象
curser = db_connection.cursor()
username = input('username:')
password = input('password:')
str_sql = "select * from user where username = '"+username+"' and password = '"+password+"'"
print(str_sql)
rows = curser.execute(str_sql)
if rows:
    print('login success')
else:
    print('username or password error')
curser.close()
db_connection.close()
```

注：以上代码存在sql注入问题

![图片](Python高级笔记_assets/ch5_img01.png)

解决方式：

```python
# 获取游标对象
curser = db_connection.cursor()
username = input('username:')
password = input('password:')
str_sql = "select * from user where username = %s and password = %s"
print(str_sql)
rows = curser.execute(str_sql, (username, password))
if rows:
    print('login success')
else:
    print('username or password error')
curser.close()
db_connection.close()
```

![图片](Python高级笔记_assets/ch5_img02.png)

---

### 数据库连接池

在 Python 中使用连接池（Connection Pool）主要是为了优化数据库连接的性能、资源管理和提高系统的可扩展性。以下是使用连接池的几个主要原因：

**提高性能**

- 减少连接开销：每次打开和关闭数据库连接都需要消耗系统资源和时间。连接池通过复用已有的连接，避免了频繁地创建和销毁连接，从而显著减少了连接的开销。
- 快速响应：当应用程序需要与数据库交互时，连接池可以直接提供一个已建立的连接，而无需等待新的连接建立，从而提高了响应速度。

**资源管理**

- 限制连接数量：连接池可以限制同时打开的数据库连接数量，避免过多的连接占用过多的数据库资源，导致数据库服务器过载。
- 合理分配资源：通过连接池，可以更好地管理连接的生命周期，例如设置连接的最大空闲时间、最大连接数等，从而合理分配数据库资源。

**简化代码逻辑**

- 统一管理：使用连接池后，应用程序不需要手动管理数据库连接的创建、释放和销毁，连接池会自动处理这些操作，从而简化了代码逻辑。
- 减少错误：手动管理连接容易出现忘记关闭连接等问题，而连接池可以自动管理连接的生命周期，减少了因连接管理不当导致的错误。

**安装**

```python
pip install dbutils
```

**创建连接池**

```python
import pymysql
from dbutils.pooled_db import PooledDB

mysql_db_pool = PooledDB(
    host='127.0.0.1',
    port=3306,
    user='root',
    passwd='root',
    db='python2511',
    charset='utf8mb4',
    creator=pymysql,  # 连接数据库的模块
    maxconnections=5,  # 通常允许的最大连接数（默认值 0 或 None 表示任意数量的连接）
    mincached=2,  # 池中空闲连接的初始数量（默认值 0 表示启动时不建立任何连接）
    maxcached=3,  # 池中的最大空闲连接数（默认值 0 或 None 表示池大小不受限制）
    blocking=True  # 确定超过最大值时的行为（如果设置为 true，则阻止并等待，直到连接数减少，但默认情况下会报错）
)

conn = mysql_db_pool.connection()
curser = conn.cursor()
str_sql = "select * from user"
curser.execute(str_sql)
rows = curser.fetchall()
for row in rows:
    print(row)
```

---

### SQL工具类

```python
import pymysql
from dbutils.pooled_db import PooledDB


class DBHelper:
    # 初始化操作
    def __init__(self):
        # TODO  可以通过配置文件进行初始化
        self.pool = PooledDB(
            host='127.0.0.1',
            port=3306,
            user='root',
            passwd='root',
            db='python2511',
            charset='utf8mb4',
            creator=pymysql,  # 连接数据库的模块
            maxconnections=5,  # 通常允许的最大连接数（默认值 0 或 None 表示任意数量的连接）
            mincached=2,  # 池中空闲连接的初始数量（默认值 0 表示启动时不建立任何连接）
            maxcached=3,  # 池中的最大空闲连接数（默认值 0 或 None 表示池大小不受限制）
            blocking=True  # 确定超过最大值时的行为（如果设置为 true，则阻止并等待，直到连接数减少，但默认情况下会报错）
        )

    # 获取连接和游标
    def get_conn_curser(self):
        conn = self.pool.connection()
        curser = conn.cursor()
        return conn, curser

    # 释放资源
    def close_conn_curser(self, *args):
        for item in args:
            item.close()

    # 增删改
    def execute(self, sql, **kwargs):
        conn, cur = self.get_conn_curser()
        line_num = cur.execute(sql, kwargs)
        conn.commit()
        # 释放资源
        self.close_conn_curser(conn, cur)
        return line_num 

    # 获取一个
    def fetch_one(self, sql, **kwargs):
        conn, cur = self.get_conn_curser()
        cur.execute(sql, kwargs)
        result = cur.fetchone()
        self.close_conn_curser(conn, cur)
        return result

    # 获取所有
    def fetch_all(self, sql, **kwargs):
        conn, cur = self.get_conn_curser()
        cur.execute(sql, kwargs)
        result = cur.fetchall()
        self.close_conn_curser(conn, cur)
        return result


db = DBHelper()
```

**测试代码**

```python
row = db.fetch_one('select * from user where id = %(id)s', id=8)
print(row)

rows = db.fetch_all('select * from user where id in %(id)s  ', id=(7, 8))
print(rows)

line_num = db.execute('delete from user where id = %(id)s', id=7)
print(line_num)

line = db.execute('update user set username = %(username)s,password= %(password)s where id = %(id)s')
print(line)

str_sql = "insert into user values(null,%(username)s,%(password)s,%(create_time)s)"
line,last_id = db.execute(str_sql, username="root", password="111111", create_time="2025-05-16")
print(line,last_id)
```

---

**大作业:**

**实现一个用户信息管理系统，结合数据库实现用户信息的增删改查，要使用函数，面向对象的思想设计程序，同时要使用欢迎页让用户选择需要执行的操作**

**数据表结构如下**：

![图片](Python高级笔记_assets/ch5_img03.png)
