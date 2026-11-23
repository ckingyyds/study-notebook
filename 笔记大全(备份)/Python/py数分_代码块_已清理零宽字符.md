# [导论]()

| 工具           | 作用                          | 类比说明       |
| -------------- | ----------------------------- | -------------- |
| **Numpy**      | 高性能数值计算（矩阵/向量）   | 数据的"发动机" |
| **Pandas**     | 表格数据处理（类似高级Excel） | 数据的"手术刀" |
| **Matplotlib** | 数据可视化（绘图库）          | 数据的"翻译官" |

1. **数据收集**

**数据从哪里来？**公司数据库 | 公开数据集（如政府数据） | 手动爬取

2. **数据清洗（最重要！）**

**典型问题**：

缺失值（如Excel里的空单元格）

arnge错误数据（如年龄填成200岁）

格式混乱（日期写成“2023年1月1日”和“01/01/2023”混用）

3. **数据分析**

**常用方法**：

统计（平均值、最大值、比例）

分组对比（如男vs女用户的消费差异）

4. **数据可视化**

**一图胜千言**：折线图（趋势） | 柱状图（对比） | 散点图（相关性）

## 安装anaconda:集成开发环境

还是跟着下一遍吧

## jupyter notebook的使用:666到家了

Jupyter是一个开源的交互式计算环境，广泛应用于数据科学、机器学习、科
学研究等领域，主要组件有JupyterNotebook和JupyterLab。

Jupyter Lab作为JupyterNotebook的继承者，提供了更现代化和功能丰富的
界面。(不常用)    现在呢:真tm好用

JupyterLab的多文档界面、内置协作功能和扩展系统使其成为数据科学家和研究人员的首选。(你懂的,写一句运行一句太爽了)

有本地jupyter 和 pycharm集成jupyter(代码提示不错)



#### jupyter 快捷键(我的发,还有这玩意)

| esc         | 从输入模式退出到命令模式        |
| ----------- | ------------------------------- |
| a           | 在当前cell上面创建一个新的cell  |
| b           | 在当前cell 下面创建一个新的cell |
| dd  ,,   x  | 删除当前cell                    |
| m           | 切换到markdown模式              |
| y           | 切换到code模式                  |
| ctrl+回车   | 运行cell                        |
| shift +回车 | 运行当前cell并创建一个新的cell  |

#### 本地jupyter打开方式 , 这么多吗

1.anaconda点jupyter,

2.命令行输你懂的,

3.jupyter图标打开

#### 用法(还真有些不知道的)

数分用python3

点上面名字能命名文件(创建文件之后)

同一个笔记本里,全局变量是互通的

如果定义过变量的语句删了,但是之前运行过,那么这个变量还是存在的

如果想删这个变量,可以点kernel>>>>>reatart,相当于重启了一次文件,刷新文件状态,这时候删掉的变量真的删掉了,打印这个变量会报错

进入markdown模式可以给这行记笔记,你发现前面的数字没有了,#开头,ctrl+enter可以放大字体(好像是markdown语法,没学过)

多按esc退出输入模式



#### pycharm中使用jupyter

跟建python文件一样的,只不过建的是jupyter文件

快捷键一样的

也就提示多一些

那vscode应该也行

怪麻烦的,好恶心啊,还要弄别的

神经,markdown还他妈用不了

滚吧滚吧





# numpy,开始吧孩子

## 一.numpy介绍(数组矩阵啥的)

numpy是Python中科学计算的基础包。**它是一个Python库**

numpy的部分功能如下：

•     ndarray，一个具有矢量算术运算和复杂广播能力的快速且节省空间的多维数组。

•     用于对整组数据进行快速运算的标准数学函数（无需编写循环）。

•     用于读写磁盘数据的工具以及用于操作内存映射文件的工具。

•     线性代数、随机数生成以及傅里叶变换功能。

•     用于集成由C、C++、Fortran等语言编写的代码的API。



为什么需要Numpy？比python快

**输出示例**：

Python列表耗时:  0.1256s    NumPy数组耗时: 0.0023s  



## 二.numpy-ndarray的特性(嫩第额瑞)

ndarray是一个数据类型

.就是'的'的意思

Numpy数组(ndarray)的核心特性(对标python的列表)
多维性：支持0维（一个数字）、1维（向量:一列）、2维（矩阵）及更高维数组。
同质性：所有元素类型必须一致（通过dtype指定）(和python不同,py列表里的数据类型可以不同)。
高效性：基于连续内存块存储，支持向量化运算(查找效率更高)。

###### 导入numpy库

```python
import numpy as np   #导入numpy库,取别名(mysql有感觉吗),要用as取别名,会方便
```

#### 多维性

几维就是几层**中括号**

###### 创建0维数组(就是一个数字)

```python
arr = np.array(5)   #创建了一个0维的ndarray数组,名字叫arr
print(arr)    #打印arr
print(arr.ndim)    #打印arr的维度,用ndim这个关键字,结果是0
```

###### 创建1维数组(一个列表)

方法跟py的列表创建方法一样圆括号里面中括号,中括号里写数字

```python
arr = np.array([1,2,3,4])#创建了一个一维的ndarray数组,名字叫arr
print(arr) #打印arr
print(arr.ndim) #打印arr的维度,ndim这个关键字,结果是1
```

###### 创建2维数组(一个矩阵)

圆括号里中括号,中括号里再多组中括号

```python
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])#创建了一个2维的ndarray数组,名字叫arr
print(arr) #打印arr
print(arr.ndim) #打印arr的维度,ndim这个关键字,结果是2
```

还有更高维度数组,但是不常用



#### 同质性

数组里的数据类型得相同,不然都变成字符串了,我觉得叫同化性比较好,数字被转换成字符串了,整型被转换成浮点型,向上转换

#### 高效性

后面再讲

## 三.ndarray的属性

（假设 arr = np.array([[1, 2], [3, 4]])）

| 属性名称 | 通俗解释                                                 | 使用示例  | 输出结果          | 实际用途                                   |
| -------- | -------------------------------------------------------- | --------- | ----------------- | ------------------------------------------ |
| shape    | **数组的形状**：行数和列数（或更高维度的尺寸）(矩阵,n*n) | arr.shape | (2, 2)  (行x列)   | 查看或调整数组结构（如变形）。             |
| ndim     | **维度数量**：数组是几维的（1维、2维、3维等）。          | arr.ndim  | 2                 | 判断数组是向量、矩阵还是高维数据。         |
| size     | **总元素个数**：数组中所有元素的总数。                   | arr.size  | 4                 | 快速计算元素总量。                         |
| dtype    | **元素类型**：数组中元素的类型（整数、浮点数等）。       | arr.dtype | int64（或 int32） | 确保计算时类型一致（如避免整数除法问题）。 |

.就是'的'的意思,arr的shape,加个print打印,好理解

这他妈不是线性代数吗

**进阶属性（了解即可）**

| 属性名称 | 应用场景                   | 示例代码     | 输出结果               | 通俗解释                                                     |
| -------- | -------------------------- | ------------ | ---------------------- | ------------------------------------------------------------ |
| T        | 矩阵运算（如矩阵乘法）。   | arr.T        | [[1, 3], [2, 4]]       | **转置**：先将列表转换为 NumPy 数组，再使用 `.T`行变列，列变行。 |
| itemsize | 优化内存占用时参考。       | arr.itemsize | 8（int64 类型占8字节） | **单个元素占用的内存字节数**。                               |
| nbytes   | 处理大数据时监控内存消耗。 | arr.nbytes   | 32（4元素 × 8字节）    | **数组总内存占用量**：size * itemsize。                      |
| flags    | 高性能计算或底层内存操作。 | arr.flags    | C_CONTIGUOUS : True 等 | **内存存储方式**：是否连续存储（高级优化）。                 |



## 四.ndarray的创建

### 创建时的细节

***1.我们在创建的时候还可以指定很多东西,比如数据类型***

```python
list1=[[1,2,3],[2,3,4]]
arr = np.array(list1,dtype=np.int32)
arr = np.array(list1,dtype='i4')  #类型代码方法
arr = np.array(list1,dtype=int) #这三种指定方式都是正确的
```

***2.创建是可以设置形状,直接加.reshape(形状)***

比如你给的数据是一维的,但是可以指定形状使之成为二维数组

切记注意数字要对上

```python
arr = np.linspace(1, 100, 20, dtype=int).reshape(4,5)
print(arr) #本来是一维数组,但是制定了形状,变成二维
#同理
arr2 = np.array([1,2,3,4,5,6,7,8,9,0]).reshape(2,5)
print(arr2)
print(arr2.ndim)  #结果为2
```



### **创建方法汇总**

1. **基础构造**： 适用于手动构建小规模数组或复制已有数据。前面的手动创建方法

2. **预定义形状填充**： 用于快速初始化固定形状的数组（如全0占位、全1初始化）。

3. **基于数值范围生成**： 生成数值序列，常用于模拟时间序列、坐标网格等。

4. **特殊矩阵生成**： 数学运算专用（如线性代数中的单位矩阵）。

5. **随机数组生成**： 模拟实验数据、初始化神经网络权重等场景。

6. **高级构造方法**： 处理非结构化数据（如文件、字符串）或通过函数生成复杂数组。

7. 

#### 1.基础创建方法

```python
arr = np.array([[1,2,3],[4,5,6]])
#语法:数组名 = 库名.函数名(数组内容)
print(arr)
```

下面这种也行

```python
list1 = [1,2,3]
arr=np.array(list1)
print(arr)
```



#### 2.复制数组

复制别的数组,我们这里复制了arr这个数组,命名为arr1

```python
arr1 = np.copy(arr)    #适用于已经有一个数组
print(arr1)
```



#### 3.预定义形状数组

预定义形状数组创建出来默认的是浮点数

###### 全0数组  (zeros方法)

```python
arr = np.zeros((2,3))    #二行三列的全0数组
print(arr.dtype) #tips:创建出来的默认都是浮点数
arr = np.zeros((2,3),dtype=int) #创建时指定为整型
```



###### 全1数组 (ones方法)

```python
arr = np.ones((3,7),dtype=np.int32)
print(arr) #三行二列的全1数组
```



###### 未初始化数组 (empty方法)

里面具体什么值我们先不指定,如果打印的话,每次不同参数里面的值都不同

```python
arr = np.empty((2,3)) #创建一个二行三列的未初始化数组
```



###### 固定值数组 (full方法)

固定一个值的数组

```python
arr = np.full((3,4),2025) #前面写形状,后面写数,创建了一个三行四列的全是2025的数组
#tips : like方法
arr1 = np.zeros_like(arr)
print(arr1)
#创建一个arr1数组,形状像arr一样,全是0
#同理也有ones_like,empty_like,full_like
arr1 = np.ones_like(arr)
arr1 = np.empty_like(arr)
arr1 = np.full_like(arr,2026)
```



#### 4.等差数列数组(arange方法)

```python
arr = np.arange(1,25,4).reshape(2,3)   #三个数字分别是开始数字,结束数字,步长
print(arr) #结果是1,5,9,13,17,21,结束数字是不包括的,左闭右开
```



#### 5. 等间隔数列数组(linspace方法)

在这个区间内(1-100),等分的生成n个数字(20份),和等差数列差不多

```python
arr = np.linspace(0,100,20).reshape(4, 5) #指定形状
print(arr)   #结果:[[  1   6  11  16  21]
				# [ 27  32  37  42  47]
				# [ 53  58  63  68  73]
 				# [ 79  84  89  94 100]]
```



#### 6.对数间隔数列(logspace方法)

#### 我觉得叫指数比较好

```python
arr = np.logspace(0,2,3,base=2)
print(arr)
arr = np.logspace(0,7,8,base=2) #参数二+1=参数三,则为全整数
print(arr)  #生成等比数列,第一个数字是2的0次方,最后一个数字是2的7次方,中间的数字成等比数列
```

第一个参数：指数的起始值  

第二个参数：指数的结束值（包含） 

第三个参数：生成的元素个数 

第四个参数：底数base，默认为10  

总结：生成等比数列，元素为 base^指数，指数从1到4均匀分布

**补充:等比数列数组的生成**

给首项和末项, 以及元素个数即可

```python
arr = np.geomspace(1, 256, 8, dtype=int)
print(arr)
```





#### 7.特殊矩阵创建(二维数组)

矩阵指的就是二维数组

##### 特殊矩阵

###### 零矩阵(zeros方法)

所有元素为0

前面学过了, 就是zeros方法

```python
arr = np.zeros((5,6),dtype=int)
print(arr)
```



###### 单位矩阵(eye方法)

对角线上为1,其余为0

```python
arr1 = np.eye(8)      #括号数字表示几乘几的,这就是8x8的单位矩阵
arr2 = np.eye(5,4)    #可以不是方阵,会有一列或者一行全是0
arr3 = np.eye(8,k=2)  #k=2,设置分布系数,对角线1偏移量
```



###### 对角矩阵(diag方法)

只有对角线有非0值,非对角线数字全为0

如果是一维数组则生成对角矩阵

```python
arr = np.diag([1,2,0,6])  #括号数字表示对角线上的值
print(arr)
#结果
[[1 0 0 0]
 [0 2 0 0]
 [0 0 0 0]
 [0 0 0 6]]
arr = np.diag([1,2,0,6],k=1) # k属性生成偏移对角矩阵
print(arr)
#结果
[[0 1 0 0 0]
 [0 0 2 0 0]
 [0 0 0 0 0]
 [0 0 0 0 6]
 [0 0 0 0 0]]
```

如果是二维数组则取对角线

```python
arr = np.array([[1, 2, 3,1],
                [4, 5, 6,1],
                [7, 8, 9,1],
                [10,11,12,1]])
arr1=np.diag(arr)
print(arr1)  #结果:[1 5 9 1]
#k属性可以偏移取对角线值
print(np.diag(arr, k=1))   #[2 6 1]
print(np.diag(arr, k=-1))  #[4 8 12]
```



###### 对称矩阵

原矩阵和转置一样,对称矩阵**一定是方阵**

一个方阵加上自己的转置就是对称矩阵

```python
arr=np.array([1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16]).reshape(4,4)
print(arr)
S = arr + arr.T
print(S)
#结果
[[ 2  7 12 17]
 [ 7 12 17 22]
 [12 17 22 27]
 [17 22 27 32]]
```



#### 8.随机数组的生成(random方法)

##### 生成0到1之间的随机浮点数(rand函数)

```python
arr = np..random.rand(2,3)   #这参数是形状
print(arr)  #每次运行结果都不一样,但范围在0到1中间
```



##### 生成指定范围之间的随机浮点数（uniform函数）

```python
arr = np.random.uniform(3,9,(3,4))   #前面是区间,后面括号里面是矩阵形状
print(arr)
```



##### 生成指定范围之间的随机整数（randint函数）

```python
arr = np.random.randint(3,50,(5,6))  #同样,前面是区间,后面是形状
print(arr)
```



前面三个都是均匀分布,均匀分布:**在某个区间内，每个数值被取到的概率是相等的**，没有哪个地方更“密集”或更“稀疏”。



##### 生成随机数列(正态分布)(randn函数)

正态分布:两边小,中间大

整个实数轴，但大多数在 [-3, 3]

```python
arr =np.random.randn(3,4) #这参数是形状
print(arr)
```



##### 生成指定内容的随机数列(choice函数)

```py
#语法:
np.random.choice(a, size=None, replace=True, p=None)
#a：你要抽样的目标。必须是一维的（列表、元组、一维数组）。
#size：抽多少个。如果不写，就只抽 1 个。
#replace：是否放回抽样。默认 True（允许抽到重复的）；如果写 False，相当于抽签不放回（过的不再出现）。
#p：每个元素被抽到的概率（权重）。默认是均匀分布（大家概率一样）。

products = np.random.choice(['A产品', 'B产品', 'C产品'], 20)
print(products)  
```



##### 设置随机种子(seed函数)

不能直接生成数组,只是设置状态,相当于固定了,元气骑士.小丑牌,跟那个种子意思差不多

random函数通用

```python
np.random.seed(20)
arr = np.random.randint(1,10,(2,5))
print(arr)  #这样的话你生成的可以和别人一样了
```

如果使用时出现问题,可能需要重启内核



#### **9.高级构造方法(略)**

**(1)** **np.loadtxt**

```python
data = np.loadtxt('data.txt') # 读取文本文件  
```

**(2)** **np.genfromtxt**

```python
data = np.genfromtxt('data.csv',  delimiter=',') # 支持复杂格式  
```



## 五.ndarray的数据类型

耳朵听烂了

| 类型                                                 | 说明        | 简写                        |
| ---------------------------------------------------- | ----------- | --------------------------- |
| `np.bool`                                            | 布尔型      | `'?'`                       |
| `np.int8` / `np.int16` / `np.int32` / `np.int64`     | 有符号整数  | `'i1'`/`'i2'`/`'i4'`/`'i8'` |
| `np.uint8` / `np.uint16` / `np.uint32` / `np.uint64` | 无符号整数  | `'u1'`/`'u2'`/`'u4'`/`'u8'` |
| `np.float16` / `np.float32` / `np.float64`           | 浮点数      | `'f2'`/`'f4'`/`'f8'`        |
| `np.complex64` / `np.complex128`                     | 复数        | `'c8'`/`'c16'`              |
| `np.str / `np.bytes`                                 | 字符串/字节 | `'U'`/`'S'`                 |
| `np.object`                                          | Python 对象 | `'O'`                       |
| `np.datetime64` / `np.timedelta64`                   | 日期时间    | `'M8'`/`'m8'`               |

uint是无符号,符号是负数啥的,int8:小于2的8次方,后面数字是占用

创建数组时可以使用dtype参数指定元素类型(前面的创建数组的细节)：

```python
arr1 = np.array([1, 2, 3], dtype=np.float64)  # [1. 2. 3.] 
arr2 = np.array([0.2, 2.5, 4.8], dtype="i8")   # [0 2 4] 
arr3 = np.array([0.2, 2.5, 4.8], dtype=float)   # [0 2 4] 
```

也可以使用**astype()方法**转换数组的元素类型：

```python
arr1 = np.array([1, 2, 3], dtype=np.float64)  # [1. 2. 3.] 
arr1 = arr1.astype(np.int64)   # [1 2 3]  
```



## 六.numpy--索引和切片

python列表那里是不是学过

目的:为了查询数据,找到数组元素数字

| **索引**/**切片类型**       | **描述/用法**                                          | **示例代码**                                                 | **示例输出**                                                 |
| --------------------------- | ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **基本索引**                | 通过整数索引直接访问元素。索引从0开始。                | arr = np.arange(1,10).reshape(3,3)<br>print(arr[1, 2])       | 6（第2行第3列的元素）                                        |
| **行**/**列切片**           | 使用冒号 : 切片语法选择行或列的子集。                  | print(arr[:, 1])<br>print(arr[::2, ::2])                     | [2 5 8]（所有行的第2列）<br>[[1, 3], [7, 9]]（隔行隔列取样） |
| **连续切片**                | 从起始索引到结束索引按步长切片。                       | arr = np.arange(10)<br>print(arr[2:9:2])<br>print(arr[2:])   | [2 4 6 8]（索引2到8，步长2）<br>[2 3 4 5 6 7 8 9]（索引2到最后） |
| **使用** **slice** **函数** | 通过 slice(start, stop, step) 定义切片规则。           | print(arr[slice(2, 9, 2)])                                   | [2 4 6 8]（等效于 arr[2:9:2]）                               |
| **布尔索引**                | 通过布尔条件筛选满足条件的元素。支持逻辑运算符 &、\|。 | data=np.random.randint(1,100,(5,6))<br/>print(data)<br/>print(data[(data > 5) & (data < 50)])<br/>print(data[data>=50]) | 输出满足条件 5 < data < 50 的元素数组。                      |



#### 一维数组的索引与切片

```python
#生成随机一维数组,里面括号是形状(20,)是一维数组,一行数字,没有高
arr = np.random.randint(1,100,(20,))
print(arr)
```



###### 基本索引:打印数组某一个数字

```python
print(arr[0])  #打印arr的第一个数字
print(arr[3])  #打印arr的第四个数字
print(arr[1],arr[5],arr[9])
```



###### 行列索引:打印某整行某整列

一维这里是打印全部数字

```python
print(arr[:])    #一个冒号表示全部矩阵
```

冒号的用法,可以加数字,前面是索引开始数字,后面是索引结束数字(就是切片了)



###### 连续切片:打印范围数字

```python
print(arr[2:5])     #打印从索引2到索引4的所有数字(左开右闭)
print(arr[3:19:2])  #可以加步长参数
```



###### 布尔索引(条件索引):

我觉得不如叫条件索引,用条件筛选索引范围

写法:中括号里写条件即可

```python
#中括号里面是条件arr数组中,arr的元素大于10
print(arr[arr>10])   
#支持逻辑运算(and:&,,,,or:|,,,,,not:~)
print(arr[(arr>30) & (arr<50)])
```



#### 二维数组的索引与切片

```python
#随机生成创建一个二维数组
arr = np.random.randint(1,100,(4,5))
print(arr)
```



###### 基础索引:打印数组某一个数字

```py
#通过索引,按行按列从开始数[1,4]就是第二行第五个数字
print(arr[1][4])
print(arr[1,4])
#上面两种写法都对,取得都是同一个数字
```



###### 行列索引:打印某行某列

```py
#打印整个矩阵
print(arr[:])   #一个冒号表示全部矩阵
print(arr[:,:]) #两个冒号用逗号隔开,表示全部的行和全部的列,也是整个矩阵
#打印某行某列
print(arr[1,:])  #打印第二行
print(arr[1])    #直接写1也行,默认存在后面的冒号
print(arr[:,1])  #但是打印列,行的冒号不能省略
```



###### 切片索引

```py
print(arr[1,2:5])           #第二行,第2到第4列的数字
print(arr[1:4,2:5])         #第2行到第4行,第2到第4列的数字
print(arr[::3, ::2])        #可以指定步长跳跃取值
print(arr[::2, [0, 1, 3]])  #切片加花式索引组合取指定的值
				           #不支持两个花式索引组合取值
```



###### 布尔索引

和一维有区别

```py
#如果只想选某行的大于30的数据
print(arr[arr>30])  #结果是一维的,不会展示位置

#可以与其他索引嵌套
arr[1,:]  #找出第二行的数据
print(arr[1,:] [arr[1,:]>30])  #嵌套呗
arr[:,2]  #找列
print(arr[:,2] [arr[:,2]>30])

#同样支持& | ~ (numoy的取反是~)
print(arr[(arr>30)&(arr<50)])
print(arr[1,:][(arr[1,:]>30)&(arr[1,:]<50)])  #嵌套
```





## 七.numpy--ndarray的运算

加减乘除???,这他妈不是线性代数吗

#### 算数运算

###### 一维数组

```python
a = np.random.randint(1,10,5)
b = np.random.randint(1,10,5)
print(a)     #两个数组的位置一定要可以对应的上
print(b)
print(a+b)   #加减乘除都行,就是对应位置上的运算
```

```python
#如果是python的列表相加,是两个列表拼接
c = [1,2,3]
d = [4,5,6]
print(c+d)     -------    [1,2,3,4,5,6]
print(c-d)      -------   error
```



###### 二维数组:矩阵

```py
a = np.random.randint(1,10,(2,3))
b = np.random.randint(1,10,(2,3))
print(a)    #同样两个数组的位置一定要可以对应的上
print(b)
print(a+b)  #也可以加减乘除,就是他妈的线代,乘方(**)也可以
```



###### 和标量运算

就是每个数算一遍

```py
arr1 = np.random.randint(1,20,(3,4))
print(arr1)
print(arr1+10)
print(arr1-10)
print(arr1*10)
print(arr1/10) 
```



###### 广播机制(形状不一样的数组之间的运算)

了解就行,平时尽量把数据弄成一样的

当两个形状不同的数组运算时，NumPy 会尝试广播：

1. 从最后一个维度开始比较
2. 维度大小相同，或其中一个为1，则兼容
3. 维度缺失的数组在前面补1

```py
a = np.array([1,2,3])          #1x3
b = np.array([[4],[5],[6]])    #3x1
```

```py
a:  1   2   3
b:  4
    5  
    6
```

同一维度:每个方向上两个形状有相同数字(1,3),或者有数字1就行

这个a,b满足有1

```py
a广播后: 1  2  3
		1  2  3
		1  2  3
b广播后: 4  4  4
		5  5  5
		6  6  6
```

然后就能运算了

```py
#结果
[[5 6 7]
 [6 7 8]
 [7 8 9]]
```

再比如:4x5的矩阵和3x6的矩阵就不行

1x3的矩阵和1x2的矩阵就不行(行方向满足,列不满足)







#### 矩阵运算(这真是线代了)

```py
a = np.array([[1,2,3],[4,5,6],[7,8,9]])
b = np.array([[4,5,6],[7,8,9],[1,2,3]])

a: 1  2  3     b: 4  5  6
   4  5  6		  7  8  9
   7  8  9		  1  2  3
```

```py
a1行 1  2  3
b1列 4  7  1
1x4+2x7+3x1=21  #第一个数字,类推
```

```python
#矩阵的乘法运算在Python中是@符号
print(a@b)
#结果:
21 27 33
57 72 87
93 117 141
```



## 八.numpy函数

| **分类**     | **函数**                  | **功能说明**                 |
| ------------ | ------------------------- | ---------------------------- |
| **基本数学** | np.sqrt(x)                | 计算平方根                   |
|              | np.exp(x)                 | 计算指数（e^x）              |
|              | np.log(x)                 | 计算自然对数（ln(x)）        |
|              | np.sin(x)                 | 计算正弦值                   |
|              | np.abs(x)                 | 计算绝对值                   |
|              | np.power(a, b)            | 计算 a 的 b 次幂             |
|              | np.round(x, n)            | 四舍五入（保留 n 位小数）    |
| **统计**     | np.sum(x)                 | 求和                         |
|              | np.mean(x)                | 计算均值                     |
|              | np.median(x)              | 计算中位数                   |
|              | np.std(x)                 | 计算标准差                   |
|              | np.var(x)                 | 计算方差                     |
|              | np.min(x) / np.max(x)     | 查找最小值/最大值            |
|              | np.percentile(x, q)       | 计算分位数（q: 0~100）       |
| **比较**     | np.greater(a, b)          | 元素级比较 a > b             |
|              | np.less(a, b)             | 元素级比较 a < b             |
|              | np.equal(a, b)            | 元素级比较 a == b            |
|              | np.logical_and(a, b)      | 逻辑与（逐元素）             |
|              | np.where(condition, x, y) | 根据条件选择元素             |
| **排序**     | np.sort(x)                | 返回排序后的副本             |
|              | x.sort()                  | 原地排序（修改原数组）       |
|              | np.argsort(x)             | 返回排序后的索引             |
|              | np.lexsort(keys)          | 多键排序（按最后一列优先）   |
| **去重**     | np.unique(x)              | 返回唯一值并排序             |
|              | np.in1d(a, b)             | 检查 a 的元素是否在 b 中存在 |
| **其他实用** | np.concatenate((a, b))    | 数组拼接                     |
|              | np.split(x, indices)      | 分割数组                     |
|              | np.reshape(x, shape)      | 调整数组形状                 |
|              | np.copy(x)                | 创建数组的深拷贝             |
|              | np.isnan(x)               | 检测 NaN 值                  |

#### 1.numpy-基本数学函数

###### 计算平方根(np.sqrt)

```py
print(np.sqrt(9))
a=np.random.randint(1,40,(3,4))
print(np.sqrt(a))   #对每个数字都开平方根,结果类型是浮点数
```



###### 计算指数(np.exp)

```py
print(np.exp(1))  #计算e的多少次方
arr = np.random.randint(1,10,(2,2))
print(arr)
print(np.exp(arr))
```



###### 计算自然对数(np.log)

```py
#求的就是ln()
print(np.log(2.71))      #约等于1
print(np.log(2.71**3))   #约等于3
```



###### 计算三角函数(正弦sin余弦cos)

```py
print(np.sin(0))         #0
print(np.sin(np.pi/2))   #1.0
print(np.sin(3*np.pi/2)  #-1.0
      
print(np.cos(0))         #1.0
print(np.cos(np.pi))     #-1.0
print(np.cos(2*np.pi))   #1.0
```



###### 计算绝对值(abs)

```py
arr = np.random.randn(3,5)  #正态分布,[-3,3]
arr2 = np.round(arr)
print(arr2)
print(abs(arr2))
```



###### 计算a^b(power)

```py
arr = np.array([1,2,3,4,5])
print(np.power(arr,2))   #arr^2
arr = np.random.randint(1,8,(2,3))
print(np.power(arr,3))   #arr^3
```



###### 四舍五入(round)

```py
arr = np.random.uniform(3,9,(3,4))
print(np.round(arr))

arr = np.array([5.541,4.511,4.511])
print(np.round(arr,1))  #后面的参数设置保留几位小数,[5.5 4.5 4.5]

#银行家舍入,遇到 .5 时，舍入到最近的偶数
nm,四舍五入还不一样,比如4.5会变成4,再加一点点就变5了
arr = np.array([5.5, 4.51, 4.4])
print(np.round(arr)) #5.5最近的偶数是6,结果[6. 5. 4.]
print(np.round(2.5))   # 2.0  
print(np.round(3.5))   # 4.0  
print(np.round(4.5))   # 4.0  
print(np.round(5.5))   # 6.0  
```



###### 向上向下取整(ceil,floor)

###### ceil:天花板, 向上取整  

######  floor:地板,   向下取整

```py
arr = np.random.uniform(3,9,(3,4))
print(arr)
print(np.ceil(arr))
print(np.floor(arr))
```



###### 检测缺失值NaN(isnan)

创建时,np.nan就是缺失值

```py
print(np.isnan([1,2,3]))
#结果:[False False False]
#不是缺失值,所以是false

print(np.isnan([1,2,np.nan,3]))
#[False False  True False]
#有一个缺失值,为true了
```



#### 2.numpy-统计函数

###### 求和(npc)

```py
arr = np.random.randint(1,15,8)
print(arr)
print(np.sum(arr))
```



###### 计算平均值(mean)(average)

```py
print(np.average(arr))  
print(np.mean(arr))     #两个都可以
```



###### 计算中位数(median)

```py
#按大小排序看中间的,奇数个数是中间的,偶数个数是中间两个相加除二
print(np.median(arr))
```



###### 方差(var)   标准差(std)

```py
方差的计算
例如 1,2,3  平均值是2
方差 :((1-2)^2+(2-2)^2+(3-2)^2)/3=0.666
标准差=sqrt(方差),标准差是方差开根号

表示数据离散程度
1,2,1,2,1,1,1,2
1,0,3,0,0,0,4,3
你懂的,上面两组数据虽然平均值相同,但是方差不一样越大数据越离散
```

```py
arr = np.array([1,2,3])  #同上面的示例
print(np.var(arr))  #0.6666666666666666
print(np.std(arr))  #0.816496580927726
```



###### 最大最小值(npc)

```py
#默认求的是整个数组的
print(np.max(arr))
print(np.min(arr))

#指定axis可以查找每行每列的最大最小值
axis表示轴,等于0就是选中每列,等于1就是选中每行
print('每列最大值',np.max(arr,axis=0))  #axis=0 列   
print('每行最大值',np.max(arr,axis=1))  #axis=1 行
```



###### 找到最大值最小值的索引(argmin,argmax)

`np.argmax(arr)` 和 `np.argmin(arr)` 默认会把二维数组**展平成一维**再找索引：

```py
arr = np.array([[3,5,2,3],
                [4,4,2,1]])
print(arr)
print(np.argmax(arr))  #结果为1
print(np.argmin(arr))  #结果为7

#拓展:如果想得到二维索引坐标,使用以下方法
print(np.unravel_index(np.argmax(arr), arr.shape))  # (0, 1)
print(np.unravel_index(np.argmin(arr), arr.shape))  # (1, 3)
```



###### 计算分位数(percentile)

中位数是特殊的分位数

0%   25%   50%(就是中位数)   75%    100%  

```py
arr = np.array([44,47,56,64,67,78])
print(arr)
print(np.percentile(arr,0))   #44
print(np.percentile(arr,25))  #49.25
print(np.percentile(arr,90))  #72.5
```

下面是计算过程

```py
25%分位数
计算方法: 6个数,有5个区间,5x0.25=1.25
1.25在[1,2]中,在第2个区间内
56-47=9        #第二个区间的长度
9x0.25=2.25    #1.25的小数部分×区间长度
2.25+47=49.25  #和区间的起始位置相加就是25%分位数
```

```py
90%分位数
计算方法: 6个数,有5个区间,5x0.9=4.5  
4.5大于4小于5,在第5个区间内
78-67=11       #第五个区间的长度
11x0.5=5.5     #4.5的小数部分×区间长度 		
67+5.5=72.5    #和区间的起始位置相加就是90%分位数
```

含义就是这组数据里面有25%的数据小于49.25



###### 累计和(cumsum)

第n项是前n项的和

```python
arr = np.array([1,2,3,4])
print(np.cumsum(arr))   #结果:[1 3 6 10]
```



###### 累计乘(cumprod)

第n项是前n项的积

```python
arr = np.array([1,2,3,4])
print(np.cumprod(arr))   #结果:[1,2,6,24]
```

累计和和累计积会默认把二维数组展平成一维再找计算,结果也是以为平铺



#### 3.numpy--比较函数

###### 比较是否大于(greater)  小于(less)  等于(equal)

```py
arr = np.random.randint(1,10,5)
arr1 = np.random.randint(1,10,5)
print(arr)   #[7 6 3 1 8]
print(arr1)  #[5 4 8 2 2]
#前面是数组,逗号后面是比较的对象,返回的是bool类型
#数组与数比较
print(np.greater(arr,4))  #[ True  True False False  True]
print(np.less(arr,7))     #[False  True  True  True False]
print(np.equal(arr,5))    #[False False False False False]

#数组与数组比较,矩阵间的比较要保证形状相同
print(np.equal(arr,arr1)) #[False False False False False]
```



###### 逻辑与(logical_and)

逻辑判断,0为false,别的都是true

```py
#创建两个-1,0,1的随机数组
arr1 = np.random.randint(-1,2,5)
arr2 = np.random.randint(-1,2,5)
```

```py
#两个都为真才为真
print(np.logical_and(arr1,arr2))
```



###### 逻辑或(logical_or)

```py
#只要有真就是真
print(np.logical_or(arr1,arr2))
```



###### 逻辑非(logical_not)

```py
#打印数字相反的bool值
print(arr1,arr2)  #[0 -1  2  0  1] [1 -1  2 -1  2]
print(np.logical_not(arr1)) #[True False False True False]     
print(np.logical_not(arr2)) #[True False True False False]
#写两个参数会将前面的逻辑非的对应值赋值给后面并打印
print(np.logical_not(arr1,arr2)) #[1 0 0 1 0]
```



###### 检查数组的值的bool性  (any:至少有一个True,all:全是True)

```py
arr1=np.random.randint(0,3,5)  #有1也有0
arr2=np.random.randint(0,1,5)  #全是0
arr3=np.random.randint(1,2,5)  #全是1

#检查至少有一个元素为True(any)
print(np.any(arr1))   #true(大概率是,极小概率为全0)
print(np.any(arr2))   #false
print(np.any(arr3))   #一定是true

#检查元素是否都是True(all)
print(np.all(arr1)    #false(大概率是)
print(np.all(arr2))   #一定为false
print(np.all(arr3))   #一定是true
```



###### 检查数组:自定义条件(where)

```py
#用法 1：条件替换（三个参数）
#语法：np.where(condition, x, y)
#逻辑：如果满足 condition，就取 x，否则取 y。

arr = np.random.randint(1,10,5)
print(arr)  #[4 2 6 3 7]
print(np.where(arr>5,arr,888))   
#满足条件就是arr(保持不变),否则为888
#结果:[888 888 6 888 7]

#可以进行一个分类
arr=np.array([1,2,3,4,5])
print(np.where(arr<3,1,0))  #符合条件的用1替代,不符合的用0,进行一个分类
score = np.random.randint(0,100,50)
print(np.where(score>60,'及格','不及格'))
```

```py
#用法 2：查找满足条件的索引（一个参数)
#语法：np.where(condition)
#逻辑：只传条件，返回满足条件的元素的索引（展平后的索引）。
arr = np.array([1, 5, 3, 8, 2])
# 找到大于4的元素的索引
print(np.where(arr > 4))
# 输出: (array([1, 3]),) 

# 可以直接用来提取值
print(arr[np.where(arr > 4)]) #和布尔索引效果一样
#布尔索引
print(arr[arr>4])
# 输出: [5 8]
```



###### **多条件分支选择**函数(select)

```py
可以把它理解为 NumPy 版本的 if...elif...else。当你需要根据多个不同的条件，分别赋不同的值时，np.where 就会显得很啰嗦，这时 np.select 就派上用场了。
```

```py
#基本语法
np.select(condlist, choicelist, default=0)
#- condlist：条件列表（一个包含多个布尔数组的列表）。
#- choicelist：结果列表（对应条件为 True 时返回的值或数组）。
#- default：默认值（如果所有条件都不满足，就返回这个值，默认是 0）。
```

```py
score = np.random.randint(0,100,50)
print (np.select([score>80,(score>=60)&(score<=80),score<60],
                 ['优秀','良好','不及格'],
                 default='未知成绩'))
```



#### 4.numpy--排序函数(sort)

排序函数(sort),生成排序函数后的索引(argsort)

```py
np.random.seed(5)  #设置种子
arr=np.random.randint(1,20,15)
print(arr)

print(np.sort(arr))    #这是仅打印排序后的数组,数组本身不改变
print(np.argsort(arr))   #这是打印排完序过后的索引
#怎么倒序:反向切片即可
print(np.sort(arr)[::-1]) 
```



#### 5.numpy--去重函数(unique)

```python
arr=np.random.randint(1,20,15)
print(arr)
print(np.unique(arr))
#去重同时还会自动排序
```

 

#### 6.numpy--数组拼接函数(concatenate)

```py
#拼接的数组的条件
条件一:维度数（ndim）必须相同
条件二:上下拼接时,列数要相同;左右拼接时,行数要相同
条件三:拼接的数组的数据类型（dtype）不要求一致，但会自动转换
```

```py
#万能拼接
print(np.concatenate((arr1,arr2)))
# 1. 沿第0轴（垂直方向，上下拼接）（axis=0） 默认是上下
arr1 = np.random.randint(1,20,(2,4))
arr2 = np.random.randint(1,20,(3,4))
print(np.concatenate((arr1,arr2), axis=0))
# 2. 沿第1轴（水平方向，左右拼接）（axis=1）
arr1 = np.random.randint(1,20,(3,4))
arr2 = np.random.randint(1,20,(3,3))
print(np.concatenate((arr1,arr2), axis=1)
```

```py
#一维数组只能沿 axis=0 拼接，想要按列拼接，可以直接用vstack
#将A和B垂直拼接为一个新数组。
A = np.array([1,2,3])
B = np.array([4,5,6])
print(np.vstack((A,B)))
```



#### 7.numpy--数组分割函数(split)

```py
#强制均分
arr = np.random.randint(1,20,(5,4))
#后面数字写的要是能整除行列数整分的
print(np.spilt(arr,5)) #默认是按axis=0拆分
print(np.split(arr,4,axis=1)) #指定按列拆分


#除不尽可以单独为一份的分割:np.array_split
arr = np.array([1, 2, 3, 4, 5])
# 分成 2 份
result = np.array_split(arr, 2)
print(result)
# 输出: [array([1, 2, 3]), array([4, 5])]
# 分成 3 份
result = np.array_split(arr, 3)
print(result)
# 输出: [array([1, 2]), array([3, 4]), array([5])]


#按索引位置分割
arr = np.random.randint(1,20,20)
print(np.split(arr,[6,12,18]))   #索引为6切一刀,为12切一刀,为18切一刀
```



#### 8.numpy--调整数组形状(reshape)

```py
arr = np.random.randint(1,100,20)
print(arr)
print(np.reshape(arr,[2,10]))
print(np.reshape(arr,[4,5]))
#一定要是能分配好的形状
```



## 练习1 

###### 题目1：温度数据分析

```py
某城市一周的最高气温（℃）为[28,30,29,31,32,30,29]。
---计算平均气温、最高气温和最低气温
---找出气温超过30℃的天数。
```

```py
print(temp)
print("平均气温",np.round(np.average(temp),1))
print("最高气温",np.max(temp))
print("最低气温",np.min(temp))
print(temp[temp>30])
print("气温超过30℃的天数:",len(temp[temp>30]),"天")
```



###### 题目2：学生成绩统计

```py
某班级5名学生的数学成绩为[85,90,78,92,88]。
---计算成绩的平均分、中位数和标准差。
---将成绩转换为十分制（假设满分为10）。
```

```py
score = np.array([85,90,78,92,88])
print(score)
print("平均分是",np.mean(score),"\n"
      "中位数是",np.median(score),"\n"
      "标准差是",np.round(np.std(score),2),"\n")
print("十分制成绩为:",score/10)
```



###### 题目3：矩阵运算

```py
给定矩阵A=[[1,2],[3,4]]和B=[[5,6],[7,8]]。
---计算A+B和A*B(逐元素乘法）。
---计算A和B的矩阵乘法（点积）。
```

```py
A=np.array([[1,2],[3,4]])
B=np.array([[5,6],[7,8]])
print(A)
print(A)
print(A+B)
print(A*B)
print(A@B)
```



###### 题目4：随机数据生成

```
生成一个（3，4）的随机整数数组，范围[0，10）。
---计算每列的最大值和每行的最小值。
---将数组中的所有奇数替换为 -1。
```

```py
arr = np.random.randint(0,10,(3,4))
print(arr)
print("每列的最大值",np.max(arr,axis=0))
print("每行的最小值",np.min(arr,axis=1))
print(np.where(arr%2==1,-1,arr))
```



## 练习2

###### 题目5：数组变形

```python
创建一个1到12的一维数组，并转换为（3，4）的二维数组。
--计算每行的和与每列的平均值。
--将数组展平为一维数组。
```

```python
arr = np.arange(1,13,1)
print(arr)
arr=np.reshape(arr,(3,4))
print(arr)
print("每行的和",np.sum(arr,axis=1))
print("每列的平均值",np.mean(arr,axis=0))
arr=np.reshape(arr,12)
print(arr)
```



###### 题目6：布尔索引

```py
---生成一个（5，5）的随机数组，范围[0,20）。
---找出数组中大于10的元素。
```

```py
#布尔索引写法
arr = np.random.randint(0,20,(5,5))
print(arr)
print(arr[arr>10])

#where函数写法
print(arr[np.where(arr>10)])
```



###### 题目7：统计函数应用(找索引位置)

```py
某公司1到6月份的销售额（万元）为[120,135,110,125,130,140]。
---计算销售额的总和、均值和方差。
---找出销售额最高的月份和最低的月份。
```

```py
gmv = np.array([120,135,110,125,130,140])
print("销售额总和为",np.sum(gmv))
print("销售额均值为",np.round(np.average(gmv),2))
print("销售额方差为",np.round(np.var(gmv),2))
print("销售额最高的月份为",np.argmax(gmv)+1,"月")
print("销售额最低的月份为",np.argmin(gmv)+1,"月")
```



###### 题目8：数组拼接

```py
给定 A=[1,2,3]和B=[4,5,6]。
---将A和B水平拼接为一个新数组。
---将A和B垂直拼接为一个新数组。
```

```py
A = np.array([1,2,3])
B = np.array([4,5,6])
print(A)
print(B)

print(np.concatenate((A,B),axis=0))
print(np.vstack((A,B)))
```



###### 题目9：唯一值与排序

```py
给定数组[2,1,2,3,1,4,3]。
--找出数组中的唯一值并排序。
--计算每个唯一值出现的次数。
```

```py
arr = np.array([2,1,2,3,1,4,3,1,2,1,2,3,4,1,2,3])
#排序
print(np.unique(arr))
#将出现次数写入空字典
num = {}
for i in arr:
    if i not in num:
        num[i]=1
    else:
        num[i]+=1
#遍历空字典,打印次数
for k,v in num.items():
    print(f"{k}出现了{v}次")
```



###### 题目10：综合应用

```py
某商店5天的销售额（万元）和成本（万元）如下：
销售额：[20,25,22,30,28]
成本:[15,18,16,22,20]
---计算每天的利润（销售额－成本）。
---计算利润的平均值和标准差。
---找出利润最高的天数。
```

```py
sales = np.array([20,25,22,30,28])
cost = np.array([15,18,16,22,20])
days_money = sales - cost
for i in range(len(days_money)):
    print(f"第{i+1}天的利润为{days_money[i]}万元")

print('利润的平均值',np.average(days_money))
print('利润的标准差',np.std(days_money))

print("利润最高的天数:",np.argmax(days_money)+1)
```



# pandas数据处理,继续加油!!!

Pandas 是 Python 数据分析工具链中最核心的库，充当数据读取、清洗、分析、统计、输出的高效工具。

Pandas 提供了易于使用的数据结构和数据分析工具，特别适用于处理结构化数据，如表格型数据（类似于Excel表格）。

能从各种类型数据源中导入数据  

Pandas是基于NumPy构建的专门为处理表格和混杂数据设计的Python库

| **工具**        | **功能特色**         | **适用场景**                 |
| --------------- | -------------------- | ---------------------------- |
| Excel           | 图形界面，简单上手   | 人工分析、小规模数据         |
| SQL             | 高效读写，最终数据源 | 数据库查询和联表             |
| Python + Pandas | 算法和分析部署核心   | 数据清洗，统计分析，可视化等 |



## 核心数据类型

| 特性         | Series             | DataFrame                     |
| ------------ | ------------------ | ----------------------------- |
| **维度**     | 一维               | 二维                          |
| **索引**     | 单索引             | 行索引+列名                   |
| **数据存储** | 同质化数据类型     | 各列可不同数据类型            |
| **类比**     | **Excel单列**      | **整张Excel工作表**           |
| **创建方式** | pd.Series([1,2,3]) | pd.DataFrame({'col':[1,2,3]}) |

## Series

最左边一列是Series Index 索引

主要内容是Series Values 值

还有Series Name,创建时设置,类似列名

```py
0    4
1    7
2   -5
3    3
Name: demo1, dtype: int64
```

### 一.创建Series方法

###### 基础创建方式

```py
import pandas as pd
#里面放的就是一维数组
pan = pd.Series([1,2,3,4,5,6,7,8,9,10,11,12,13]) #S必须大写
#传的是列表元组都可以
pan = pd.Series((1,2,3,4,5,6,7,8,9,10,11,12,13)) 
print(pan)  #会出现两列,一列是索引,一列是创建出来的数,以及数据类型
```



###### 设置标签索引和数据类型

```py
#值不需要属性名,写最前面
#索引和类型需要属性名
#索引传的是列表元组也都可以
pan=pd.Series([1,2,3,4,5,6],index=['A','B','C','D','E','F'],dtype='float')
pan=pd.Series([1,2,3,4,5,6],index=('A','B','C','D','E','F'),dtype='float')
print(pan)

#结果
A    1.0
B    2.0
C    3.0
D    4.0
E    5.0
F    6.0
dtype: float64
```



###### 时间序列索引创建方法

```py
pd.date_range() —— 生成时间索引
#功能：快速生成一段连续的时间序列，通常用作 DataFrame 或 Series 的索引。

#语法:
    pd.date_range(start=None, end=None, periods=None, freq='D')
#start：起始日期。
#end：结束日期
#periods：生成多少个时间点。
这三个属性三选二就可以确定序列
#freq：频率。常见的有 'D'（日历日）、'B'（工作日）、'h'（小时）、'MS'（月初）、'ME'（月末）。

#示例:
dates = pd.date_range(start='2026-10-01', periods=10, freq='D')
prices = [100, 102, 101, 105, 107, 106, 108, 104, 103, 109]
s = pd.Series(prices, index=dates, name='收盘价')
```



###### 设置名字(name)

给这个Series一个名字

```py
pan = pd.Series([1,2,3,4,5,6],index=['A','B','C','D','E','F'],name='随便干嘛',dtype='float')
```



###### 通过字典创建

键对应索引,值对应值

```py
s = pd.Series({'a':1,'b':2,'c':3,'d':4,'e':5},name="字典创建")
print(s)
```

通过已有字典,选择指定的值创建series

```py
s = {'a':1,'b':2,'c':3,'d':4,'e':5}
s1=pd.Series(s , index = ["a","c"])  #从s里面取
print(s1)
#结果
a    1
c    3
dtype: int64
```



###### 直接传入numpy创建

因为numpy能快速生成一维数组

```py
arr = np.random.rand(12)
pan = pd.Series(arr,name = 'numpy简单创建')
print(pan)

arr = np.random.rand(12)
arr2 = np.random.randint(1,20,12)
pan = pd.Series(arr,index = arr2,dtype=float,name="值和索引都用numpy传入")
print(pan)
```

Series里面直接嵌套numpy

```py
score = pd.Series(np.random.randint(50,101,10),
                  index=['学生'+str(i) for i in range(1,11)],
                  name = 'numpy嵌套',
                  dtype = float)
print(score)
```



### 二.series的属性

|              **属性** | **说明**                                                     |
| --------------------: | :----------------------------------------------------------- |
|             **index** | 获取Series值的索引,还会返回索引的数据类型                    |
|            **values** | Series的值                                                   |
| **dtype**或**dtypes** | Series的元素类型                                             |
|             **shape** | Series的形状,查这个没鸟用                                    |
|              **ndim** | Series的维度,神了,只有一维啊,查这个没鸟用                    |
|              **size** | Series的元素个数                                             |
|              **name** | Series的名称                                                 |
|             **loc[]** | 显式索引，按标签索引或切片,查找索引对应的值  ,括号内是索引,用的是索引的名字 |
|            **iloc[]** | 隐式索引，按位置索引或切片  用的是索引的位置                 |
|              **at[]** | 使用标签访问单个元素                                         |
|             **iat[]** | 使用位置访问单个元素                                         |

跟numpy差不多

```py
#创建示例数据
pan = pd.Series([1,2,3,4,5,6],index=['a','b','c','d','e','f'],name = 'test',dtype = int)
print(pan)

print(pan.index) #Index(['a', 'b', 'c', 'd', 'e', 'f'], dtype='str')
print(pan.values) #[1 2 3 4 5 6]
print(pan.dtype) #int64
print(pan.dtypes) #int64
print(pan.size) #6
print(pan.name) #test

#显式索引,通过真正的索引名
print(pan.loc['a'])        
print(pan.loc['a':'d'])  #支持切片  

#隐式索引,通过元素的隐藏下标
print(pan.iloc[0])        
print(pan.iloc[0:3])  #支持切片

#at和loc一模一样,但不支持切片
#iat和iloc一米一样,不支持切片而已
print(pan.at['c'])
print(pan.iat[2])
```

 

### 三.访问Series数据

```py
#创建示例数据
pan = pd.Series([33,55,66,77,88,99],
                index=['学生1','学生2','学生3','学生4','学生5','学生6'],
                name = 'test',
                dtype = int)
```

如果设置了标签索引,用隐式索引访问的话可能会有问题

```py
注意点:
如果不设置索引，默认标签是 0, 1, 2, ...，这时标签和位置恰好一致，所以用 s[0] ,s[1] 访问元素没问题。
但一旦你自定义了索引（比如 '学生1'、'学生2'），标签就不再是 0,1,2...，这时如果还用 s[0] 去访问，就会报错。
```



###### 索引访问(标签索引)

```py
print(pan['学生2'])  #55

#也可以用series的索引属性访问
#显式
print(pan.loc['学生2']) #55 
#隐式
print(pan.iloc[1]) #55
```



###### 切片索引

```py
print(pan['学生2':'学生5'])

#也可以用series的索引切片访问
#显式
print(pan.loc['学生2':'学生5'])
#隐式
print(pan.iloc[1:5])
```



###### 布尔索引(我觉得依旧可以叫条件索引)

```py
print(pan[pan<60])
#结果
学生1    33
学生2    55
Name: test, dtype: int64
```



###### 访问表头/表尾数据(head/tail)

```py
#返回表头数据(head),不写参数就是默认前5行,参数是行数
print(pan.head())
print(pan.head(3))
#访问表尾数据(tail),不写参数就是默认后5行
print(pan.tail())
print(pan.tail(3))
```





### 四.Series的常用方法

s是series名

| **用途分类** | **方法**          | **说明**                                                  | **示例代码**                       |
| ------------ | ----------------- | --------------------------------------------------------- | ---------------------------------- |
| 条件判断     | isin()            | 判断元素是否包含在参数集合中                              | s.isin([1, 2])                     |
| 缺失值处理   | isna()            | 判断是否为缺失值（如 NaN 或 None）                        | s.isna()                           |
| 聚合统计     | sum()             | 求和，自动忽略缺失值                                      | s.sum()                            |
| 聚合统计     | mean()            | 平均值                                                    | s.mean()                           |
| 聚合统计     | min()             | 最小值                                                    | s.min()                            |
| 聚合统计     | max()             | 最大值                                                    | s.max()                            |
| 聚合统计     | var()             | 方差                                                      | s.var()                            |
| 聚合统计     | std()             | 标准差                                                    | s.std()                            |
| 聚合统计     | median()          | 中位数                                                    | s.median()                         |
| 聚合统计     | mode()            | 众数（可返回多个）                                        | s.mode()                           |
| 聚合统计     | quantile(q)       | 分位数，q 取 0~1 之间                                     | s.quantile(0.25)                   |
| 聚合统计     | describe()        | 常见统计信息（count、mean、std、min、25%、50%、75%、max） | s.describe()                       |
| 频率统计     | value_counts()    | 每个唯一值的出现次数                                      | s.value_counts()                   |
| 频率统计     | count()           | 非缺失值数量                                              | s.count()                          |
| 频率统计     | nunique()         | 唯一值个数（去重）                                        | s.nunique()                        |
| 唯一处理     | unique()          | 获取去重后的值数组                                        | s.unique()                         |
| 唯一处理     | drop_duplicates() | 去除重复项                                                | s.drop_duplicates()                |
| 抽样分析     | sample()          | 随机抽样                                                  | s.sample(2)                        |
| 排序操作     | sort_index()      | 按索引排序                                                | s.sort_index()                     |
| 排序操作     | sort_values()     | 按值排序                                                  | s.sort_values()                    |
| 替换值       | replace()         | 替换值                                                    | s.replace({1: 100})                |
| 转换结构     | to_frame()        | 将 Series 转为 DataFrame                                  | s.to_frame()                       |
| 比较判断     | equals()          | 判断两个 Series 是否完全相等                              | s1.equals(s2)                      |
| 信息提取     | keys()            | 返回 Series 的索引对象                                    | s.keys()                           |
| 统计关系     | corr()            | 计算相关系数（默认皮尔逊）                                | s1.corr(s2)                        |
| 统计关系     | cov()             | 协方差                                                    | s1.cov(s2)                         |
| 可视化       | hist()            | 绘制直方图（需安装 matplotlib）                           | s.hist()                           |
| 遍历操作     | items()           | 返回索引和值的迭代器                                      | for i, v in s.items(): print(i, v) |

```py
#创建示例数据
s = pd.Series([10,1,2,4,np.nan,None,26],
              index=['a','c','e','d','b','f','g'])
print(s)
```





###### 判断元素是否存在(isin)

返回全部,用bool表示

```py
print(s.isin([10,26]))  
a     True
b    False
c    False
d    False
e    False
f    False
g     True


print(s[s.isin([10,26])])  #使用索引打印出
a    10.0
g    26.0
dtype: float64
```



###### 判断哪些是缺失值(isna)	

(是的为True)

判断是否为缺失值（如 NaN 或 None）

```py
print(s.isna())
#结果
a    False
b    False
c    False
d    False
e     True
f     True
g    False
dtype: bool

#进阶用法
#1 统计缺失值个数
print(s.isna().sum())  #2

#2 筛选出缺失值
print(s[s.isna()])
#结果
e   NaN
f   NaN
dtype: float64
```







###### 查看所有的描述性信息(describe)

```py
print(s.describe(),dtype=)

#结果
count     5.000000
mean      8.600000
std      10.334409
min       1.000000
25%       2.000000
50%       4.000000
75%      10.000000
max      26.000000
dtype: float64
```



###### 获取非缺失值元素个数(count)

会忽略缺失值

```py
print(s.count())  #5
```



###### 获取元素索引(keys),最大值最小值索引(idmax),(idmin)

```py
print(s.keys())      #通过方法获得
print(s.index)       #通过属性获得

#最大值对应索引
print(s.idxmax())    #g 
#最小值对应索引
print(s.idxmin())    #b
```



###### 元素排序(sort)

按值排序时空值会排在最后

```py
print(s.sort_values())    #按值排序
print(s.sort_index())	  #按索引排序

#默认升序,括号内加ascending=False则为降序
print(s.sort_values(ascending=False))  
print(s.sort_index(ascending=False))	
```



###### 分位数(quantlie)

怎么算的去numpy看吧

```python
print(s.quantile(0.2)) #参数和numpy不一样,numpy是20
```



###### 众数(mode)

返回的是一个新的series,如果有多个众数会有多个值在里面

```py
print(s.mode())

#结果
0    2.0
dtype: float64
```



###### 元素出现次数(value_counts)

不包括空值缺失值
返回的是一个新的series,name默认为count

```py
print(s.value_counts())

#结果
2.0     4
10.0    1
1.0     1
4.0     1
26.0    1
Name: count, dtype: int64
```



###### 去重(unique)(drop_duplicates)

并没有实际去重,重新赋值后可以真正去重

```py
print(s.drop_duplicates())   #返回series
#结果
a    10.0
c     1.0
e     2.0
d     4.0
b     NaN
g    26.0
dtype: float64

print(s.unique())			#返回array
#结果
[10.  1.  2.  4. nan 26.]
```



###### 去重后的元素个数(nunique)

```py
print(s.nunique())     #不包括缺失值空值 5

#或者用count()
print(s.drop_duplicates().count())  #5
```



###### 随机抽样(sample)

```py
s.sample(2)  #括号内参数是抽样的个数
```



###### 重新采样方法(resample)

```py
`resample` 是 Pandas 中用于对时间序列数据进行频率转换和重采样的核心方法。简单来说，它可以把高频率数据（如每日）聚合为低频率（如每月），也可以将低频率数据插值或填充为高频率。还有'M','D','H'等参数
```

```py
常在时间序列索引创建后续出现,时间序列索引创建时通常会指定频率
我们会用resample进行重新指定频率进行后续计算
```

```py
s.resample('QS').mean()    #求每个季度的数据的均值
#在这里把频率降为了季度start,就是每个季度,而且要在这里求值,不然出现的会是日期
```



###### 替换值(replace)

```py
s.replace({2: 100})   #将2替换成100输出,实际series没有改变
```



###### 隐式索引和显式索引相互转换

```py
#已得到显式索引,求隐式索引
s.index.get_loc(显式索引)  #对s的index使用get_loc方法

#已得到隐式索引,求显式索引
s.index[隐式索引]          #通过调用s的index属性
```



###### 其他

由于很多方法都是很基础或者在numpy中遇到过,也可能较为冷门,所以不做特殊说明,只在这里演示一下

```py
#求和，自动忽略缺失值
s.sum()

#平均值
s.mean()

#最小值,最大值
s.min()   
s.max()

#方差,标准差
s.var()
s.std()

#中位数
s.median()

#将 Series 转为 DataFrame
s.to_frame()

#绘制直方图(需安装 matplotlib)
s.hist()

#返回索引和值的迭代器
for i, v in s.items(): print(i, v)

#Series重命名
s.rename('新名字')

#把数据向前或向后移动指定的行数
s.shift(periods=1, fill_value=None)  #periods：移动的步数,属性名可以不写直接写步数,fill_value：平移后空出来的位置用什么填补(默认是 NaN)

#计算相邻元素差值
s.diff(periods=2)  #periods参数不写默认为1
    
#计算变化率
s.pct_change(periods=1) 
#periods：跨几期计算。默认 1 表示跟前一行比；改成 2 就是跟前两行比
本质数学公式:(当前值 - 前一个值) / 前一个值  or  当前值 / 前一个值 - 1
    
#下面这些都是需要两个series的
#判断两个 Series 是否完全相等
s1.equals(s2)  

#计算相关系数（默认皮尔逊）
s1.corr(s2)

#协方差
s1.cov(s2)
```



### 五.series案例分析

#### 1.Series案例------学生成绩统计

```py
创建一个包含10名学生数学成绩的Series，成绩范围在50-100之间。
计算平均分、最高分、最低分，并找出高于平均分的学生人数，并输出学生。
```

```py
np.random.seed(42)
score = np.random.randint(50,101,10)
score = pd.Series(score,index=['学生'+str(i) for i in range(1,11)])
print(f"平均分是{score.mean()}")
print(f"最高分是{score.max()}")
print(f"最低分是{score.min()}")
print(f"高于平均分的学生有{score[score>score.mean()].count()}个")
#输出学生
students = score[score>score.mean()].keys()
for i in students:
    print(i,end=' ')
```



#### 2.Series案例-------温度数据分析

diff方法获取差值

```py
给定某城市一周每天的最高温度Series
temperatures= pd.Series([28,31,29,32,30,27,33],
				index=['周一','周二','周三','周四','周五','周六','周日'],
                  name='城市一周每天的最高温')

完成以下任务：
找出温度超过30度的天数
计算平均温度
将温度从高到低排序
找出温度变化最大的两天
```

```py
temperatures= pd.Series([28,31,29,32,30,27,33],
index=['周一','周二','周三','周四','周五','周六','周日'],name='城市一周每天的最高温')
print(temperatures)
print("找出温度超过30度的天数有",temperatures[temperatures>30].count(),'天')
print("平均温度为",temperatures.mean())
print(temperatures.sort_values(ascending=False))

print('找出温度变化最大的两天',
       temperatures.diff().abs().idxmax(), 		  	 				    temperatures.index[temperatures.index.get_loc(temperatures.diff().abs().idxmax())-1])

```



#### 3.series案例-------股票价格分析

```py
给定某股票连续10个交易日的收盘价数据：[102.3,103.5,105.1,104.8,106.2,107.0,106.5,108.1,109.3,110.2]
创建从2023-01-01起10天的Series
计算每日收益率（当日收盘价/前日收盘价-1）
找出收益率最高和最低的日期
计算波动率（收益率的标准差）
```

```py
#创建Series,使用pd.date_range方法创建时间序列
date = pd.date_range(start='2023-01-01',periods=10)
money = [102.3,103.5,105.1,104.8,106.2,107.0,106.5,108.1,109.3,110.2]
prices = pd.Series(money,index=date,name="10个交易日的收盘价")
#计算每日收益率
#方法1:计算s.shift()
print(prices/prices.shift(1)-1)
#方法2:使用s.pct_change方法
change=prices.pct_change(1)  #收益率
print(change)
#找出收益率最高和最低的日期
print('收益率最高的日期',change.idxmax())
print('收益率最低的日期',change.idxmin())
#计算波动率
print('波动率为',change.std())
```





#### 4.series案例-------销售数据分析

freq日期自动增长设置,用于pd.date_range
resample重新采样--重新选择频率

```py
某产品过去12个月的销售量数据:[120,135,145,160,155,170,180,175,190,200,210,220]
创建从2023-1-1起12个月的Series
计算季度平均销量
找出销量最高的月份
计算月环比增长率
找出连续增长超过2个月的月份
```

```py
#创建从2023-1-1起12个月的Series
month = pd.date_range(start='2023-01',periods=12,freq='MS')
data = [120,135,145,160,155,170,180,175,190,200,210,220]
sales = pd.Series(data,index=month,name="12个月的销售量数据")
#计算季度平均销量
print(sales.resample('QS').mean().rename('季度平均销量'))
#找出销量最高的月份
print('销量最高的月份',sales.idxmax())
# 计算月环比增长率
print('月环比增长率为',(sales/sales.shift(1)-1).rename('月环比增长率'))

#找出连续增长超过2个月的月份
#没毛病啊我觉得我这逻辑非常棒
a = sales-sales.shift(1) #相较于上个月的销量变化
num = {}  #字典记录变化情况数据
x = 0 #x为增长次数
for m,d in a.items():
    if d>=0:
        x+=1  #该月份增长了
        num[m]=x
    elif d<0:
        num[m]=-1 #降低的月份,设置成-1
        x=0  #x重新赋值成1
growth_months=[]  #得到增加的月份
for i,j in num.items():
    if j>=2:
        growth_months.append(i.strftime('%Y-%m'))  #提取时间是ds问的
print('连续增长超过2个月的月份',growth_months)
```







#### 5.series案例-------每小时销售数据分析

(Between_time)(nlargest()---取前几个最大值)

```py
某商店每小时销售额随机生成:
创建从00:00起24小时的Series
按天重采样计算每日总销售额
计算每天营业时间（8:00-22:00）和非营业时间的销售额比例
找出销售额最高的3个小时
```

```py
data = np.random.randint(0,101,24)
hours = pd.date_range(start='2025-01-01 00:00',periods=24,freq='h')
prices = pd.Series(data,index=hours,name='每小时销售数据')
# print(prices)
#按天重采样计算每日总销售额
print(prices.resample('D').sum().rename('每日总销售额'))
#计算每天营业时间（ 8:00-22:00）和非营业时间的销售额比例
a=prices['2025-01-01 08:00:00':'2025-01-01 22:00:00']
print('营业时间销售额',a.sum())
print('非营业时间销售额',prices.sum()-a.sum())
print('营业时间和非营业时间的销售额比例',a.sum()/(prices.sum()-a.sum()))
#找出销售额最高的3个小时
#思路:排序+索引
print('销售额最高的3个小时',prices.sort_values(ascending=False)[0:3].keys())
```



## DataFrame

二维,像个表,也可以看成多个Series组成的

   

### 一.DataFrame的创建

主要和字典有关

###### 1.基础创建方法

```py
df = pd.DataFrame(
    {
		"id":[1,2,3,4,5],
		'name':['tom','jerry','bob','alice','jenny'],
		'age':[22,25,24,21,20],
     	'money':[1213123,131321311,3133123,1312312,4324324]
	},index=['a','b','c','d','e'],    #设置索引处,不设置默认01234
      columns=['name','age','money','id']   #设置列的顺序,鸡肋啊
)
df
```



###### 1.通过series,numpy创建

```py
#series创建的就是一列数据,numpy创建的是字典的值
dates = pd.date_range(start='2026-7-1',periods=20,freq='D')
df = pd.DataFrame({
    '日期': dates,
    '产品': np.random.choice(['A产品', 'B产品', 'C产品'],20),
    '销量': np.random.randint(5, 60, 20),
    '单价': np.random.choice([9.9, 19.9, 29.9], 20)
})
print(df)
```



###### 3.先创字典,再创DataFrame

```py
data = {
    '姓名': ['张三', '李四', '王五', '赵六', '孙七'],
    '语文': [85, 92, 78, 88, 95],
    '数学': [90, 85, 88, 92, 70],
    '英语': [78, 88, 92, 85, 80]
}
index_data=np.random.randint(1,20,5)
df = pd.DataFrame(data,index=index_data)
df
```



###### 4.设置某字段为索引

```py
#方式一:从字典创建
data = {
    'id': [101, 102, 103],
    'name': ['张三', '李四', '王五'],
    'age': [25, 30, 28]
}
# 把 'id' 这一列的数据作为索引
df = pd.DataFrame(data, index=data['id'])
df
#会保留原列
```

```py
#方式二:创建后设置(set_index)__本人更推荐这种
data = {
    'id': [101, 102, 103],
    'name': ['张三', '李四', '王五']
}
df = pd.DataFrame(data)
# 将 'id' 列设为索引（默认会从原表中移除该列，变成真正的索引）
df = df.set_index('id')
也可以设置inplace属性,这样就不用赋值给原df了
df.set.index('id',inplace=True)
df

#如果你想保留 'id' 作为普通列，同时把它当索引，可以加个参数：
df = df.set_index('id', drop=False)
```

```py
#方式三:读取文件时设置索引(index_col)
# 读取CSV时，直接让第一列（或指定列名）作为索引
df = pd.read_csv('data.csv', index_col='id') 
# 或者传入列号：index_col=0 (表示第一列)
```

###### 5.通过已有数据创建新列

```py
df['年工资']=df['money']/df['age']
df
```

###### 6.重命名列名或索引

前面写老的,后面写新的,可以同时改很多列或索引

```py
df.rename(columns={'age':'年龄'},index={0,4})     #inplace=True确认
```



### 二.DataFrame的属性

| **属性**    | **说明**                                                     |
| ----------- | ------------------------------------------------------------ |
| **index**   | DataFrame的行索引     df名.index                             |
| **columns** | DataFrame的列标签                                            |
| **values**  | DataFrame的值                                                |
| **ndim**    | DataFrame的维度  固定为2鸡肋                                 |
| **shape**   | DataFrame的形状   先行后列                                   |
| **size**    | DataFrame的元素个数                                          |
| **dtypes**  | DataFrame的元素类型  ,多个列,会有多个类型(正常情况下),注意不是dtype |
| **T**       | 行列转置  , 666                                              |
| **loc[]**   | 显式索引，按行列标签索引或**切片**                           |
| **iloc[]**  | 隐式索引，按行列位置索引或**切片**                           |
| **at[]**    | 使用行列标签**访问单个元素**(不能切片呗,和series一样)        |
| **iat[]**   | 使用行列位置**访问单个元素**(不能切片呗,和series一样)        |

```py
#示例数据创建(基础创建方法)
df = pd.DataFrame(
    {
		"id":[1,2,3,4,5],
		'name':['tom','jerry','bob','alice','jenny'],
		'age':[22,25,24,21,20],
     	'money':[1213123,131321311,3133123,1312312,4324324]
	},index=['a','b','c','d','e']   #设置索引处,不设置默认01234  
)
print(df)
```

```py
#属性示例
df.index  #Index(['a', 'b', 'c', 'd', 'e'], dtype='str')
df.columns  #Index(['name', 'age', 'money', 'id'], dtype='str')
df.values  #array([['tom', 22, 1213123, 1],
       	 		# ['jerry', 25, 131321311, 2],
       			# ['bob', 24, 3133123, 3],
       			# ['alice', 21, 1312312, 4],
           		# ['jenny', 20, 4324324, 5]], dtype=object)
df.shape  #(5, 4)
df.size   #20
df.dtypes  #name       str
		  #age      int64
		  #money    int64
		  #id       int64
		  #dtype: object
df.T  # a	b	c	d	e
	#id	1	2	3	4	5
    #name tom jerry bob alice jenny
    #age 22	25	24	21	20
    #money 1213123 131321311 3133123 1312312 4324324
    
#索引属性主要用来访问数据等,我们马上后面再讲解
```



### 三,访问DataFrame的数据(索引)

###### 访问单行(条)或多行数据 

```py
#显式索引:设置的索引名
df.loc['a']      #索引取某一条
df.loc['a':'c']  #切片取多条数据

#隐式索引
df.iloc[1]
df.iloc[1:3]
```



###### 访问单列(字段)或多列数据

```py
#显式索引
df.loc[:,'name']   #:表示所有行,name是要取的字段
df.loc[:,'name':'money'] #返回多个字段

#隐式索引
df.iloc[:,0]   #字段也有隐式索引,默认从0开始
df.iloc[:,1:4]

#也可以直接写字段指定访问
#这是选择想看的字段,不是切片
df[['name','age','id']]  #注意要用两层中括号,内层是列表表示要取的字段
df['name']   #如果只取一列字段只需要一层括号,类型是series
df.name  #这也可以取单列数据
```



###### 访问单个数据或指定范围数据

先行索引后列名

```py
#指定两个位置的索引访问单个数据
df.loc['a','name']   #'tom'

#指定两个位置的索引切片访问指定的行的指定的字段
df.loc['b':'d','id':'age']
#	id	name	age
#b	2	jerry	25
#c	3	bob		24
#d	4	alice	21
```



###### 查看部分数据(head,tail)

不写参数默认5行

```py
df.head(3)
df.tail(4)
```



###### 布尔索引(条件索引)

```py
#单个条件
#和numpy,series本质差不多,不同点是,DataFrame需要再加上字段
df[df.money>4000000]

#多个条件(逻辑符号呗)
df[(df.money>4000000)&(df.age>23)]
```



###### 随机抽样

```py
df.sample()   #括号里可以写参数,选择取几条
```



###### 修改值

只有一个示例,不做拓展

```py
df.loc[1, 'gender'] = 'dad'  #精确坐标
```



### 四.DataFrame的常用方法

| **方法**              | **说明**                                                     |
| --------------------- | ------------------------------------------------------------ |
| **head()**            | 查看前n行数据，默认5行                                       |
| **tail()**            | 查看后n行数据，默认5行                                       |
| **isin()**            | 元素是否包含在参数集合中                                     |
| **isna()**            | 元素是否为缺失值                                             |
| **sum()**             | 求和                                                         |
| **mean()**            | 平均值                                                       |
| **min()**             | 最小值                                                       |
| **max()**             | 最大值                                                       |
| **var()**             | 方差                                                         |
| **std()**             | 标准差                                                       |
| **median()**          | 中位数                                                       |
| **mode()**            | 众数                                                         |
| **quantile()**        | 指定位置的分位数，如quantile(0.5)                            |
| **describe()**        | 常见统计信息                                                 |
| **info()**            | 基本信息                                                     |
| **value_counts()**    | 每个唯一值的元素的个数                                       |
| **count()**           | 非空元素的个数                                               |
| **drop_duplicates()** | 去重                                                         |
| **sample()**          | 随机采样                                                     |
| **replace()**         | 用指定值代替原有值                                           |
| **equals()**          | 判断两个DataFrame是否相同                                    |
| **cummax()**          | 累计最大值                                                   |
| **cummin()**          | 累计最小值                                                   |
| **cumsum()**          | 累计和                                                       |
| **cumprod()**         | 累计积                                                       |
| **diff()**            | 一阶差分，对序列中的元素进行差分运算，也就是用当前元素减去前一个元素得到差值，默认情况下，它会计算一阶差分，即相邻元素之间的差值。参数：  periods：整数，默认为 1。表示要向前或向后移动的周期数，用于计算差值。正数表示向前移动，负数表示向后移动。  axis：指定计算的轴方向。0 或 'index' 表示按列计算，1 或 'columns' 表示按行计算，默认值为 0。 |
| **sort_index()**      | 按行索引排序                                                 |
| **sort_values()**     | 按某列的值排序，可传入列表来按多列排序，并通过ascending参数设置升序或降序 |
| **nlargest()**        | 返回某列最大的n条数据                                        |
| **nsmallest()**       | 返回某列最小的n条数据                                        |

```py
#示例数据创建(基础创建方法)
df = pd.DataFrame(
    {
		"id":[1,2,3,4,5],
		'name':['tom','jerry','bob','alice','jenny'],
		'age':[22,np.nan,24,22,20],
     	'money':[1213123,131321311,np.nan,1312312,4324324]
	},index=['a','b','c','d','e']   #设置索引处,不设置默认01234  
)
print(df)
```



###### 元素是否 在DataFrame中   isin()

```py
#可以写很多值,只要在集合中对应位置就会是True
df.isin(['jack','20',1312312,'tom'])   
#注意是区分类型的,整型20可以查到True,字符型查不到
```



###### 是否有缺失值 isna()

```py
df.isna()  #有缺失值的位置会返回True
df[['id','age']].isna()  #访问数据后再检查缺失值
```



###### 描述性信息**describe()**  

```py
#查看数据的一些统计性信息
#如数据个数,平均值,方差,标准差,最大最小值等
df.describe()
```



###### 基本信息  info()

```py
#查看DataFrame的一些基本信息
#如数据条数(索引个数),字段数,以及每个字段的名称类型以及空值个数
df.info()
```



###### 每列有多少数据  count()

缺失值不统计

```py
df.count()  #查看每个字段的非空元素的个数
#结果
id       5
name     5
age      4
money    4
dtype: int64
```



###### 数据出现的次数  value_counts()

会默认降序排列

```py
#相同行会算作同一条数据,如果某行有空值会忽略该行
df.value_counts()   
#结果
id  name   age   money    
1   tom    22.0  1213123.0    1
4   alice  21.0  1312312.0    1
5   jenny  20.0  4324324.0    1
Name: count, dtype: int64id  name   age   money    

#如果想把空值也算入数据并统计,需要修改dropna为false
df.value_counts(dropna=False)
#结果
id  name   age   money      
1   tom    22.0  1213123.0      1
2   jerry  NaN   131321311.0    1
3   bob    24.0  NaN            1
4   alice  21.0  1312312.0      1
5   jenny  20.0  4324324.0      1
Name: count, dtype: int64

#实际分析中常常是对单列(series)进行分析处理
df.age.value_counts()
#结果
age
22.0    2
24.0    1
20.0    1
Name: count, dtype: int64
```

###### 删除某列

```py
# 删除一整列
df.drop(columns=['name'], inplace=True)
```



###### 检查重复   duplicated()

会返回一个与原表等长的布尔型 Series（True 表示这一行是重复的，False 表示不是)

而且第一次出现的数据不算重复,后面的才算True,可以通过keep参数选择哪一条不算重复数据(keep='last',默认为first,keep=False则只要重复了都算重复数据True)

```py
df.duplicated()   				#检查全部数据有无重复
df.duplicated(subset='列名')    #按哪些列来判断是否重复
df.duplicated(subset='age',keep=False)   
#结果
a     True
b    False
c    False
d     True
e    False
dtype: bool
#可以与bool索引嵌套
```



###### 去重  drop_duplicates()

上面检查出来了重复行,然后进行去重
用来删除重复行的核心方法

```py
#语法
df.drop_duplicates(subset=None, keep='first', inplace=False)

#核心参数拆解
subset：按哪些列来判断是否重复。默认是 None，表示所有列的值都完全一样才算重复。
keep：保留哪一条。
'first'（默认）：保留第一次出现的，删掉后面的。
'last'：保留最后一次出现的，删掉前面的。
False：只要重复，就把所有重复项全删了（一个都不留）。
inplace：是否直接在原表上修改。默认 False（返回新表，原表不变）。
```

```py
df.drop_duplicates(subset='age',keep=False)  #只要是重复值就删除
#结果
#	id	name    age	     money
#b	2	jerry	NaN	    131321311.0
#c	3	bob	    24.0	   NaN
#e	5	jenny	20.0	4324324.0
df.drop_duplicates(subset='age',inplace=True)  #在原表上操作
```



###### 替换某值 replace()

```py
df.replace(22,18)  #所有22被替换成18(原表未修改)
df.replace(22,18,inpalce-True)  #直接在原表上修改
```



###### 累计和累计积  cumsum()cumprod()

```py
#每行向下叠加计算和,空值不计算
df.cumsum()
#str类型会拼接,数值型会每行求上面数据的和

df.cumprod()
#有str类型会报错
df.age.cumprod()  #只选择数值型进行计算
```



###### 排序(sort)

```py
#按索引排序 
sort_index()

#按值排序     
sort_values(by='列名')   #要选择一个列作为排列对象

df.sort_values(['money'],ascending=False)  
#ascending=False 降序,不写默认升序
#空值都放在最后

#选择多列排序    
df.sort_values(by=['age','money'],ascending=True)
```



###### 取最大几个值  nlargest()

```py
df.nlargest(2,columns=['age'])
```



###### 取最小几个值  nsmallest()

```py
df.nsmallest(3,columns=['money'])
```



###### 其他

由于很多方法都是很基础或者在numpy中遇到过,也可能较为冷门,所以不做特殊说明,只在这里演示一下

```py
#求和 sum()
#直接对整张表求和,字符串会拼接在一起
df.sum()
#选择需要的列求和
df.age.sum()
df[['age','id']].sum()
#横向求和,别的统计方法也适用
df.sum(axis=1)  #注意不能有int型的数据
df[['数学','英语','物理']].sum(axis=1)  #案例的例子

#最大值(max)
df.max()
df.age.max()

#平均数
df.mean()  #有字符型会报错
df.age.mean()

#中位数(median)
df.median()
df.money.median()

#cummax(),cummin()
出现更大值时变成更大值保持到出现下一个最大值时

#equal()
判断两个DataFrame是否一样

#等等等等,后面再补充
#series的大部分方法DataFrame都可以用
```







### 五.DataFrame案例

##### 案例1:学生成绩分析

```py
场景：某班级的学生成绩数据如下，请完成下任务：
'姓名':['张三','李四','王五','赵六','钱七']
'数学':[85,92,78,88,95]
'英语':[90,88,85,92,80]
'物理':[75,80,88,85,90]
1.计算每位学生的总分和平均分。
2.找出数学成绩高于90分或英语成绩高于85分的学生。
3.按总分从高到低排序，并输出前3名学生。
```

```py
#创建DataFrame
score={
    '姓名':['张三','李四','王五','赵六','钱七'],
    '数学':[85,92,78,88,95],
    '英语':[90,88,85,92,80],
    '物理':[75,80,88,85,90]
}
df = pd.DataFrame(score,index=(1,2,3,4,5))
df.set_index('姓名',inplace=True)
print(df)
#1.计算每位学生的总分和平均分。
print(df[['数学','英语','物理']].sum(axis=1))
#2.找出数学成绩高于90分或英语成绩高于85分的学生。
print(df[(df.数学>90)|(df.英语>85)].index)
#3.按总分从高到低排序，并输出前3名学生。
df['总分']=df['数学']+df['英语']+df['物理']
print(df.nlargest(3,columns='总分'))
```



##### 案例2:销售数据分析

```py
场景：某公司销售数据如下，请完成以下任务：
'产品名称':['A','B','C','D']
'单价':[100,150,200,120]
'销量':[50,30,20,40]
1.计算每种产品的总销售额（销售额=单价×销量）
2.找出销售额最高的产品。
3.按销售额从高到低排序，并输出所有产品信息。
```

```py
#创建数据
data = {
    '产品名称':['A','B','C','D'],
		'单价':[100,150,200,120],
		'销量':[50,30,20,40]
}
data=pd.DataFrame(data,index=(1,2,3,4))
#1.计算每种产品的总销售额（销售额=单价×销量）
data['销售额']=data['单价']*data['销量']
print(data[['产品名称','销售额']])
#2.找出销售额最高的产品。
print(data['产品名称'][data.销售额==data.销售额.max()])
# 3.按销售额从高到低排序，并输出所有产品信息。
data.sort_values(['销售额'],ascending=False)
```



##### 案例3:电商用户行为分析

```py
场景：某电商平台的用户行为数据如下，请完成以下任务：
		'用户ID':[101,102,103,104,105],
		'用户名':['Alice','Bob','Charlie','David','Eve'],
		'商品类别':['电子产品','服饰','电子产品','家具','服饰'],
		'商品单价':[1201,301,800,150,201.01],
		'购买数量':[1,3,2,5,4]
1.计算每位用户的总消费金额（消费金额=商品单价×购买数量）
2.找出消费金额最高的用户，并输出其所有信息
3.计算所有用户的平均消费金额（保留2位小数）
4.统计电子产品的总购买数量
```

```py
data = pd.DataFrame({
		'用户ID':[101,102,103,104,105],
		'用户名':['Alice','Bob','Charlie','David','Eve'],
		'商品类别':['电子产品','服饰','电子产品','家具','服饰'],
		'商品单价':[1201,301,800,150,201.01],
		'购买数量':[1,3,2,5,4]
},index=(1,2,3,4,5))
data
# 1.计算每位用户的总消费金额（消费金额=商品单价×购买数量）
data['总消费金额']=data['商品单价']*data['购买数量']
# data
print(data[['用户ID', '总消费金额']])
# 2.找出消费金额最高的用户，并输出其所有信息
print(data.nlargest(1,columns='总消费金额'))
# 3.计算所有用户的平均消费金额（保留2位小数）
print('所有用户的平均消费金额',data['总消费金额'].mean().round(2))
# 4.统计电子产品的总购买数量
print(data[data['商品类别']=='电子产品']['购买数量'].sum())
```







## pandas数据分析

### 一.数据的导入导出(read_____,to_)

###### 导入CSV文件(read_csv)

```py
df = pd.read_csv('./data/employees.csv')   
#括号内填文件地址,注意使用相对路径和绝对路径
```

###### 保存导出CSV文件(to_csv)

```py
df.to_csv('new.csv')
#括号内写路径和新文件名
```



###### 导入json文件(read_json)

**方法一**

```py
pc = pd.read_json('data1.json') 
```

**方法二**

```py
#先读取json文件的字典
import  os   #专门处理系统文件的库
with open('./data/test.json',encoding='utf-8') as f:  
		data = json.load(f)
#通过字典创建csv文件
df = pd.DataFrame(data['users'])  #users是表头名
df
```

```py
#再写入csv文件
df.to_csv('test.csv')
```





### 二.缺失值的处理(数据清洗)

##### 缺失值的认知 

```py
np.nan :  #not a number 倾向于不是一个数字
None   :  #表示不存在
pd.NA  :  #倾向于没有数值
```

##### **创建时指定缺失值**

```py
#示例数据
s = pd.Series([1,2,np.nan,None,pd.NA])
df =pd.DataFrame([[1,pd.NA,2],[2,np.nan,5],[None,4,6],[2,3,4]])
```



##### 查看缺失值isna(),以及缺失值个数

在写代码时，用哪个都行，结果绝对一模一样。**但官方更推荐用 `isna()`。**

```py
print(s.isna())
print(s.isnull())
print(df.isna())
print(df.isnull())
#加sum()可以查看每列的缺失值个数
df.isna().sum(axis=1)  #每行缺失值个数
df.isna().sum()        #每列缺失值个数
```



##### 去除缺失值 (dropna())

###### series去除缺失值



**series删掉的仅是缺失值**

```py
print(s.dropna())
print(s.dropna(inplace=True))   #直接在原数据上操作
```







###### DataFrame去除缺失值



**DataFrame删掉的是缺失值以及缺失值存在的整行或整列**

```py
print(df.dropna())   #去除缺失值时去除一整行的记录
print(df.dropna(axis=1))  #去除缺失值时去除一整列的记录
#加上inplace属性:在原数据上操作 
```

**去除DF缺失值时的控制**

```py
#如果所有的值都是缺失值,才删除这一行
print(df.dropna(how='all'))
```

```py
#控制有几个缺失值时才保留数据
print(df.dropna(thresh=2))  #至少有两个是正常值,才保留这一行
```

```py
#如果某列有缺失值,则删除有缺失值的行(subset)
print(df.dropna(subset=['列名']))
print(df.dropna(subset=1))  #上面的示例数据
#如果想删除某列用drop直接删就行
```



##### 缺失值的填充(fillna)

###### 导入示例数据

```py
data = pd.read_csv('weather_withna.csv')
data
```



###### 固定值填充  fillna()

```py
#语法:
df名.fillna({'列名':某值,'列名':某值})
#如果不指定列,则所有空值都用这个某值填充,也不用写大括号
```

**使用指定值来填充**

```py
data.fillna({'weather':'sunny'})
data.fillna({'temp_max':123})
```

**使用计算值填充**

```py
#指定列,后面写对应列的计算值即可
data.fillna({'wind':data.wind.mean()})  
#wind这一列的缺失值用所有wind的mean来填充
```

**使用附近值填充**

```py
#  -----front,用前面的相邻值填充
data.ffill()
#  -----behand,用后面的相邻值填充
data.bfill()
```

这都是基础填充方式,具体填充要看具体业务使用具体公式



### 三.重复数据处理,数据类型转换

###### 插入数据

```py
data={
    'name':['alice','alice','bob','alice','jack','bob'],
    'age':[26,25,30,25,35,30],
    'city':['NY','NY','LA','NY','SF','LA']
}
df=pd.DataFrame(data)
df
```



###### 检查重复数据-----duplicated()

```py
df.duplicated()   #一整条数据完全相同,则标记为重复
df.duplicated(subset='name')    #按哪些列来判断是否重复
```



###### 去重-----drop_duplicates()

```py
#去除完全相同的数据
df.drop_duplicates()  
#指定根据某列的重复值去除数据,subset参数
df.drop_duplicates(subset=['name'])  
#keep参数---保留出现的数据---默认first
df.drop_duplicates(subset=['name'],keep='last')  #保留最后一条重复值
```





###### 数据类型的转换(astype)

导入数据,预览类型

```py
df =pd.read_csv('sleep.csv')
df
df.dtypes   #-----先查看数据类型
```

```python
#预览类型结果
person_id                    int64
gender                         str
age                          int64
occupation                     str
sleep_duration             float64
sleep_quality              float64
physical_activity_level      int64
stress_level                 int64
bmi_category                   str
blood_pressure                 str
heart_rate                   int64
daily_steps                  int64
sleep_disorder                 str
dtype: object
```

```py
#直接修改原表的数据,没有inplace=True
# -----int64转换成int16
df.age=df.age.astype('int16',inplace=True) 
#-------str类型转换成category类型(枚举型类型)
df.gender=df.gender.astype('category')
#如果用astype修改类型后是只读的,想要修改只能改回原类型或别的宽松类型
```





###### 布尔类型映射------映射(map)

要想修改原表，必须用等号把结果重新赋值给这一列：

```py
#新增新列is_male,用map对female和male映射成bool型
df['is_male'] = df.gender.map({'Female':'女','Male':'男'})
```



### 四.数据变形

##### 导入数据

```py
data = {	
	'ID':[1,2],
	'name':['alice smith','bob smith'],
	'Math':[90,85],
	'English':[88,92],
	'Science':[95,89]
}
df = pd.DataFrame(data)
df
#	ID	name	   Math	 English Science
#0	1	alice smith	 90		88	  95
#1	2	bob smith	 85		92	  89
```

##### 矩阵转置

```py
df.T  #行列转换
df   
#	       0	1
#ID	       1	2
#name	alice smith bob smith 
#Math	  90	85
#English  88	92
#Science  95	89
```



##### 宽表转长表(列转行) melt()

```py
#语法
pd.melt(frame,id_vars=None,value_vars=None,var_name=None,value_name='value')
#参数详解
#frame:变形对象
#id_vars:保持不变的列
#var_name:要变的新列名
#values_name:要变的值的新列名
```

```py
df2 = pd.melt(df,id_vars=['ID','name'],var_name='科目',value_name='成绩')
df2
#
ID	name	科目	成绩
0	1	alice smith	Math	90
1	2	bob smith	Math	85
2	1	alice smith	English	88
3	2	bob smith	English	92
4	1	alice smith	Science	95
5	2	bob smith	Science	89
```



##### 长表转宽表(行转列) pivot()

```py
#语法
df.pivot(index=None, columns=None, values=None)
#参数详解
#index：你想保留在左侧作为“行标签”的列（比如：姓名）。
#columns：你想把它拆开变成“新列名”的列（比如：科目）。
#values：你想填入新表格里的“具体数据”列（比如：分数）
```

```py
df3 = pd.pivot(df2,index=['ID','name'],columns='科目',values='成绩')
		科目	English	Math Science
ID	name			
1	alice smith	88	  90	95
2	bob smith   92	  85	89
```



##### 分列(split)

将name列分成两列

```py
#引号内是根据什么分裂,这个例子中是根据空格分,expand参数使结果变成新列
df.name.str.split(" ",expand=True)   
#将两列命名并加入原df
df[['first','last']]  =  df.name.str.split(" ",expand=True)  
df
```



```python
	ID	name	   Math	English	Science	 first	 last
0	1	alice smith	90	  88	  95	alice	smith
1	2	bob smith	85	  92	  89	bob	    smith
```



##### 实例

###### 读入数据

```py
df = pd.read_csv('./data/sleep.csv')
df
```

###### 取两列看看

```py
df[['person_id','gender','blood_pressure']]
```

###### 分列

```py
df[['person_id','gender','blood_pressure']]
df[['high','low']]=df.blood_pressure.str.split('/',expand=True)
df[['gender1','gerder2']]=df.gender.str.split('a',expand=True)#写着玩的
df
```



### 五.数据分箱-----pd.cut

##### 介绍和语法

```py
#介绍
把连续的数值分割成离散的区间
比如，把具体的年龄(25,35,48...)变成 "青年、中年、老年"；或者把"百分制成绩"变成"及格、良好、优秀"
```

```py
#语法
pd.cut(x.bins,labels)    
# x---分割的数据,一般是df的列,series
# bins---切分规则。可以是一个整数(表示平均切几份),也可以是一个列表(自定义切分点)
# labels----给切好的箱子起名字
```



##### 导入数据

```py
df = pd.read_csv('./data/employees.csv')
df
```

```py
#举例,对salary进行分箱,查看各区间工资情况
df1=df.sample(10)[['employee_id','salary']]
df1   #先取随机10条
```





##### 分箱

###### 默认区间数量等分区间

```py
#分割的数据为salary
#bins等于3,分成3段区间
#起始值.结束值是所有数据的最大值.最小值,另两个数是均分的数
#对应的数在对应的区间,区间只有三种
pd.cut(df1['salary'],bins=3)    #类型是枚举类型
```

```python
#随机抽样的
72     (6133.333, 9566.667]
49      (9566.667, 13000.0]
15       (2689.7, 6133.333]
101     (9566.667, 13000.0]
41       (2689.7, 6133.333]
56      (9566.667, 13000.0]
39       (2689.7, 6133.333]
25       (2689.7, 6133.333]
34       (2689.7, 6133.333]
8       (9566.667, 13000.0]
Name: salary, dtype: category
Categories (3, interval[float64, right]): [(2689.7, 6133.333] < (6133.333, 9566.667] < (9566.667, 13000.0]]
```

###### 自定义值等分区间

```py
#bins参数填列表,自动分成以下区间
#四个数,三个区间
pd.cut(df1['salary'],bins=[0,5000,10000,20000])
```

```python
#随机抽样的
78     (5000, 10000]
93         (0, 5000]
52     (5000, 10000]
8     (10000, 20000]
40         (0, 5000]
36         (0, 5000]
29         (0, 5000]
17         (0, 5000]
10     (5000, 10000]
32         (0, 5000]
Name: salary, dtype: category
Categories (3, interval[int64, right]): [(0, 5000] < (5000, 10000] < (10000, 20000]]
```

###### 分箱后的列加入原数据

```py
df1['salary_level']=pd.cut(df1['salary'],bins=[0,5000,10000,20000],labels=['低','中','高'])
```



##### 设置label标签

```py
#给每个区间命名,有几个区间就写几个名字
pd.cut(df1['salary'],bins=[0,10000,20000,30000],labels=['低','中','高'])   
```

```python
#随机抽样的
80    低
78    中
15    低
37    低
75    中
14    高
70    中
11    中
59    中
3     中
Name: salary, dtype: category
Categories (3, str): ['低' < '中' < '高']
```

 

##### 另一种分箱(均分人数)-----qcut

cut是均分区间,qcut是均分人数

```py
pd.qcut(df1['salary'],3,labels=[1,2,3]) 
#没有bins参数了,直接写数字表示段数
#如果数据有大量重复值，不要让 Pandas 瞎猜，直接自己指定分位数列表，或者加上 duplicates='drop'：
```

```python
25     1
58     2
22     2
50     3
30     1
53     2
9      2
40     1
105    3
79     1
Name: salary, dtype: category
Categories (3, int64): [1 < 2 < 3]
```

这里10除不尽所以有4,**把多出来的名额，优先分给前面的箱子**



##### 实例

###### 读入数据

```py
df = pd.read_csv('./data/sleep.csv')
#取两列
df[['person_id','sleep_quality']]
```

###### 分箱

```py
#对sleep_quality进行分箱
df['睡眠质量']=pd.cut(df['sleep_quality'],bins=3,labels=['低质量','中质量','高质量'])
df[['person_id','sleep_quality','睡眠质量']]
```

```py
	person_id	sleep_quality	睡眠质量
0		1			7.0			中质量
1		2			4.9			中质量
2		3			6.0			中质量
3		4			10.0		高质量
4		5			9.5			高质量
...		...			...			...
395		396			7.9			高质量
396		397			6.1			中质量
397		398			6.5			中质量
398		399			9.1			高质量
399		400			7.0			中质量
```

###### 统计数量分析

```py
df.睡眠质量.value_counts()
```

```python
睡眠质量
中质量    206
高质量    129
低质量     65
Name: count, dtype: int64
```







### 六.时间数据处理

###### 数据类型Timestamp

Pandas 中一种**特殊的标量类型（Scalar Type）**，专门用来表示**单个精确的时间点**

```py
d = pd.Timestamp('2015-05-02 10:22')
print(d)                  #2015-05-02 10:22:00
print(type(d))			 #<class 'pandas.Timestamp'>
#时间戳类型,后面加.＋日期单位单词,可以打印出来
print("年: ",d.year) 	 #年:  2015
print("月: ",d.month)	 #月:  5
print("日: ",d.day)  	 #日:  2
print("季度: ",d.quarter)  #季度:  2
print("小时: ",d.hour)     #小时:  10
print("分钟: ",d.minute)   #分钟:  22
```



###### 判断是否是月底月初

单位单词加end或者start

```py
d.is_month_end     #false
d.is_month_start   #false 

#年底同理
d.is_year_end      #false
```

###### 获取星期几---day_name()

```py
d.day_name()       #'Saturday'
```



###### 转换维度(to_period())

```py
#转换为月
d=d.to_period("M")

#只能低纬度转向高纬度:分钟>小时>天>月
#例如:刚才转换成月了,现在无法转成天,或者可以换一个别的遍历接受

#同维度可以计算
```



###### 字符串,df转成日期类型to_datetime

```py
#字符串转换成日期类型
a=pd.to_datetime('201502281022')
print(a)         #2015-02-28 10:22:00
#字符串201502281022变成时间戳类型了
print(type(a))   #<class 'pandas.Timestamp'>
```

```py
#DataFrame列转换成日期类型
#创建示例数据
df = pd.DataFrame({
	'sales':[100,200,300],
	'date':['20250601','20250602','20250603']
})
#定义新的列命名为datetime,是由df的date列转换的
df['datetime']  =  pd.to_datetime(df.date)   

#如果DataFrame想要获取星期几,直接用day_name会报错
df['datetime'].day_name()  #报错
因为df['datetime']是series类型(dataframe的一列),day_name是timestemp的方法
#要获取星期几的话,中间加一个.dt接口  dt.day_name
df['week'] = df['datetime'].dt.day_name()   #新列week表示星期几
df

	sales	date	datetime	week
0	100	  20250601	2025-06-01	Sunday
1	200	  20250602	2025-06-02	Monday
2	300	  20250603	2025-06-03	Tuesday
```







###### csv文件中日期的转换

导入文件

```py
df = pd.read_csv('weather.csv')
df.info()   #查看df信息,发现date是str类型
```

```python
<class 'pandas.DataFrame'>
RangeIndex: 1461 entries, 0 to 1460
Data columns (total 6 columns):
 #   Column         Non-Null Count  Dtype  
---  ------         --------------  -----  
 0   date           1461 non-null   str    
 1   precipitation  1461 non-null   float64
 2   temp_max       1461 non-null   float64
 3   temp_min       1461 non-null   float64
 4   wind           1461 non-null   float64
 5   weather        1461 non-null   str    
dtypes: float64(4), str(2)
memory usage: 68.6 KB
```

```py
df['datetime']=pd.to_datetime(df.date)   #date列转换并保存为新列
df.info()  #查看新表信息,发现多了datetime列,是datetime64类型
```

```python
<class 'pandas.DataFrame'>
RangeIndex: 1461 entries, 0 to 1460
Data columns (total 7 columns):
 #   Column         Non-Null Count  Dtype         
---  ------         --------------  -----         
 0   date           1461 non-null   str           
 1   precipitation  1461 non-null   float64       
 2   temp_max       1461 non-null   float64       
 3   temp_min       1461 non-null   float64       
 4   wind           1461 non-null   float64       
 5   weather        1461 non-null   str           
 6   datetime       1461 non-null   datetime64[us]
dtypes: datetime64[us](1), float64(4), str(2)
memory usage: 80.0 KB
```





###### csv导入时直接解析日期

```py
#设置parse_dates参数修改格式
df = pd.read_csv('./data/weather.csv',parse_dates=['date'])   #解析date列,解析为dates格式
df.info()   #发现date列直接被转换成datetime格式了
```

```python
<class 'pandas.DataFrame'>
RangeIndex: 1461 entries, 0 to 1460
Data columns (total 6 columns):
 #   Column         Non-Null Count  Dtype         
---  ------         --------------  -----         
 0   date           1461 non-null   datetime64[us]
 1   precipitation  1461 non-null   float64       
 2   temp_max       1461 non-null   float64       
 3   temp_min       1461 non-null   float64       
 4   wind           1461 non-null   float64       
 5   weather        1461 non-null   str           
dtypes: datetime64[us](1), float64(4), str(1)
memory usage: 68.6 KB
```

```py
#这时直接完成周几转换
df['week'] = df.date.dt.day_name()
```





###### 设置日期数据作为索引,通过日期索引切片选择数据

```py
df.set_index('date',inplace = True)    #inplace参数直接代替原来的df的索引
print(df.loc['2013-01-01':'2013-02-10'])  #日期索引切片
```



###### 时间间隔数据类型(Timedelta)

```py
#设置数据
d1 = pd.Timestamp('2026-06-14')
d2 = pd.Timestamp('2005-09-14')
d3 = d1-d2
print(d3)     #7578 days 00:00:00
print(type(d3))  #<class 'pandas.Timedelta'>,两个时间的间隔
```





###### 实例

这钩八例子有啥用

```py
#导入数据,解析date为日期型
df = pd.read_csv('weather.csv',parse_dates=['date']) 

#设置日期列为索引
df.set_index(['date'],inplace=True)

#查看基本信息
df.info()

#间隔列表示的是每一天与date的0索引处(2012-01-01)的差值
df['间隔'] = df.date-df.date.iloc[0]  #时间间隔数据类型
```

```python
<class 'pandas.DataFrame'>
DatetimeIndex: 1461 entries, 2012-01-01 to 2015-12-31
Data columns (total 7 columns):
 #   Column         Non-Null Count  Dtype          
---  ------         --------------  -----          
 0   date           1461 non-null   datetime64[us] 
 1   precipitation  1461 non-null   float64        
 2   temp_max       1461 non-null   float64        
 3   temp_min       1461 non-null   float64        
 4   wind           1461 non-null   float64        
 5   weather        1461 non-null   str            
 6   间隔             1461 non-null   timedelta64[us]#时间间隔数据类型
dtypes: datetime64[us](1), float64(4), str(1), timedelta64[us](1)
memory usage: 91.3 KB
```

```py
date	precipitation	temp_max	temp_min	wind	weather	间隔
date							
2012-01-01	2012-01-01	0.0	12.8	5.0	4.7	drizzle	0 days
2012-01-02	2012-01-02	10.9	10.6	2.8	4.5	rain	1 days
2012-01-03	2012-01-03	0.8	11.7	7.2	2.3	rain	2 days
2012-01-04	2012-01-04	20.3	12.2	5.6	4.7	rain	3 days
2012-01-05	2012-01-05	1.3	8.9	2.8	6.1	rain	4 days
...	...	...	...	...	...	...	...
2015-12-27	2015-12-27	8.6	4.4	1.7	2.9	rain	1456 days
2015-12-28	2015-12-28	1.5	5.0	1.7	1.3	rain	1457 days
2015-12-29	2015-12-29	0.0	7.2	0.6	2.6	fog	1458 days
2015-12-30	2015-12-30	0.0	5.6	-1.0	3.4	sun	1459 days
2015-12-31	2015-12-31	0.0	5.6	-2.1	3.5	sun	1460 days
1461 rows × 7 columns
```



###### 时间序列  pd.date_range

```py
#创建时间序列
#写法一
days = pd.date_range('2025-01-23','2026-02-12',freq="W")
#生成起始时间和结束时间内的所有时间,freq设置的是频率,该频率为周
print(days)   

#写法二
days = pd.date_range('2025-01-23',periods=10,freq="YE")
#这个写法,写的不是结束时间,写的是周期,这里周期为年尾
#(就是从起始时间开始,一共需要多少个数据,这里设置的是10个)
print(days)

#频率参数
M,Y,Q等需要E或者S
```



###### 重新采样 resample

```py
df = pd.read_csv('./data/weather.csv',parse_dates=['date']) 

#按时间为维度,要重新设置索引
df.set_index('date',inplace=True)

#对temp_max和temp_min都要重新采样,以年底为单位,求平均值
df[['temp_max','temp_min']].resample('YE').mean()
#按年为单位计算temp_max和temp_min的平均值并输出

#结果
			temp_max   temp_min
date		
2012-12-31	15.276776	7.289617
2013-12-31	16.058904	8.153973
2014-12-31	16.995890	8.662466
2015-12-31	17.427945	8.835616

```





### 七.分组聚合(统计)-----groupby

```py
#语法
df.groupby('分组的字段')['聚合的字段'].['聚合函数']
```

###### 导入数据以及数据预处理

```py
#导入数据
df = pd.read_csv('./data/employees.csv')


#数据预处理
#打印department_id这列缺失值数量,发现只有一个
print(df.department_id.isna().sum())    #np.int64(1)
df.dropna(subset=['department_id'],inplace=True)   #仅删除department_id这列有缺失值的行,并重新赋值

```

###### 具体分组操作

```py
#计算不同部门的平均薪资
df.groupby('department_id')   #对部门分组
df.groupby('department_id').groups  #分组并且可以查看分组,每个值包括对应的索引
df.groupby('department_id').get_group(30.0)   #分组并且可以查看具体的某个分组
```



###### 具体聚合操作

```py
#求每组(部门)的薪资总和,不写sum()这种聚合函数是没有输出的
df.groupby('department_id')['salary'].sum()   
#结果
department_id
10.0       4400.0
20.0      19000.0
30.0      24900.0
40.0       6500.0
50.0     156400.0
60.0      28800.0
70.0      10000.0
80.0     304500.0
90.0      58000.0
100.0     51600.0
110.0     20300.0
Name: salary, dtype: float64


#求每组(部门)的薪资平均值,保留两位小数
df1=df.groupby('department_id')['salary'].mean().round(2)   
df1
#排序
df1.sort_values(ascending=False)    #只有一列是Series,不需要指定列,降序
```



###### 计算不同部门不同岗位的人的平均工资

```py
df.groupby(['department_id','job_id']).groups   #分组后再分组
df2=df.groupby(['department_id','job_id'])[['salary']].mean()
df2['salary']=df2['salary'].round(1)  #处理一下数据
df2
#排序
df2.sort_values('salary',ascending=False)
```

###### 最强聚合函数agg

普通的聚合函数（如 `.mean()`）一次只能做一个动作。而 `agg` 的强大在于：它可以一次性执行多个动作，甚至可以自定义动作。

```py
#普通写法--对单列进行操作
data.groupby(['sex','island'])['body_mass_g'].agg(['mean','count']) 

#另一种写法--蒙多想干什么就干什么
data.groupby(['sex','island']).agg({
    'body_mass_g':['mean','count'],
    'bill_length_mm':['max']
})
```



###### apply和transform

**`apply`**,使用是需要传递函数进去

```py
Series
# 1. 准备数据
df = pd.DataFrame({'name': ['alice', 'bob', 'charlie']})
# 2. 写个加工工人的函数（传入一个字符串，返回大写）
def make_upper(text):
    return text.upper()
# 3. 使用 apply 召唤工人
df['name_upper'] = df['name'].apply(make_upper)
```

```py
#可以直接些匿名函数
# 需求：把每个人的年龄加 5 岁
df = pd.DataFrame({'age': [20, 25, 30]})

# 用 lambda 写：x 代表传进来的每一个年龄
df['age_5_years_later'] = df['age'].apply(lambda x: x + 5)
```

```py
DataFrame
df = pd.DataFrame({
    '姓名': ['张三', '李四', '王五', '赵六'],
    '基础工资': [8000, 12000, 6000, 15000]
})
def add_10_percent(salary):
    return salary * 1.1
# 召唤工人：把“基础工资”这一列挨个塞进函数里，结果存入新列
df['涨薪后工资'] = df['基础工资'].apply(add_10_percent)
```

**`transform`**

数据库的开窗函数

```py
df = pd.DataFrame({
    '部门': ['A', 'A', 'B', 'B', 'B'],
    '姓名': ['张三', '李四', '王五', '赵六', '孙七'],
    '工资': [5000, 7000, 6000, 8000, 10000]
})

print("--- 原始数据 ---")
print(df)
print("\n")

# 需求1：给所有人工资涨 10%（单列转换）
df['涨薪后'] = df['工资'].transform(lambda x: x * 1.1)

# 需求2：求每个部门的平均工资，并且贴回每一个人身上（核心用法！）
df['部门平均工资'] = df.groupby('部门')['工资'].transform('mean')

print("--- 处理后 ---")
print(df)
```



### 八,pandas综合案例-企鹅数据分析

###### 1.导入必要的库

```py
import pandas as pd
import numpy as np
```



###### 2.导入数据

```py
data = pd.read_csv('./data/penguins.csv')
data.head(10)
data.info()
```



###### 3.数据清洗

```py
#查看缺失值数量
print(data.isna().sum())  


#结果
species               0
island                0
bill_length_mm        2
bill_depth_mm         2
flipper_length_mm     2
body_mass_g           2
sex                  11
dtype: int64

#删除缺失值,太假了,实际工作你敢这样直接删.?????
data.dropna(inplace=True)  
print(data.isna().sum())
species              0
island               0
bill_length_mm       0
bill_depth_mm        0
flipper_length_mm    0
body_mass_g          0
sex                  0
dtype: int64
```



###### 4.数据特征的构造

```py
data.head(10)  #查看新的10行

#数据类型的转换:sex转换成category枚举型
data.sex=data.sex.astype('category')
data.info()

#构造新的特征列
data['bill_ratio'] = data.bill_length_mm/data.bill_depth_mm
```



###### 5.数据分析

**数据分箱-把体重分成三个等级,分析体重**

```py
data['mass_level']=pd.cut(data.body_mass_g,bins=3,labels= ['低','中','高'])
data.head(10)  #查看新的10行
print(data.mass_level.value_counts())

#结果
mass_level
低    150
中    128
高     55
Name: count, dtype: int64
```



**按岛屿.性别分组**

agg:对数据执行一个或多个「计算操作」，将多行 / 多列数据汇总成一个结果(聚合)

```py
data.groupby(['sex','island'])['body_mass_g'].agg(['mean','count'])
#结果
					mean	count
sex	island		
Female	Biscoe	   4319.375000	80
		Dream	  3446.311475   61
		Torgersen 3395.833333	24
Male	Biscoe	  5104.518072	83
		Dream	  3987.096774	62
		Torgersen 4034.782609	23
```

这啥数据分析,不想笑吗





### 九.pandas综合案例-睡眠质量分析

###### 1.导入必要的库

```py
import pandas as pd
import numpy as np
```



###### 2.导入数据

```py
df = pd.read_csv('./data/sleep.csv')
df.head(10)
df.info()
```



###### 3.数据清洗

```python
df.isna().sum()   #查看缺失值数量
person_id                    0
gender                       0
age                          0
occupation                   0
sleep_duration               0
sleep_quality                0
physical_activity_level      0
stress_level                 0
bmi_category                 0
blood_pressure               0
heart_rate                   0
daily_steps                  0
sleep_disorder             290
dtype: int64

#sleep_disorder这一列几乎全是空值
df.drop(columns='sleep_disorder',inplace=True)  #删除sleep_disorder这一列的缺失值
df.info()       #查看数据情况,发现sleep_disorder已被删除
df.describe()   #简单查看一数据统计信息
```





###### 4.数据特征的构造

```py
#修改数据类型
df['gender']=df.gender.astype('category')
df['occupation']=df.occupation.astype('category')
df['bmi_category']=df.bmi_category.astype('category')

#通过/将血压分成两列
df[['high','low']]=df.blood_pressure.str.split('/',expand=True)  
df

#对睡眠质量进行数据分箱
df['quality__level']=pd.cut(df.sleep_quality,bins=3,labels=['差','中等','优秀'])
df.quality__level.value_counts()
#结果
quality__level
中等    206
优秀    129
差      65
Name: count, dtype: int64
```





###### 5.数据分析

**比如说bmi的分布**

```python
print(df.bmi_category.value_counts())
bmi_category
Overweight     109
Underweight    102
Obese           98
Normal          91
Name: count, dtype: int64
```

根据不同的bmi分组,

```py
df.groupby('bmi_category').agg({	
	'sleep_duration':'mean',
	'sleep_quality':'mean'
	'stress_level':'mean'
	})
#结果
		sleep_duration	sleep_quality	stress_level
bmi_category			
Normal		7.794505	6.342857	4.857143
Obese		8.072449	6.189796	5.765306
Overweight	8.274312	6.101835	5.642202
Underweight	7.982353	5.896078	5.558824
```









# 数据可视化---matplotlib,seaborn



### 一.matplotlib---折线图---plot--一般看数据趋势用折线图

**pip install matplotlib  别忘了下载哦**

###### 导入工具

```py
import matplotlib.pyplot   as plt  
from matplotlib import rcParams   #导入这个包,使中文能正常显示,设置字体集
rcParams['font.family'] = 'SimHei'    #设置成winodws默认字体集,mac是STHeiti
```



###### 创建图表(画布),设置大小---figure(图表)-

```py
就相当于画画,这一步就是拿纸出来
plt.figure(figsize=(10,5))   #plt图表方法,figsize是图表大小,这里设置的是宽是10,高是5
```



###### 导入数据(这里是创建)

```
month = ['1月','2月','3月','4月','5月']

sales = [101,435,244,611,341]
```



###### 绘制折线图---plot

	plt.plot(month,sales,label='员工A组',color='orange',linewidth=2,linestyle='--',marker=o)   #左边是x轴,右边是y轴,label是图例参数,linewidth参数是线的粗细,linestyle参数是设置线的样式(下面有),marker参数是设置圆点



###### 添加标题---title

plt.title('2025年工资',color='red',fontsize=20)  #第一个是标题名字,后面可以设置属性参数



###### 添加坐标轴标签----xlabel,ylabel

plt.xlabel('月份',fontsize=10)

plt.xlabel('工资',fontsize=10)



###### 添加图例---legend

plt.legend(loc='upper left')    #这里只是设置图例位置(这里是左上),真正设置图例内容在绘制折线图时用label



###### 添加网格线--grid

plt.grid(True,alpha=0.5,linestyle='--')   #alpha参数--设置网格线透明度,linestyle参数是设置网格线样式,--是虚线

plt.grid(axis='y')   #只给水平方向添加网格线



###### 设置刻度字体大小---xticks,yticks

plt.xsticks(rotation=0,fontsize=12)   #rotation参数---设置刻度旋转角度



###### 设置y轴的范围(正常是给的默认的值的范围区间)---ylim

plt.ylim(0,650)



###### 在每个数据点上显示数值--for循环＋zip(打包)关键字+text命令

for x,y in zip(month,sales):   #从month和sales里打包遍历

		plt.text(x,y,str(y),ha='center',va='bottom')  #x,y,str(y):前两个x,y表示xy坐标显示的是str(y)(直接用y也可以),ha表示水平居中方式,这里是中心居中,va表示竖直方向居中方式,这里是顶部居中



###### 显示图标----show

plt.show()



### 二.matplotlib---柱状图/条形图---bar--自动优化排版--一般做对比用

图之间参数都差不多,我们直接复制上面的全部,先注释掉我们不需要的

###### 柱状图(bar)

import matplotlib.pyplot as plt
from matplotlib import rcParams
rcParams['font.family'] = 'SimHei'

plt.figure(figsize=(10,7))

#导入数据
subject = ['语文','数学','英语','科学','体育']
scores = [94,82,93,74,84]

#绘制柱状图
plt.bar(subject,scores,label='小钟',color='orange')

#设置图例
plt.legend(loc='upper right')

#设置标题
plt.title('2025年小钟成绩分布',color='red',fontsize=15) 

#设置x,y轴标题

plt.xlabel('科目',fontsize=10)
plt.ylabel('分数',fontsize=10)

plt.grid(axis='y',alpha=0.5,linestyle='--')

显示数值

plt.ylim(0,100)
for x,y in zip(subject,scores):   #从month和sales里打包遍历
		plt.text(x,y,y,ha='center',va='bottom',fontsize=15) 

###### 自动优化排版--tight_layout---聊胜于无

plt.tight_layout()
plt.show()



###### 条形图(就是柱状图倒过来)---barh

import matplotlib.pyplot as plt

from matplotlib import rcParams
rcParams['font.family'] = 'SimHei'

plt.figure(figsize=(10,7))



#导入数据

subject = ['语文','数学','英语','科学','体育']
scores = [94,82,93,74,84]



plt.barh(subject,scores)

别的都一样

plt.title('成绩')



### 三.matplotlib---饼图/环形图---pie

import matplotlib.pyplot as plt
from matplotlib import rcParams
rcParams['font.family'] = 'SimHei'

plt.figure(figsize=(10,7))



things = ['学习','娱乐','运动','睡觉','其他']

times = [4,3,2,5,8,2]

colors1 = ['#66b3ff','#99ff99','#ffcc99','#ff9999','#ff4499']   #设置配色方案,

###### #绘制饼图--尽量不要用超过6个类

plt.pie(times,labels=things,    #前面写的是要被切割的数据,后面要用labels=名字

			autopct='%.1f%%',             #百分比显示,参数是保留几位小数
	
			startangle=90     #调整角度
	
			colors=colors1  #设置配色

)   



plt.title('一天的时间分割')





###### #绘制环形图

plt.pie(times,labels=things,    

			autopct='%.1f%%',             
	
			startangle=90,     
	
			colors=colors1,  
	
			wedgeprops={'width':0.5},   #环形图就是饼图中间挖空,wedgeprop参数就是设置挖空率
	
			pctdistance=0.6   #设置的是百分比数字离圆心的距离

)   

 

###### 设置中心文字

plt.text(0,0,'总计\n100%')   0,0是坐标,后面是文字



###### #绘制爆炸式饼图----将某块移出来一点



explode1=[0.1,0,0,0,0]   #配置突出块的位置
plt.pie(times,labels=things,    
			autopct='%.1f%%',             
			startangle=90,  
			colors=colors1,
            explode = explode1   #设置突出块

			shadow=True   #给突出块设置阴影
	
		) 

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260616211700293.png" alt="image-20260616211700293" style="zoom:33%;" />

### 四.matplotlib---散点图---scatter---折线图没有线(线性回归)---观察两变量相关性

import matplotlib.pyplot as plt
from matplotlib import rcParams
rcParams['font.family'] = 'SimHei'

plt.figure(figsize=(10,5))

month = ['1月','2月','3月','4月','5月']
sales = [300,435,244,611,334]



plt.scatter(month,sales,label='员工A组',color='orange',linewidth=1,linestyle='--',marker='o')
plt.title('2025年工资',color='red',fontsize=10) 
plt.xlabel('月份',fontsize=10)
plt.ylabel('工资(K)',fontsize=10)
plt.legend(loc='upper left')
plt.grid(axis=1,alpha=0.5,linestyle='--')   #alpha参数--设置网格线透明度,linestyle参数是设置网格线样式,--是虚线
plt.xticks(rotation=30,fontsize=15)
plt.ylim(0,650)
for x,y in zip(month,sales):   #从month和sales里打包遍历
		plt.text(x,y,y,ha='center',va='bottom',fontsize=15) 
plt.show()







### 五.matplotlib---箱线图---boxplot---数据分布,异常IQR,跟分位数有关



![image-20260617174727614](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260617174727614.png)



IQR看实战异常值处理用到了

import matplotlib.pyplot as plt

data = {
    '语文': [82, 85, 88, 70, 90, 76, 84, 83, 95],
    '数学': [75, 80, 79, 93, 88, 82, 87, 89, 92],
    '英语': [70, 72, 68, 65, 78, 80, 85, 90, 95]
}

plt.figure(figsize=(8, 6))  #设置画布
plt.**boxplot**(data.values(), tick_labels=data.keys())  

plt.title("各科成绩分布（箱线图）")
plt.ylabel("分数")
plt.grid(True, axis='y', linestyle='--', alpha=0.5)
plt.show()

![image-20260617175156796](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260617175156796.png)

看不太懂

### 六.matplotlib--在一个画布上绘制多个图表

import matplotlib.pyplot as plt
from matplotlib import rcParams
rcParams['font.family'] = 'SimHei'

subject = ['语文','数学','英语','科学','体育']
scores = [94,82,93,74,84]

###### 创建画布---subplot(新方法) 

f1=plt.subplot(2,2,1)----subplot:子图--两行两列的一个图标,1是索引

f1

###### 画第一个图表

f1.plot(month,sales)

###### 画第二个图表-每画一个图,创建一个小画布

f2=plt.subplot(2,2,2)
f2.bar(subject,scores)



### 七.matplotlib综合案例讲解--温度分析

###### 1.导入库

import pandas as pd

import matplotlib.pyplot  as plt

#设置中文

from matplotlib import rcParams

rcParams['font.sans-serif'] = 'SimHei'

###### 2.导入数据

df = pd.read_csv('weather.csv')



###### 3.处理数据

df['date']=pd.to_datatime(df.date)

df.isna().sum()

```python
date             0
precipitation    0
temp_max         0
temp_min         0
wind             0
weather          0
dtype: int64
```

###### 4.绘制气温趋势变化图

plt.figure(figsize=(10,10))   #创建画布
df=df[df['date'].dt.year==2015]   #数据太多了,这里用布尔索引取一年的数据
plt.plot(df.date,df.temp_max,label='最高气温')
plt.plot(df.date,df.temp_min,label='最低气温')   #表里可以显示两条线
plt.title('2015气温趋势变化')
plt.xlabel('日期')
plt.ylabel('温度')
plt.legend(loc='upper left')
plt.grid(axis='y')
df['temp_mean']=df.temp_max+df.temp_min

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260617204823796.png" alt="image-20260617204823796" style="zoom:25%;" />

用他们的平均气温画图,算一下就行

df['temp_mean']=(df.temp_max+df.temp_min)/2
plt.plot(df.date,df.temp_mean,label='平均气温')
plt.title('2015平均气温趋势变化')
plt.xlabel('日期')
plt.ylabel('温度')
plt.legend(loc='upper left')
plt.grid(axis='y')

###### 5.绘制降水量的直方图----直方图的绘制--hist

df.head()

plt.hist(df.precipitation,bins=5)

类似数据分箱

x轴表示分箱的区间,y轴表示个数

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260617205431691.png" alt="image-20260617205431691" style="zoom:33%;" />



### 八.seaborn绘图的学习

较高级的图用seaborn

```py
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
rcParams['font.sans-serif'] = 'SimHei'
df = pd.read_csv('penguins.csv')
```



| species | island | bill_length_mm | bill_depth_mm | flipper_length_mm | body_mass_g |    sex |        |
| ------: | -----: | -------------: | ------------: | ----------------: | ----------: | -----: | ------ |
|       0 | Adelie |      Torgersen |          39.1 |              18.7 |       181.0 | 3750.0 | Male   |
|       1 | Adelie |      Torgersen |          39.5 |              17.4 |       186.0 | 3800.0 | Female |
|       2 | Adelie |      Torgersen |          40.3 |              18.0 |       195.0 | 3250.0 | Female |
|       3 | Adelie |      Torgersen |           NaN |               NaN |         NaN |    NaN | NaN    |
|       4 | Adelie |      Torgersen |          36.7 |              19.3 |       193.0 | 3450.0 | Female |



#直方图

sns.histplot(data=df,x='species')   **#直接告诉数据集,选择列名就可以了**

 

#核密度图

  sns.kdeplot(data=df，x="bill_length_mm")                                                                                                                                                                                                                                                                                                                                                    











# 项目实战---房地产市场洞察与价值评估

### 项目介绍

###### 字段名	含义	说明

city	城市	房屋所在的城市名称，例如“合肥”、“重庆”等。
address	详细地址	房屋的具体位置，包含街道、交叉口等信息。
area	面积	房屋的面积，单位为平方米（㎡）。
floor	楼层	房屋所在的楼层信息，例如“中层（共18层）”。
name	小区名称	房屋所在的小区或楼盘名称。
price	价格	房屋的总价，单位为“万”或“元”。
province	省份	房屋所在的省份或直辖市名称。
rooms	户型	房屋的户型结构，例如“3室2厅”。
toward	朝向	房屋的朝向，例如“南北向”、“南向”等。
unit	单价	房屋的单价，单位为“元/㎡”。
year	建造年份	房屋的建造年份，例如“2013年建”。
origin_url	原始链接	房屋信息的来源网页链接。



###### **分析及统计问题**

| 编号 | 问题                                                 | 分析主题         | 分析目标                       | 分组字段                | 指标/方法                     |
| ---- | ---------------------------------------------------- | ---------------- | ------------------------------ | ----------------------- | ----------------------------- |
| A1   | 哪些变量最影响房价？面积、楼层、房间数哪个影响更大？ | 特征相关性       | 了解房屋各特征对房价的线性影响 | 无                      | 皮尔逊相关系数                |
| A2   | 全国房价总体分布是怎样的？是否存在极端值？           | 描述性统计       | 概览数值型字段的分布特征       | 无                      | 平均数/中位数/四分位数/标准差 |
| A3   | 哪些城市房价最高？直辖市与非直辖市差异如何？         | 城市对比         | 比较不同城市房价水平           | city                    | 均价/单价中位数/箱线图        |
| A4   | 高价房在面积、楼层等方面有什么特征？                 | 价格分层         | 识别不同价位房屋特征差异       | 价格分段(低中高)        | 列联表/卡方检验               |
| A5   | 哪种户型最受欢迎？三室比两室贵多少？                 | 户型分析         | 分析不同户型的市场表现         | rooms                   | 占比/平均单价/溢价率          |
| A6   | 南北向是否真比单一朝向贵？贵多少？                   | 朝向溢价         | 评估不同朝向的价格差异         | toward                  | 方差分析/多重比较             |
| A7   | 新房比10年老房贵多少？折旧规律如何？                 | 楼龄效应         | 研究建筑年份对房价的影响       | year分段(5年间隔)       | 趋势线/回归分析               |
| A8   | 哪些区域交易最活跃？新区和老城区哪个更贵？           | 区域热度         | 识别各城市热门交易区域         | address(提取区域关键词) | 交易量/价格增长率             |
| A9   | 哪个面积段的性价比最高？超大户型有溢价吗？           | 面积区间         | 分析不同面积段的价格特征       | area分段(50㎡间隔)      | 密度图/价格梯度               |
| A10  | 中层真的比高层贵吗？差价是多少？                     | 楼层差异         | 比较不同楼层的价格表现         | floor(高中低层)         | Kruskal-Wallis检验            |
| A11  | 直辖市房价是否显著更高？单价和总价差异如何？         | 直辖市vs非直辖市 | 对比直辖市与非直辖市的房价差异 | province                | 独立样本t检验/曼-惠特尼U检验  |

### 数据导入

###### 导入库 

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib import rcParams
rcParams['font.family']='SimHei'

######  导入数据
df = pd.read_csv('house_sales.csv')
######  数据概览
print(df.head())
print(df.info())
print('数据量',len(df))
print('字段量',len(df.columns))



###### 数据清洗处理

#数据清洗
#1.删除无用的列,发现url这一列是不需要参加数据分析的
df.drop(columns='origin_url',inplace=True)
df.head()

#2.检查缺失值并删除缺失值
df.isna().sum()
df.dropna(inplace=True)
print(df.isna().sum())
print(len(df))

#3.检查是否有重复值并删除重复数据
df.duplicated().sum()
df.drop_duplicates(inplace=True)
print('缺失值数量为',df.duplicated().sum())
print('总数据量条数为',len(df))





### 数据类型转换

#查看数据查看数据类型
df.head()

print(df.info())

```python
 0   city      28104 non-null  object
 1   address   28104 non-null  object
 2   area      28104 non-null  object
 3   floor     28104 non-null  object
 4   name      28104 non-null  object
 5   price     28104 non-null  object
 6   province  28104 non-null  object
 7   rooms     28104 non-null  object
 8   toward    28104 non-null  object
 9   unit      28104 non-null  object
 10  year      28104 non-null  object
dtypes: object(11)
memory usage: 2.6+ MB
Non
```

###### 面积的类型转换

#发现area列将来可能需要计算但是是object类型,进行转换
df.area=df.area.str.replace('㎡',' ').astype(float) # 去掉单位㎡转换成浮点型,存入原表



###### 售价price的类型转换

#发现将来售价可能进行计算,去掉单位并转换成浮点型
df.price=df.price.str.replace('万','').astype(float)



###### 朝向的数据类型的转换

#可以转换成 分类类型
#先查看各朝向分类情况
df.toward.astype('category')  #发现分成10类
#传回去
df.toward=df.toward.astype('category') 



###### 单位面积售价 的数据类型转换

#unit与price转换方式和类型类似
df.unit=df.unit.str.replace('元/㎡','').astype(float)



###### 房屋建造年份 的数据类型转换

#年份可能参与计算
#年份是整数
df.year=df.year.str.replace('年建','').astype(int)



###### 查看最终情况

print(df.info())
df.head()

```python
<class 'pandas.core.frame.DataFrame'>
Index: 28104 entries, 0 to 106088
Data columns (total 11 columns):
 #   Column    Non-Null Count  Dtype   
---  ------    --------------  -----   
 0   city      28104 non-null  object  
 1   address   28104 non-null  object  
 2   area      28104 non-null  float64 
 3   floor     28104 non-null  object  
 4   name      28104 non-null  object  
 5   price     28104 non-null  float64 
 6   province  28104 non-null  object  
 7   rooms     28104 non-null  object  
 8   toward    28104 non-null  category
 9   unit      28104 non-null  float64 
 10  year      28104 non-null  int64   
dtypes: category(1), float64(3), int64(1), object(6)
memory usage: 2.4+ MB
None
```





### 异常值处理

###### 先查看几条数据

df.head()

###### 房屋面积的异常处理

#找出房屋面积过大或过小的值
print(len(df[df.area<20]))
print(len(df[df.area>500]))
df[df.area<20].head() #面积过小

df[df.area>500].head() #面积过大

#查出正常数据的数量
print(len(df[(df.area>20) & (df.area<500)]))
#将正常数据传入原数据中
df = df[(df.area>20) & (df.area<500)]
#再查一次异常数据,发现已经消失
print(len(df[(df.area<20) | (df.area>500)]))



###### #房屋售价的异常值处理  IQR方法---箱线图

Q1 = df.price.quantile(0.25)  #四分之一分位数
Q3 = df.price.quantile(0.75)  #四分之三分位数
IQR = Q3-Q1
low_price = Q1 - 1.5*IQR
high_price  = Q3 + 1.5*IQR
print(df[df.price<low_price])   #发现没有小于的
print(df[df.price>high_price])  #都是大于的
len(df[(df.price>low_price) & (df.price<high_price)])   #统计正常值数量
df = df[(df.price>low_price) & (df.price<high_price)]  #传回

### 数据特征构造

###### 地址的地区district构造

df['district']=df.address.str.split('-').str[0]  #先用字符串选择器,用-将address字符串分开,再通过字符串索引取第一个内容并存入新数据特征district
df.head()   #发现已完成

###### 楼层的类型Floor_type构造

df['floor_type']=df.floor.str.split('（').str[0]  #选择floor列用字符串选择器通过（分割,再用str[0]取分割后的第一个内容,即某层,最后传入新的数据特征列floor_type
df.sample(5)   #随机取样发现已完成

###### 是否是直辖市zxs构造

#使用匿名函数写
df['直辖市']=df.city.apply(lambda x: '是' if x in ['北京','上海','天津','重庆'] else '否')
df.sample(5)   #随机取样发现已完成

###### 卧室的数量bed_num

df['bed_num']=df.rooms.str.split('室').str[0].astype(int)
df.sample(5)

###### 客厅的数量live_num---正则表达式方法

#df['live_num']=df.rooms.str.split('室').str[1].str.split('厅').str[0].astype(int)  #分割后再次分割取数字,该方法较麻烦
**正则表达式方法**
df['live_num']=df.rooms.str.extract(r'(\d+)厅').astype(int)  #取的是厅前面的数字
df.sample(5)

###### 楼龄building_age

from datetime import datetime  #引入时间模块
df['building_age']=datetime.now().year-df.year   #使用当前时间
df.sample(5)

###### 价格的分段price_labels----分箱操作

df['price_labels']=pd.cut(df.price,bins=4,labels=['低价','中价','高价','豪华'])
df.sample(5)

最终情况

![image-20260623010112060](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623010112060.png)



### 具体问题分析

#### 特征相关值---

##### 问题编号:A1

问题:哪些变量最影响房价?面积、单价、房龄哪个影响更大?
分析主题:特征相关性
分析目标:了解房屋各特征对房价的线性影响
分组字段:无
指标/方法:皮尔逊相关系数

###### #选择数值型特征

a=df[['price','area','unit','building_age']].corr()  #corr:皮尔逊相关系数方法

出现一个对称矩阵,值越大,两个参数的相关性越高,负相关去相反



|              |    price |      area |      unit | building_age |
| -----------: | -------: | --------: | --------: | -----------: |
|        price | 1.000000 |  0.437029 |  0.730997 |     0.087499 |
|         area | 0.437029 |  1.000000 | -0.201596 |    -0.087509 |
|         unit | 0.730997 | -0.201596 |  1.000000 |     0.197860 |
| building_age | 0.087499 | -0.087509 |  0.197860 |     1.000000 |

###### #对房价的影响最大的几个因素的排序

a.price.sort_values(ascending=False)[1:]  #[1:]是切片切掉price自己

```python
unit            0.730997
area            0.437029
building_age    0.087499
Name: price, dtype: float64
```

影响最大的是每平米单价,接着是大小,最后是房龄

###### 相关性的热力图

plt.figure(figsize = (10,10))

sns.heatmap(a)

越红相关度越高

![image-20260623012015204](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623012015204.png)

#### 房价分布直方图

##### 问题编号: A2

 问题: 全国房价总体分布是怎样的？是否存在极端值？
 分析主题: 描述性统计
 分析目标: 概览数值型字段的分布特征
 分组字段: 无
 指标/方法: 平均数/中位数/四分位数/标准差

###### 描述性信息

df.describe()

|       |         area |        price |         unit |         year |     live_num | building_age |
| ----: | -----------: | -----------: | -----------: | -----------: | -----------: | ------------ |
| count | 25382.000000 | 25382.000000 | 25382.000000 | 25382.000000 | 25382.000000 | 25382.000000 |
|  mean |   102.683130 |   112.218826 | 11273.759869 |  2013.105468 |     1.845205 | 12.894532    |
|   std |    32.827656 |    54.400754 |  5354.765656 |     6.024982 |     0.407410 | 6.024982     |
|   min |    21.000000 |     9.000000 |  1000.000000 |  1976.000000 |     0.000000 | 3.000000     |
|   25% |    85.000000 |    70.307500 |  7520.250000 |  2011.000000 |     2.000000 | 9.000000     |
|   50% |    99.000000 |   100.000000 | 10151.000000 |  2015.000000 |     2.000000 | 11.000000    |
|   75% |   122.000000 |   145.000000 | 13846.000000 |  2017.000000 |     2.000000 | 15.000000    |
|   max |   470.000000 |   266.300000 | 85288.000000 |  2023.000000 |    12.000000 | 50.000000    |

###### 房价分布直方图
plt.subplot()  # 1行2列的第2个图
plt.hist(df['price'], bins=30, color='skyblue')
plt.title('房价分布直方图')
plt.xlabel('价格(元)')
plt.ylabel('房屋数量')
plt.tight_layout()  # 避免标题重叠
plt.show()

![image-20260623012934611](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623012934611.png)



#### 城市房价分析

##### 问题编号: A3

 问题: 哪些城市房价最高？直辖市与非直辖市差异如何？
 分析主题: 城市对比
 分析目标: 比较不同城市房价水平
 分组字段: city
 指标/方法: 均价/单价中位数/箱线图

###### *# 按城市统计*

 city_stats = df.groupby('city').agg({
   'price': ['mean', 'median', 'count'],
   'unit': ['mean', 'median']
 })
 print("\n各城市房价统计:")
 display(city_stats.sort_values(('unit', 'mean'), ascending=False).head(10))

```python
各城市房价统计:
```

|      |            |        | price |              |    unit |
| ---: | ---------: | -----: | ----: | -----------: | ------: |
|      |       mean | median | count |         mean |  median |
| city |            |        |       |              |         |
| 深圳 | 219.571429 |  228.0 |     7 | 41859.428571 | 38012.0 |
| 上海 | 172.206250 |  199.5 |    48 | 25832.645833 | 26439.0 |
| 福州 | 180.447619 |  185.0 |    42 | 24473.642857 | 21869.5 |
| 淳安 | 164.260000 |  188.5 |    30 | 24298.000000 | 22601.0 |
| 蓟州 | 178.643438 |  170.5 |    64 | 24271.500000 | 20175.0 |
| 厦门 | 190.966667 |  200.0 |    15 | 23965.266667 | 25628.0 |
| 桐庐 | 161.672564 |  175.0 |    39 | 23774.205128 | 23263.0 |
| 永泰 | 169.945455 |  174.5 |    44 | 22960.704545 | 21349.0 |
| 苏州 | 191.036207 |  196.0 |    58 | 22837.637931 | 22109.0 |
| 三亚 | 188.647059 |  190.0 |    34 | 22732.441176 | 21236.5 |

######  *#* *可视化前10城市*

 top_cities = city_stats.sort_values(('unit', 'mean'), ascending=False).head(10).index
 df_top = df[df['city'].isin(top_cities)]

 plt.figure(figsize=(12, 6))
 sns.boxplot(x='city', y='price', data=df_top, order=top_cities)
 plt.title('TOP10城市房价分布对比', fontsize=14)
 plt.xlabel('城市')
 plt.ylabel('价格(元)')
 plt.xticks(rotation=45)
 plt.tight_layout()
 plt.show()

![image-20260623014807694](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623014807694.png)

##### 问题编号: A4

问题: 哪种户型最受欢迎？三室比两室贵多少？
分析主题: 户型分析
分析目标: 分析不同户型的市场表现
分组字段: rooms
指标/方法: 占比/平均单价/溢价率

###### 1. 统计各户型（按卧室数量）的房源数量及占比

bed_stats = df['bed_num'].value_counts().sort_index()
bed_percent = (bed_stats / len(df) * 100).round(2)
print("各卧室数量房源数量及占比：")
print(pd.DataFrame({'数量': bed_stats, '占比(%)': bed_percent}))

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623172402606.png" alt="image-20260623172402606" style="zoom:50%;" />

###### 2. 计算各卧室数量的平均单价

avg_unit = df.groupby('bed_num')['unit'].mean().round(2)
print("\n各卧室数量平均单价（元/㎡）：")
print(avg_unit)

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623172429308.png" alt="image-20260623172429308" style="zoom:50%;" />

###### 3. 确定最受欢迎的户型（数量最多）

most_popular = bed_stats.idxmax()
print(f"\n最受欢迎的户型：{most_popular}室，共有 {bed_stats.max()} 套，占比 {bed_percent.max():.2f}%")

```python
最受欢迎的户型：3室，共有 13905 套，占比 53.20%
```

###### 4. 以两室为基准（bed_num=2）计算溢价率

base_unit = avg_unit[2]  # 两室平均单价
premium = ((avg_unit - base_unit) / base_unit * 100).round(2)
premium.name = '溢价率(%)'
print("\n各户型相对两室的溢价率：")
print(premium)

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623172538630.png" alt="image-20260623172538630" style="zoom:50%;" />

###### #5. 可视化：数量和平均单价

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

#左图：数量分布
bed_stats.plot(kind='bar', ax=axes[0], color='skyblue')
axes[0].set_title('各卧室数量房源数量')
axes[0].set_xlabel('卧室数量')
axes[0].set_ylabel('房源数量')
for i, v in enumerate(bed_stats):
    axes[0].text(i, v + 10, str(v), ha='center')

#右图：平均单价
avg_unit.plot(kind='bar', ax=axes[1], color='salmon')
axes[1].set_title('各卧室数量平均单价')
axes[1].set_xlabel('卧室数量')
axes[1].set_ylabel('平均单价 (元/㎡)')
for i, v in enumerate(avg_unit):
    axes[1].text(i, v + 200, f'{v:.0f}', ha='center')

plt.tight_layout()
plt.show()

![image-20260623172730081](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623172730081.png)

###### 6. 更细致的户型组合分析

print("\n各具体户型（卧室+客厅）组合的数量及均价：")
room_detail = df.groupby('rooms').agg(
    数量=('rooms', 'count'),
    均价=('unit', 'mean')
).sort_values('数量', ascending=False)
print(room_detail.head(10))

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623172749491.png" alt="image-20260623172749491" style="zoom:50%;" />



##### 问题编号: A5

问题: 新房比10年老房贵多少？折旧规律如何？
分析主题: 楼龄效应
分析目标: 研究建筑年份对房价的影响
分组字段: year分段(5年间隔)
指标/方法: 趋势线/回归分析

from scipy import stats  # 用于线性回归
#确保数据存在，building_age 已计算
#检查 building_age 范围
print(f"楼龄范围: {df['building_age'].min()} ~ {df['building_age'].max()} 年")
print(f"建成年份范围: {df['year'].min()} ~ {df['year'].max()} 年")

```python
楼龄范围: 3 ~ 50 年
建成年份范围: 1976 ~ 2023 年
```

###### #---------- 1. 按年份分箱（5年间隔）统计平均单价 ----------

#创建年份区间标签：例如 2000-2004, 2005-2009, ...
bins = range(1975, 2026, 5)  #从1975到2025，步长5
labels = [f"{i}-{i+4}" for i in bins[:-1]]  #生成区间标签

df['year_group'] = pd.cut(df['year'], bins=bins, labels=labels, right=False)

#按年份分组计算平均单价和房源数量
year_group_stats = df.groupby('year_group').agg(
    平均单价=('unit', 'mean'),
    房源数量=('unit', 'count')
).dropna()  #去掉空组

print("\n各年份区间房源统计：")
print(year_group_stats)

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623184120099.png" alt="image-20260623184120099" style="zoom:50%;" />

###### #---------- 2. 绘制折线图：年份 vs 平均单价 ----------

plt.figure(figsize=(12, 6))
plt.plot(year_group_stats.index, year_group_stats['平均单价'], marker='o', linestyle='-', color='b')
plt.title('不同建成年份区间的平均单价')
plt.xlabel('建成年份区间')
plt.ylabel('平均单价 (元/㎡)')
plt.xticks(rotation=45)
plt.grid(True, linestyle='--', alpha=0.7)

#标注每个点的数值
for i, (x, y) in enumerate(zip(year_group_stats.index, year_group_stats['平均单价'])):
    plt.text(x, y + 200, f'{y:.0f}', ha='center', va='bottom', fontsize=9)

plt.tight_layout()
plt.show()

![image-20260623184136085](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623184136085.png)

###### #---------- 3. 按楼龄分组（更直观） ----------

#将楼龄分为几段：0-5, 5-10, 10-15, 15-20, 20+
bins_age = [0, 5, 10, 15, 20, 100]
labels_age = ['0-5年', '5-10年', '10-15年', '15-20年', '20年以上']
df['age_group'] = pd.cut(df['building_age'], bins=bins_age, labels=labels_age, right=False)

age_group_stats = df.groupby('age_group').agg(
    平均单价=('unit', 'mean'),
    房源数量=('unit', 'count')
)

print("\n各楼龄区间统计：")
print(age_group_stats)

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623184203697.png" alt="image-20260623184203697" style="zoom:50%;" />

###### #---------- 4. 计算“新房”与“10年老房”的价差 ----------

#定义新房：楼龄 ≤ 5 年；10年老房：楼龄 10~15 年（含）
new_home = df[df['building_age'] <= 5]['unit'].mean()
old_10 = df[(df['building_age'] >= 10) & (df['building_age'] < 15)]['unit'].mean()

print(f"\n新房（≤5年）平均单价：{new_home:.2f} 元/㎡")
print(f"10年老房（10~15年）平均单价：{old_10:.2f} 元/㎡")
print(f"新房比10年老房贵：{(new_home - old_10):.2f} 元/㎡，溢价率：{(new_home/old_10 - 1)*100:.2f}%")

```python
新房（≤5年）平均单价：9306.84 元/㎡
10年老房（10~15年）平均单价：11865.16 元/㎡
新房比10年老房贵：-2558.31 元/㎡，溢价率：-21.56%
```

###### #---------- 5. 线性回归分析：楼龄与单价的关系 ----------

#使用 building_age 作为自变量 X，unit 作为因变量 y
X = df['building_age'].values.reshape(-1, 1)
y = df['unit'].values

#计算线性回归（使用 scipy.stats.linregress 或 sklearn）
slope, intercept, r_value, p_value, std_err = stats.linregress(df['building_age'], df['unit'])

print("\n========== 线性回归结果 ==========")
print(f"回归方程：单价 = {slope:.2f} * 楼龄 + {intercept:.2f}")
print(f"斜率（每年折旧）：{slope:.2f} 元/㎡/年")
print(f"相关系数 R：{r_value:.4f}")
print(f"R²：{r_value**2:.4f}")
print(f"p-value：{p_value:.4f}  （{'显著' if p_value < 0.05 else '不显著'}）")

```python
========== 线性回归结果 ==========
回归方程：单价 = 192.67 * 楼龄 + 9119.29
斜率（每年折旧）：192.67 元/㎡/年
相关系数 R：0.1991
R²：0.0397
p-value：0.0000  （显著）
```

###### #绘制散点图与回归线

plt.figure(figsize=(10, 6))
plt.scatter(df['building_age'], df['unit'], alpha=0.3, s=10, label='实际数据')
plt.plot(df['building_age'], slope * df['building_age'] + intercept, color='red', linewidth=2, label='回归线')
plt.title('楼龄与单价的线性回归')
plt.xlabel('楼龄 (年)')
plt.ylabel('单价 (元/㎡)')
plt.legend()
plt.grid(True, linestyle='--', alpha=0.5)
plt.tight_layout()
plt.show()

![image-20260623184308489](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623184308489.png)

#### 朝向分析

##### 问题编号: A6

 问题: 南北向是否真比单一朝向贵？贵多少？
 分析主题: 朝向溢价
 分析目标: 评估不同朝向的价格差异
 分组字段: toward
 指标/方法: 方差分析/多重比较

###### *# 筛选主要朝向（出现次数>50次）*

 toward_counts = df['toward'].value_counts()

```python
toward
南北向    14425
南向      8562
东南向      950
东向       410
北向       254
西南向      243
西向       156
东西向      150
西北向      129
东北向      103
Name: count, dtype: int64
```

 main_towards = toward_counts[toward_counts > 50].index
 df_toward = df[df['toward'].isin(main_towards)]

######  *#* *朝向统计*

 toward_stats = df_toward.groupby('toward').agg({
   'price': ['mean', 'median'],
   'unit': 'median',
   'building_age': 'mean'
 }).sort_values(('unit', 'median'), ascending=False)

 print("\n各朝向价格表现:")
 display(toward_stats)

|        |            |  price |    unit | building_age |
| -----: | ---------: | -----: | ------: | -----------: |
|        |       mean | median |  median |         mean |
| toward |            |        |         |              |
| 西南向 | 133.369959 |  130.0 | 13152.0 |    14.526749 |
|   西向 |  96.946346 |   84.0 | 12384.0 |    14.346154 |
| 西北向 | 113.793101 |  105.0 | 12290.0 |    14.410853 |
| 东北向 | 111.439903 |  100.0 | 12041.0 |    13.446602 |
|   北向 |  89.504724 |   75.0 | 11455.0 |    14.177165 |
|   东向 | 106.186439 |   90.0 | 11281.5 |    13.682927 |
| 东南向 | 111.175579 |  102.0 | 10800.0 |    11.931579 |
|   南向 | 109.883707 |  100.0 | 10573.0 |    12.541696 |
| 南北向 | 114.196544 |  100.0 |  9785.0 |    13.023847 |
| 东西向 |  97.674667 |   82.0 |  8968.0 |    16.533333 |

######  *#* *方差分析*

 groups = [group['unit'].values for name, group in df_toward.groupby('toward')]

######  *#* *可视化*

 plt.figure(figsize=(12, 6))
 sns.boxplot(x='toward', y='unit', data=df_toward,
       order=toward_stats.index)
 plt.title('不同朝向单价分布', fontsize=14)
 plt.xticks(rotation=45)
 plt.tight_layout()
 plt.show()

![image-20260623013751035](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623013751035.png)

发现南北向和南向的更多



##### 问题编号: A7

问题: 哪些区域交易最活跃？新区和老城区哪个更贵？
分析主题: 区域热度
分析目标: 识别各城市热门交易区域
分组字段: address(提取区域关键词)
指标/方法: 交易量/价格增长率

###### #1. 统计各区域的房源数量

district_count = df['district'].value_counts().reset_index()
district_count.columns = ['district', '房源数量']

###### #2. 统计各区域的平均单价和楼龄

district_stats = df.groupby('district').agg(
    平均单价=('unit', 'mean'),
    平均楼龄=('building_age', 'mean')
).reset_index()

###### #3. 合并

district_info = pd.merge(district_stats, district_count, on='district', how='inner')

###### #4. 排序 - 使用正确的列名

district_info = district_info.sort_values('房源数量', ascending=False)
print("各区域统计（前20名）：")
print(district_info.head(20))

<img src="C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623184505479.png" alt="image-20260623184505479" style="zoom:50%;" />

###### #---------- 可视化：交易最活跃的15个区域 ----------

top15 = district_info.head(15)

plt.figure(figsize=(12, 6))
bars = plt.bar(top15['district'], top15['房源数量'], color='steelblue')
plt.title('交易最活跃的15个区域（按房源数量）')
plt.xlabel('区域')
plt.ylabel('房源数量')
plt.xticks(rotation=45, ha='right')
for bar in bars:
    height = bar.get_height()
    plt.text(bar.get_x() + bar.get_width()/2., height + 5, f'{int(height)}', ha='center', va='bottom')
plt.tight_layout()
plt.show()

![image-20260623184526926](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623184526926.png)





###### #---------- 新区和老城区分类 ----------

###### #方法1：关键词分类

def classify_district(name):
    name = str(name)
    new_keywords = ['新区', '新城', '开发区', '工业园', '科技园', '高新区', '新城区']
    old_keywords = ['老城', '旧城', '中心', '老区', '老城区']
    

    for kw in new_keywords:
        if kw in name:
            return '新区'
    for kw in old_keywords:
        if kw in name:
            return '老城区'
    return '其他'

district_info['区域类型'] = district_info['district'].apply(classify_district)

type_stats = district_info.groupby('区域类型').agg(
    平均单价=('平均单价', 'mean'),
    房源总数=('房源数量', 'sum')
).reset_index()

print("\n按关键词分类的区域类型统计：")
print(type_stats)

#可视化
plt.figure(figsize=(8, 5))
sns.barplot(x='区域类型', y='平均单价', data=type_stats, palette='Set2')
plt.title('不同区域类型的平均单价对比')
plt.xlabel('区域类型')
plt.ylabel('平均单价 (元/㎡)')
for i, row in type_stats.iterrows():
    plt.text(i, row['平均单价'] + 200, f"{row['平均单价']:.0f}", ha='center')
plt.tight_layout()
plt.show()

```python
按关键词分类的区域类型统计：
  区域类型          平均单价   房源总数
0   其他  12926.519200  23346
1   新区  11442.546995   2033
2  老城区  12363.567909    756
```

![image-20260623184628111](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623184628111.png)

###### #方法2：按平均楼龄中位数分类

median_age = district_info['平均楼龄'].median()
district_info['按楼龄分类'] = district_info['平均楼龄'].apply(lambda x: '新区' if x < median_age else '老城区')

age_type_stats = district_info.groupby('按楼龄分类').agg(
    平均单价=('平均单价', 'mean'),
    房源总数=('房源数量', 'sum')
).reset_index()

print(f"\n按平均楼龄中位数（{median_age:.1f}年）划分的结果：")
print(age_type_stats)

plt.figure(figsize=(8, 5))
sns.barplot(x='按楼龄分类', y='平均单价', data=age_type_stats, palette='Set3')
plt.title('按楼龄划分的区域类型平均单价')
plt.xlabel('区域类型')
plt.ylabel('平均单价 (元/㎡)')
for i, row in age_type_stats.iterrows():
    plt.text(i, row['平均单价'] + 200, f"{row['平均单价']:.0f}", ha='center')
plt.tight_layout()
plt.show()

```python
按平均楼龄中位数（12.0年）划分的结果：
  按楼龄分类          平均单价   房源总数
0    新区  11537.944457  12719
1   老城区  14006.839842  13416
```

![image-20260623185302113](C:\Users\35181\AppData\Roaming\Typora\typora-user-images\image-20260623185302113.png)

###### #---------- 比较 ----------

#从关键词分类中提取新区和老城区的平均单价
new_avg = type_stats[type_stats['区域类型'] == '新区']['平均单价'].values
old_avg = type_stats[type_stats['区域类型'] == '老城区']['平均单价'].values

if len(new_avg) > 0 and len(old_avg) > 0:
    new_avg = new_avg[0]
    old_avg = old_avg[0]
    print(f"\n新区平均单价：{new_avg:.2f} 元/㎡")
    print(f"老城区平均单价：{old_avg:.2f} 元/㎡")
    print(f"新区比老城区贵：{(new_avg - old_avg):.2f} 元/㎡，溢价率：{(new_avg/old_avg - 1)*100:.2f}%")
else:
    print("\n关键词匹配未产生足够的新区或老城区样本，请检查关键词。")

```python
新区平均单价：11442.55 元/㎡
老城区平均单价：12363.57 元/㎡
新区比老城区贵：-921.02 元/㎡，溢价率：-7.45%
```
