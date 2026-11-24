# MySQL 8 详细教程

## MySQL 8 安装指南

### 1. 下载 MySQL 8

#### Windows 系统安装步骤

**步骤 1: 访问 MySQL 官网下载页面**

打开浏览器，访问 MySQL 官方下载页面：
https://dev.mysql.com/downloads/mysql/

![MySQL官网下载页面](图片占位符: MySQL官网下载页面.png)

**步骤 2: 选择操作系统和版本**

- 在页面中找到 "Select Operating System" 下拉菜单
- 选择 "Microsoft Windows"
- 选择 MySQL 8.0.x 版本（推荐选择最新的稳定版本）

![选择操作系统和版本](图片占位符: 选择操作系统和版本.png)

**步骤 3: 下载安装包**

点击 "Download" 按钮下载 MySQL Installer：
- 选择 "mysql-installer-community-8.0.x.x.msi" （完整安装包，推荐）
- 或者选择 "mysql-installer-web-community-8.0.x.x.msi" （网络安装包，较小）

![下载安装包](图片占位符: 下载安装包.png)

**步骤 4: 开始安装**

双击下载的安装包，启动安装向导：
- 选择安装类型：推荐选择 "Developer Default"（开发者默认）
- 点击 "Next" 继续

![安装向导](图片占位符: 安装向导.png)

**步骤 5: 检查安装要求**

安装程序会自动检查系统要求：
- 确保所有依赖项都已满足
- 点击 "Execute" 执行必要操作
- 完成后点击 "Next"

![检查安装要求](图片占位符: 检查安装要求.png)

**步骤 6: 选择安装组件**

在 "Installation" 页面中：
- MySQL Server: 必选，数据库服务器
- MySQL Workbench: 推荐，图形化管理工具
- MySQL Shell: 推荐，命令行工具
- MySQL Connector/ODBC: 可选，ODBC驱动
- MySQL Connector/Python: 可选，Python驱动

![选择安装组件](图片占位符: 选择安装组件.png)

**步骤 7: 执行安装**

- 点击 "Execute" 开始安装
- 等待所有组件安装完成
- 点击 "Next" 继续

![执行安装](图片占位符: 执行安装.png)

**步骤 8: 配置 MySQL Server**

进入配置向导：
- **配置类型和网络**：
  - Config Type: Development Computer（开发电脑）
  - Port: 3306（默认端口）
  - Open Windows Firewall port for network access: 勾选

![配置类型和网络](图片占位符: 配置类型和网络.png)

- **身份验证方式**：
  - 选择 "Use Strong Password Encryption"（推荐）

![身份验证方式](图片占位符: 身份验证方式.png)

- **设置 root 密码**：
  - 输入 root 用户密码（请牢记！）
  - 重复输入密码确认
  - 点击 "Next"

![设置root密码](图片占位符: 设置root密码.png)

- **Windows 服务配置**：
  - Configure MySQL Server as a Windows Service: 勾选
  - Windows Service Name: MySQL80
  - Start the MySQL Server at System Startup: 勾选（推荐）
  - Run Windows Service as: Standard System Account

![Windows服务配置](图片占位符: Windows服务配置.png)

- **应用配置**：
  - 点击 "Execute" 应用所有配置
  - 等待配置完成
  - 点击 "Finish" 完成

![应用配置](图片占位符: 应用配置.png)

**步骤 9: 完成安装**

- 点击 "Next" 继续
- 点击 "Finish" 完成整个安装过程

![完成安装](图片占位符: 完成安装.png)

### 2. 验证安装

#### 方法一：使用命令行验证

1. 打开命令提示符（CMD）或 PowerShell
2. 输入以下命令检查 MySQL 服务状态：

```bash
net start MySQL80
```

如果服务正在运行，会显示服务已启动的信息。

3. 登录 MySQL：

```bash
mysql -u root -p
```

输入之前设置的 root 密码，如果成功登录，会看到 MySQL 提示符：

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.33 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

4. 查看版本信息：

```sql
SELECT VERSION();
```

![命令行验证](图片占位符: 命令行验证.png)

#### 方法二：使用 MySQL Workbench 验证

1. 打开 MySQL Workbench
2. 点击首页的 "MySQL Connections" 中的本地连接
3. 输入 root 密码
4. 如果成功连接，会显示 Workbench 主界面

![MySQL Workbench连接](图片占位符: MySQLWorkbench连接.png)

### 3. 配置环境变量（可选但推荐）

为了在任意位置都能使用 MySQL 命令，建议配置环境变量：

**步骤 1: 找到 MySQL 安装路径**

默认安装路径为：`C:\Program Files\MySQL\MySQL Server 8.0\bin`

**步骤 2: 添加到系统环境变量**

1. 右键点击 "此电脑"，选择 "属性"
2. 点击 "高级系统设置"
3. 点击 "环境变量"
4. 在 "系统变量" 中找到 "Path"，点击 "编辑"
5. 点击 "新建"，添加 MySQL bin 目录路径
6. 点击 "确定" 保存

![配置环境变量](图片占位符: 配置环境变量.png)

**步骤 3: 验证环境变量**

重新打开命令提示符，输入：

```bash
mysql --version
```

如果显示 MySQL 版本信息，说明配置成功。

### 4. 常见安装问题解决

#### 问题 1: 安装失败，提示缺少 Visual C++ Redistributable

**解决方案：**
- 下载并安装 Visual C++ Redistributable for Visual Studio 2015-2022
- 下载地址：https://aka.ms/vs/17/release/vc_redist.x64.exe

#### 问题 2: 服务启动失败

**解决方案：**
1. 检查端口 3306 是否被占用：

```bash
netstat -ano | findstr :3306
```

2. 如果被占用，可以修改 MySQL 配置文件中的端口号
3. 或者停止占用端口的其他服务

#### 问题 3: 无法连接到 MySQL 服务器

**解决方案：**
1. 检查 MySQL 服务是否正在运行：

```bash
net start MySQL80
```

2. 检查防火墙设置
3. 检查 root 密码是否正确

### 5. 安装后首次配置建议

#### 5.1 修改默认字符集

编辑 MySQL 配置文件 `my.ini`（位于 `C:\ProgramData\MySQL\MySQL Server 8.0\`），添加以下内容：

```ini
[client]
default-character-set=utf8mb4

[mysql]
default-character-set=utf8mb4

[mysqld]
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
```

重启 MySQL 服务：

```bash
net stop MySQL80
net start MySQL80
```

#### 5.2 创建普通用户（安全考虑）

```sql
-- 创建用户
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'MyPassword123!';

-- 授予权限
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'localhost' WITH GRANT OPTION;

-- 刷新权限
FLUSH PRIVILEGES;
```

#### 5.3 配置远程访问（如需要）

```sql
-- 创建允许远程访问的用户
CREATE USER 'remoteuser'@'%' IDENTIFIED BY 'RemotePassword123!';

-- 授予权限
GRANT ALL PRIVILEGES ON *.* TO 'remoteuser'@'%' WITH GRANT OPTION;

-- 刷新权限
FLUSH PRIVILEGES;
```

### 6. 卸载 MySQL（如需要）

如果需要完全卸载 MySQL：

1. 停止 MySQL 服务：

```bash
net stop MySQL80
```

2. 在控制面板中卸载 MySQL 相关程序

3. 删除 MySQL 安装目录和数据目录：
   - `C:\Program Files\MySQL\`
   - `C:\ProgramData\MySQL\`

4. 删除注册表相关项（谨慎操作）

---

# 基础篇

## 通用语法及分类

- DDL: 数据定义语言，用来定义数据库对象（数据库、表、字段）
- DML: 数据操作语言，用来对数据库表中的数据进行增删改
- DQL: 数据查询语言，用来查询数据库中表的记录
- DCL: 数据控制语言，用来创建数据库用户、控制数据库的控制权限

### DDL（数据定义语言）

数据定义语言

#### 数据库操作

查询所有数据库：
```sql
SHOW DATABASES;
```

**示例输出：**
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

查询当前数据库：
```sql
SELECT DATABASE();
```

**示例输出：**
```
+------------+
| DATABASE() |
+------------+
| NULL       |
+------------+
1 row in set (0.00 sec)
```

创建数据库：
```sql
CREATE DATABASE [ IF NOT EXISTS ] 数据库名 [ DEFAULT CHARSET 字符集] [COLLATE 排序规则 ];
```

**详细示例：**

1. 创建简单数据库：
```sql
CREATE DATABASE mycompany;
```

2. 创建数据库时指定字符集：
```sql
CREATE DATABASE mycompany DEFAULT CHARSET utf8mb4;
```

3. 创建数据库时指定字符集和排序规则：
```sql
CREATE DATABASE mycompany DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

4. 使用 IF NOT EXISTS 避免重复创建错误：
```sql
CREATE DATABASE IF NOT EXISTS mycompany DEFAULT CHARSET utf8mb4;
```

**执行示例：**
```sql
mysql> CREATE DATABASE mycompany DEFAULT CHARSET utf8mb4;
Query OK, 1 row affected (0.01 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mycompany          |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
```

删除数据库：
```sql
DROP DATABASE [ IF EXISTS ] 数据库名;
```

**详细示例：**

1. 删除数据库：
```sql
DROP DATABASE mycompany;
```

2. 使用 IF EXISTS 避免删除不存在的数据库时报错：
```sql
DROP DATABASE IF EXISTS mycompany;
```

**执行示例：**
```sql
mysql> DROP DATABASE mycompany;
Query OK, 0 rows affected (0.02 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

使用数据库：
```sql
USE 数据库名;
```

**详细示例：**
```sql
mysql> USE mycompany;
Database changed

mysql> SELECT DATABASE();
+------------+
| DATABASE() |
+------------+
| mycompany  |
+------------+
1 row in set (0.00 sec)
```

##### 注意事项

- UTF8字符集长度为3字节，有些符号占4字节，所以推荐用utf8mb4字符集

#### 表操作

查询当前数据库所有表：
```sql
SHOW TABLES;
```

**示例输出：**
```
+----------------------+
| Tables_in_mycompany  |
+----------------------+
| employees            |
| departments          |
+----------------------+
2 rows in set (0.00 sec)
```

查询表结构：
```sql
DESC 表名;
```

**详细示例：**
```sql
mysql> DESC employees;
+--------------+-------------+------+-----+---------+----------------+
| Field        | Type        | Null | Key | Default | Extra          |
+--------------+-------------+------+-----+---------+----------------+
| id           | int         | NO   | PRI | NULL    | auto_increment |
| name         | varchar(50) | NO   |     | NULL    |                |
| age          | int         | YES  |     | NULL    |                |
| department_id| int         | YES  | MUL | NULL    |                |
| hire_date    | date        | YES  |     | NULL    |                |
+--------------+-------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)
```

查询指定表的建表语句：
```sql
SHOW CREATE TABLE 表名;
```

**详细示例：**
```sql
mysql> SHOW CREATE TABLE employees\G
*************************** 1. row ***************************
       Table: employees
Create Table: CREATE TABLE `employees` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `age` int DEFAULT NULL,
  `department_id` int DEFAULT NULL,
  `hire_date` date DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `idx_department_id` (`department_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci
1 row in set (0.00 sec)
```

创建表：
```sql
CREATE TABLE 表名(
	字段1 字段1类型 [COMMENT 字段1注释],
	字段2 字段2类型 [COMMENT 字段2注释],
	字段3 字段3类型 [COMMENT 字段3注释],
	...
	字段n 字段n类型 [COMMENT 字段n注释]
)[ COMMENT 表注释 ];
```

#### MySQL字段类型详解

##### 1. 数值类型

**整数类型：**

| 类型 | 字节 | 范围（有符号） | 范围（无符号） | 适用场景 |
|------|------|---------------|---------------|----------|
| TINYINT | 1 | -128 到 127 | 0 到 255 | 小数值、状态标识、年龄 |
| SMALLINT | 2 | -32,768 到 32,767 | 0 到 65,535 | 中等数值、分类ID |
| MEDIUMINT | 3 | -8,388,608 到 8,388,607 | 0 到 16,777,215 | 较大数值 |
| INT | 4 | -2,147,483,648 到 2,147,483,647 | 0 到 4,294,967,295 | 大数值、主键、外键 |
| BIGINT | 8 | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807 | 0 到 18,446,744,073,709,551,615 | 超大数值、订单号、ID |

**使用建议：**
- **TINYINT**：适合存储状态值（0/1）、年龄（0-120）、小范围枚举值
- **SMALLINT**：适合存储分类ID、小范围计数器
- **INT**：最常用的整数类型，适合主键、外键、计数器
- **BIGINT**：适合存储订单号、交易ID、大数值计数器

**示例：**
```sql
CREATE TABLE user_status (
    id INT PRIMARY KEY AUTO_INCREMENT,
    age TINYINT UNSIGNED COMMENT '年龄：0-255',
    status TINYINT DEFAULT 1 COMMENT '状态：0-禁用，1-启用',
    category_id SMALLINT COMMENT '分类ID',
    view_count INT DEFAULT 0 COMMENT '浏览次数',
    order_number BIGINT COMMENT '订单号'
);
```

**小数类型：**

| 类型 | 字节 | 范围 | 适用场景 |
|------|------|------|----------|
| FLOAT | 4 | ±1.1754351e-38 到 ±3.4028235e+38 | 单精度浮点数，科学计算 |
| DOUBLE | 8 | ±2.2250738585072014e-308 到 ±1.7976931348623157e+308 | 双精度浮点数，精确计算 |
| DECIMAL(M,D) | M+2 | 精确数值，M为总位数，D为小数位数 | 金额、价格、精确计算 |

**使用建议：**
- **FLOAT**：适合存储不要求精确的小数，如温度、坐标
- **DOUBLE**：适合存储需要较高精度的小数
- **DECIMAL**：**强烈推荐**用于存储金额、价格等需要精确计算的数据

**示例：**
```sql
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    price DECIMAL(10,2) COMMENT '价格：10位数字，2位小数',
    weight DOUBLE COMMENT '重量',
    temperature FLOAT COMMENT '温度',
    discount DECIMAL(5,4) COMMENT '折扣率：5位数字，4位小数'
);
```

##### 2. 字符串类型

**固定长度字符串：**

| 类型 | 字节 | 特点 | 适用场景 |
|------|------|------|----------|
| CHAR(M) | M | 固定长度，不足补空格 | 固定长度数据，如手机号、身份证号 |
| BINARY(M) | M | 二进制固定长度 | 二进制数据 |

**可变长度字符串：**

| 类型 | 字节 | 特点 | 适用场景 |
|------|------|------|----------|
| VARCHAR(M) | L+1 | 可变长度，L为实际长度 | 可变长度文本，如姓名、地址 |
| VARBINARY(M) | L+1 | 二进制可变长度 | 二进制数据 |
| TINYBLOB | L+1 | 最大255字节 | 小型二进制数据 |
| BLOB | L+2 | 最大65,535字节 | 中型二进制数据 |
| MEDIUMBLOB | L+3 | 最大16MB | 大型二进制数据 |
| LONGBLOB | L+4 | 最大4GB | 超大型二进制数据 |
| TINYTEXT | L+1 | 最大255字符 | 小型文本 |
| TEXT | L+2 | 最大65,535字符 | 中型文本 |
| MEDIUMTEXT | L+3 | 最大16MB字符 | 大型文本 |
| LONGTEXT | L+4 | 最大4GB字符 | 超大型文本 |

**使用建议：**
- **CHAR(M)**：适合存储固定长度的数据，如手机号、邮政编码、MD5值
- **VARCHAR(M)**：适合存储可变长度的文本，如姓名、邮箱、地址
- **TEXT系列**：适合存储大段文本，如文章内容、评论
- **BLOB系列**：适合存储二进制数据，如图片、文件（但建议存储文件路径）

**示例：**
```sql
CREATE TABLE user_info (
    id INT PRIMARY KEY AUTO_INCREMENT,
    phone CHAR(11) COMMENT '手机号',
    id_card CHAR(18) COMMENT '身份证号',
    username VARCHAR(50) COMMENT '用户名',
    email VARCHAR(100) COMMENT '邮箱',
    address VARCHAR(200) COMMENT '地址',
    bio TEXT COMMENT '个人简介',
    avatar_path VARCHAR(255) COMMENT '头像路径'
);
```

##### 3. 日期时间类型

| 类型 | 字节 | 范围 | 格式 | 适用场景 |
|------|------|------|------|----------|
| DATE | 3 | 1000-01-01 到 9999-12-31 | YYYY-MM-DD | 日期，如生日 |
| TIME | 3 | -838:59:59 到 838:59:59 | HH:MM:SS | 时间，如营业时间 |
| YEAR | 1 | 1901 到 2155 | YYYY | 年份 |
| DATETIME | 8 | 1000-01-01 00:00:00 到 9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS | 日期时间 |
| TIMESTAMP | 4 | 1970-01-01 00:00:01 到 2038-01-19 03:14:07 | YYYY-MM-DD HH:MM:SS | 时间戳，自动更新 |

**使用建议：**
- **DATE**：适合存储日期，如生日、入职日期
- **TIME**：适合存储时间，如营业时间、工作时间
- **DATETIME**：适合存储日期时间，如创建时间、更新时间
- **TIMESTAMP**：适合存储时间戳，支持自动更新，常用于记录修改时间

**示例：**
```sql
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_date DATE COMMENT '订单日期',
    delivery_time TIME COMMENT '配送时间',
    create_time DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    update_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'
);
```

##### 4. 枚举和集合类型

| 类型 | 特点 | 适用场景 |
|------|------|----------|
| ENUM('value1','value2',...) | 单选，最多65,535个值 | 性别、状态等有限选项 |
| SET('value1','value2',...) | 多选，最多64个值 | 标签、权限等多选项 |

**使用建议：**
- **ENUM**：适合存储单选枚举值，如性别（男/女）、状态（启用/禁用）
- **SET**：适合存储多选值，如用户权限、商品标签

**示例：**
```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    gender ENUM('男', '女', '其他') COMMENT '性别',
    status ENUM('active', 'inactive', 'pending') COMMENT '状态',
    permissions SET('read', 'write', 'delete', 'admin') COMMENT '权限'
);
```

##### 5. JSON类型

| 类型 | 特点 | 适用场景 |
|------|------|----------|
| JSON | 存储JSON格式数据 | 配置信息、动态属性 |

**使用建议：**
- **JSON**：适合存储结构化的动态数据，如商品属性、用户配置

**示例：**
```sql
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) COMMENT '商品名称',
    attributes JSON COMMENT '商品属性',
    config JSON COMMENT '配置信息'
);

-- 插入JSON数据
INSERT INTO products (name, attributes) VALUES 
('手机', '{"color": "黑色", "storage": "128GB", "price": 3999}');

-- 查询JSON数据
SELECT name, JSON_EXTRACT(attributes, '$.color') AS color FROM products;
```

##### 6. 字段类型选择最佳实践

**性能考虑：**
1. **选择最小够用的类型**：如年龄用TINYINT而不是INT
2. **优先使用数值类型**：数值类型比字符串类型性能更好
3. **避免NULL**：如果字段不允许为空，设置为NOT NULL
4. **合理使用索引**：为常用查询字段创建索引

**存储考虑：**
1. **金额使用DECIMAL**：避免使用FLOAT或DOUBLE存储金额
2. **大文件存储路径**：不要直接存储文件内容，存储文件路径
3. **文本长度控制**：合理设置VARCHAR长度，避免浪费空间

**兼容性考虑：**
1. **字符集选择**：推荐使用utf8mb4字符集
2. **时区处理**：DATETIME不受时区影响，TIMESTAMP受时区影响
3. **版本兼容**：JSON类型需要MySQL 5.7.8+

**详细示例：**

1. 创建简单的员工表：
```sql
CREATE TABLE employees (
	id INT PRIMARY KEY AUTO_INCREMENT COMMENT '员工ID',
	name VARCHAR(50) NOT NULL COMMENT '员工姓名',
	age INT COMMENT '员工年龄',
	gender CHAR(1) DEFAULT '男' COMMENT '性别',
	hire_date DATE COMMENT '入职日期'
) COMMENT '员工信息表';
```

2. 创建带有外键约束的部门表和员工表：
```sql
-- 创建部门表
CREATE TABLE departments (
	id INT PRIMARY KEY AUTO_INCREMENT COMMENT '部门ID',
	dept_name VARCHAR(50) NOT NULL COMMENT '部门名称',
	location VARCHAR(100) COMMENT '部门位置'
) COMMENT '部门信息表';

-- 创建员工表（包含外键）
CREATE TABLE employees (
	id INT PRIMARY KEY AUTO_INCREMENT COMMENT '员工ID',
	name VARCHAR(50) NOT NULL COMMENT '员工姓名',
	age INT COMMENT '员工年龄',
	department_id INT COMMENT '部门ID',
	hire_date DATE COMMENT '入职日期',
	CONSTRAINT fk_emp_dept FOREIGN KEY (department_id) REFERENCES departments(id)
) COMMENT '员工信息表';
```

3. 创建带有各种约束的用户表：
```sql
CREATE TABLE users (
	id INT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID',
	username VARCHAR(30) NOT NULL UNIQUE COMMENT '用户名',
	password VARCHAR(100) NOT NULL COMMENT '密码',
	email VARCHAR(100) NOT NULL UNIQUE COMMENT '邮箱',
	age INT CHECK (age >= 18 AND age <= 100) COMMENT '年龄',
	status TINYINT DEFAULT 1 COMMENT '状态：1-正常，0-禁用',
	create_time DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
	update_time DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'
) COMMENT '用户信息表';
```

**执行示例：**
```sql
mysql> CREATE TABLE employees (
    -> id INT PRIMARY KEY AUTO_INCREMENT COMMENT '员工ID',
    -> name VARCHAR(50) NOT NULL COMMENT '员工姓名',
    -> age INT COMMENT '员工年龄',
    -> gender CHAR(1) DEFAULT '男' COMMENT '性别',
    -> hire_date DATE COMMENT '入职日期'
    -> ) COMMENT '员工信息表';
Query OK, 0 rows affected (0.03 sec)

mysql> SHOW TABLES;
+----------------------+
| Tables_in_mycompany  |
+----------------------+
| employees            |
+----------------------+
1 row in set (0.00 sec)
```

**最后一个字段后面没有逗号**

添加字段：
```sql
ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];
```

**详细示例：**

1. 添加单个字段：
```sql
ALTER TABLE employees ADD nickname VARCHAR(20) COMMENT '昵称';
```

2. 添加多个字段：
```sql
ALTER TABLE employees 
ADD phone VARCHAR(20) COMMENT '联系电话',
ADD email VARCHAR(100) COMMENT '邮箱地址';
```

3. 添加带有约束的字段：
```sql
ALTER TABLE employees ADD salary DECIMAL(10,2) DEFAULT 0.00 COMMENT '薪资';
```

4. 在指定位置添加字段：
```sql
-- 在第一个位置添加字段
ALTER TABLE employees ADD employee_no VARCHAR(20) COMMENT '员工编号' FIRST;

-- 在某个字段后面添加字段
ALTER TABLE employees ADD phone VARCHAR(20) COMMENT '联系电话' AFTER name;
```

**执行示例：**
```sql
mysql> ALTER TABLE employees ADD phone VARCHAR(20) COMMENT '联系电话';
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC employees;
+--------------+-------------+------+-----+---------+----------------+
| Field        | Type        | Null | Key | Default | Extra          |
+--------------+-------------+------+-----+---------+----------------+
| id           | int         | NO   | PRI | NULL    | auto_increment |
| name         | varchar(50) | NO   |     | NULL    |                |
| age          | int         | YES  |     | NULL    |                |
| gender       | char(1)     | YES  |     | 男      |                |
| hire_date    | date        | YES  |     | NULL    |                |
| phone        | varchar(20) | YES  |     | NULL    |                |
+--------------+-------------+------+-----+---------+----------------+
6 rows in set (0.00 sec)
```

修改数据类型：
```sql
ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);
```

**详细示例：**
```sql
-- 将phone字段的长度从20修改为30
ALTER TABLE employees MODIFY phone VARCHAR(30);

-- 将age字段修改为不能为NULL
ALTER TABLE employees MODIFY age INT NOT NULL;

-- 修改字段类型并添加默认值
ALTER TABLE employees MODIFY gender CHAR(1) DEFAULT '未知';
```

修改字段名和字段类型：
```sql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];
```

**详细示例：**

1. 修改字段名和类型：
```sql
-- 将nickname字段修改为username，类型为varchar(30)
ALTER TABLE employees CHANGE nickname username VARCHAR(30) COMMENT '用户名';
```

2. 只修改字段名，不修改类型：
```sql
ALTER TABLE employees CHANGE phone mobile_phone VARCHAR(20) COMMENT '手机号码';
```

3. 修改字段名、类型和约束：
```sql
ALTER TABLE employees CHANGE age employee_age TINYINT UNSIGNED COMMENT '员工年龄';
```

**执行示例：**
```sql
mysql> ALTER TABLE employees CHANGE phone mobile_phone VARCHAR(20) COMMENT '手机号码';
Query OK, 0 rows affected (0.06 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC employees;
+--------------+-------------+------+-----+---------+----------------+
| Field        | Type        | Null | Key | Default | Extra          |
+--------------+-------------+------+-----+---------+----------------+
| id           | int         | NO   | PRI | NULL    | auto_increment |
| name         | varchar(50) | NO   |     | NULL    |                |
| age          | int         | YES  |     | NULL    |                |
| gender       | char(1)     | YES  |     | 男      |                |
| hire_date    | date        | YES  |     | NULL    |                |
| mobile_phone | varchar(20) | YES  |     | NULL    |                |
+--------------+-------------+------+-----+---------+----------------+
6 rows in set (0.00 sec)
```

删除字段：
```sql
ALTER TABLE 表名 DROP 字段名;
```

**详细示例：**

1. 删除单个字段：
```sql
ALTER TABLE employees DROP mobile_phone;
```

2. 删除多个字段：
```sql
ALTER TABLE employees 
DROP mobile_phone,
DROP nickname;
```

**执行示例：**
```sql
mysql> ALTER TABLE employees DROP mobile_phone;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC employees;
+--------------+-------------+------+-----+---------+----------------+
| Field        | Type        | Null | Key | Default | Extra          |
+--------------+-------------+------+-----+---------+----------------+
| id           | int         | NO   | PRI | NULL    | auto_increment |
| name         | varchar(50) | NO   |     | NULL    |                |
| age          | int         | YES  |     | NULL    |                |
| gender       | char(1)     | YES  |     | 男      |                |
| hire_date    | date        | YES  |     | NULL    |                |
+--------------+-------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)
```

修改表名：
```sql
ALTER TABLE 表名 RENAME TO 新表名;
```

**详细示例：**
```sql
-- 将employees表重命名为staff
ALTER TABLE employees RENAME TO staff;

-- 或者使用RENAME语法
RENAME TABLE employees TO staff;
```

**执行示例：**
```sql
mysql> ALTER TABLE employees RENAME TO staff;
Query OK, 0 rows affected (0.01 sec)

mysql> SHOW TABLES;
+----------------------+
| Tables_in_mycompany  |
+----------------------+
| staff                |
+----------------------+
1 row in set (0.00 sec)
```

删除表：
```sql
DROP TABLE [IF EXISTS] 表名;
```

**详细示例：**

1. 删除表：
```sql
DROP TABLE staff;
```

2. 使用IF EXISTS避免删除不存在的表时报错：
```sql
DROP TABLE IF EXISTS staff;
```

3. 删除多个表：
```sql
DROP TABLE IF EXISTS staff, departments;
```

**执行示例：**
```sql
mysql> DROP TABLE IF EXISTS staff;
Query OK, 0 rows affected (0.02 sec)

mysql> SHOW TABLES;
Empty set (0.00 sec)
```

删除表，并重新创建该表：
```sql
TRUNCATE TABLE 表名;
```

**详细示例：**
```sql
-- 清空employees表中的所有数据，但保留表结构
TRUNCATE TABLE employees;

-- 与DELETE的区别：
-- DELETE FROM employees;  -- 逐行删除，可以回滚，不会重置自增ID
-- TRUNCATE TABLE employees;  -- 删除整个表并重新创建，不能回滚，会重置自增ID
```

**执行示例：**
```sql
mysql> TRUNCATE TABLE employees;
Query OK, 0 rows affected (0.03 sec)

-- 注意：TRUNCATE操作会重置自增ID
mysql> INSERT INTO employees (name, age) VALUES ('张三', 25);
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM employees;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   25 | 男     | NULL      |
+----+--------+------+--------+-----------+
1 row in set (0.00 sec)
```

### DML（数据操作语言）

#### 添加数据

指定字段：
```sql
INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...);
```

**详细示例：**

1. 插入指定字段的数据：
```sql
INSERT INTO employees (name, age, gender, hire_date) 
VALUES ('张三', 25, '男', '2023-01-15');
```

2. 插入多个字段的数据：
```sql
INSERT INTO employees (name, age, gender, hire_date, department_id) 
VALUES ('李四', 30, '女', '2022-06-20', 1);
```

**执行示例：**
```sql
mysql> INSERT INTO employees (name, age, gender, hire_date) 
    -> VALUES ('张三', 25, '男', '2023-01-15');
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM employees;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   25 | 男     | 2023-01-15|
+----+--------+------+--------+-----------+
1 row in set (0.00 sec)
```

全部字段：
```sql
INSERT INTO 表名 VALUES (值1, 值2, ...);
```

**详细示例：**

1. 插入所有字段的数据（按照表结构顺序）：
```sql
INSERT INTO employees 
VALUES (NULL, '王五', 28, '男', '2023-03-10');
```

2. 注意：NULL值会自动使用自增ID

**执行示例：**
```sql
mysql> INSERT INTO employees 
    -> VALUES (NULL, '王五', 28, '男', '2023-03-10');
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM employees;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   25 | 男     | 2023-01-15|
|  2 | 王五   |   28 | 男     | 2023-03-10|
+----+--------+------+--------+-----------+
2 rows in set (0.00 sec)
```

批量添加数据：
```sql
INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);
INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);
```

**详细示例：**

1. 批量插入指定字段的数据：
```sql
INSERT INTO employees (name, age, gender, hire_date) VALUES 
('赵六', 32, '男', '2022-08-15'),
('孙七', 27, '女', '2023-02-20'),
('周八', 35, '男', '2021-11-30');
```

2. 批量插入所有字段的数据：
```sql
INSERT INTO employees VALUES 
(NULL, '吴九', 29, '女', '2022-12-05'),
(NULL, '郑十', 31, '男', '2022-04-18'),
(NULL, '钱一', 26, '女', '2023-05-22');
```

**执行示例：**
```sql
mysql> INSERT INTO employees (name, age, gender, hire_date) VALUES 
    -> ('赵六', 32, '男', '2022-08-15'),
    -> ('孙七', 27, '女', '2023-02-20'),
    -> ('周八', 35, '男', '2021-11-30');
Query OK, 3 rows affected (0.02 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM employees;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   25 | 男     | 2023-01-15|
|  2 | 王五   |   28 | 男     | 2023-03-10|
|  3 | 赵六   |   32 | 男     | 2022-08-15|
|  4 | 孙七   |   27 | 女     | 2023-02-20|
|  5 | 周八   |   35 | 男     | 2021-11-30|
+----+--------+------+--------+-----------+
5 rows in set (0.00 sec)
```

3. 使用INSERT INTO ... SELECT语句批量插入：
```sql
-- 从一个表复制数据到另一个表
INSERT INTO employees_backup (name, age, gender, hire_date)
SELECT name, age, gender, hire_date FROM employees WHERE age > 30;
```

##### 注意事项

- 字符串和日期类型数据应该包含在引号中
- 插入的数据大小应该在字段的规定范围内
- 批量插入比单条插入效率更高
- 插入数据时要保证数据类型和字段类型匹配

#### 更新和删除数据

修改数据：
```sql
UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2, ... [ WHERE 条件 ];
```

**详细示例：**

1. 修改单个字段：
```sql
UPDATE employees SET age = 26 WHERE id = 1;
```

2. 修改多个字段：
```sql
UPDATE employees SET age = 31, gender = '女' WHERE name = '王五';
```

3. 批量修改：
```sql
UPDATE employees SET age = age + 1 WHERE gender = '男';
```

4. 使用条件表达式：
```sql
UPDATE employees SET 
    age = CASE 
        WHEN age < 25 THEN age + 2
        WHEN age BETWEEN 25 AND 30 THEN age + 1
        ELSE age
    END;
```

**执行示例：**
```sql
mysql> UPDATE employees SET age = 26 WHERE id = 1;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SELECT * FROM employees WHERE id = 1;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   26 | 男     | 2023-01-15|
+----+--------+------+--------+-----------+
1 row in set (0.00 sec)
```

**重要提示：**
- 务必使用WHERE子句，否则会修改表中所有记录
- 建议在执行UPDATE前先用SELECT查询确认要修改的记录

删除数据：
```sql
DELETE FROM 表名 [ WHERE 条件 ];
```

**详细示例：**

1. 删除指定记录：
```sql
DELETE FROM employees WHERE id = 5;
```

2. 根据条件删除：
```sql
DELETE FROM employees WHERE age < 25;
```

3. 批量删除：
```sql
DELETE FROM employees WHERE gender = '女' AND age > 30;
```

4. 删除所有记录（谨慎使用）：
```sql
DELETE FROM employees;
```

**执行示例：**
```sql
mysql> DELETE FROM employees WHERE id = 5;
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM employees;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   26 | 男     | 2023-01-15|
|  2 | 王五   |   28 | 男     | 2023-03-10|
|  3 | 赵六   |   32 | 男     | 2022-08-15|
|  4 | 孙七   |   27 | 女     | 2023-02-20|
+----+--------+------+--------+-----------+
4 rows in set (0.00 sec)
```

**重要提示：**
- 务必使用WHERE子句，否则会删除表中所有记录
- DELETE操作可以回滚（在事务中）
- DELETE不会重置自增ID

**DELETE vs TRUNCATE 对比：**

| 特性 | DELETE | TRUNCATE |
|------|--------|----------|
| 删除方式 | 逐行删除 | 删除整个表并重新创建 |
| WHERE子句 | 支持 | 不支持 |
| 事务支持 | 可以回滚 | 不能回滚 |
| 自增ID | 不重置 | 重置 |
| 触发器 | 激活 | 不激活 |
| 性能 | 较慢 | 较快 |

#### 数据备份和恢复

**备份数据库：**

```bash
mysqldump -u root -p -B 数据库名1 数据库名2 > d:\work\bak.sql
```

**详细示例：**

1. 备份单个数据库：
```bash
mysqldump -u root -p -B mycompany > d:\work\mycompany_backup.sql
```

2. 备份多个数据库：
```bash
mysqldump -u root -p -B mycompany testdb > d:\work\multi_db_backup.sql
```

3. 备份所有数据库：
```bash
mysqldump -u root -p -A > d:\work\all_databases_backup.sql
```

**备份表：**

```bash
mysqldump -u root -p 数据库名 表1 表2 > d:\work\bak.sql
```

**详细示例：**

1. 备份单个表：
```bash
mysqldump -u root -p mycompany employees > d:\work\employees_backup.sql
```

2. 备份多个表：
```bash
mysqldump -u root -p mycompany employees departments > d:\work\tables_backup.sql
```

3. 只备份数据结构（不包含数据）：
```bash
mysqldump -u root -p -d mycompany employees > d:\work\employees_structure.sql
```

4. 只备份数据（不包含结构）：
```bash
mysqldump -u root -p -t mycompany employees > d:\work\employees_data.sql
```

**恢复数据库：**

必须先进入mysql命令行才可以：

```bash
mysql -u root -p
source d:\work\bak.sql
```

**详细示例：**

1. 在MySQL命令行中恢复：
```sql
mysql -u root -p
Enter password: ****

mysql> source d:\work\mycompany_backup.sql
```

2. 直接从命令行恢复：
```bash
mysql -u root -p mycompany < d:\work\mycompany_backup.sql
```

3. 恢复特定表：
```bash
mysql -u root -p mycompany < d:\work\employees_backup.sql
```

**备份恢复最佳实践：**

1. 定期备份：建议每天或每周进行备份
2. 备份验证：定期测试备份文件的完整性
3. 异地存储：将备份文件存储在不同位置
4. 命名规范：使用日期时间命名备份文件
5. 压缩备份：对大型备份文件进行压缩

```bash
# 示例：创建带时间戳的压缩备份
mysqldump -u root -p -B mycompany | gzip > d:\work\mycompany_$(date +%Y%m%d_%H%M%S).sql.gz
```

### DQL（数据查询语言）

语法：
```sql
SELECT
	字段列表
FROM
	表名
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后的条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

#### 基础查询

查询多个字段：
```sql
SELECT 字段1, 字段2, 字段3, ... FROM 表名;
SELECT * FROM 表名;
```

**详细示例：**

1. 查询指定字段：
```sql
SELECT id, name, age FROM employees;
```

2. 查询所有字段：
```sql
SELECT * FROM employees;
```

3. 查询并计算字段：
```sql
SELECT name, age, age + 5 AS future_age FROM employees;
```

**执行示例：**
```sql
mysql> SELECT id, name, age FROM employees;
+----+--------+------+
| id | name   | age  |
+----+--------+------+
|  1 | 张三   |   26 |
|  2 | 王五   |   28 |
|  3 | 赵六   |   32 |
|  4 | 孙七   |   27 |
+----+--------+------+
4 rows in set (0.00 sec)
```

设置别名：
```sql
SELECT 字段1 [ AS 别名1 ], 字段2 [ AS 别名2 ], 字段3 [ AS 别名3 ], ... FROM 表名;
SELECT 字段1 [ 别名1 ], 字段2 [ 别名2 ], 字段3 [ 别名3 ], ... FROM 表名;
```

**详细示例：**

1. 使用AS关键字设置别名：
```sql
SELECT name AS employee_name, age AS employee_age FROM employees;
```

2. 省略AS关键字设置别名：
```sql
SELECT name employee_name, age employee_age FROM employees;
```

3. 为表设置别名：
```sql
SELECT e.name, e.age FROM employees e;
```

4. 为计算字段设置别名：
```sql
SELECT name, age, salary, salary * 12 AS annual_salary FROM employees;
```

**执行示例：**
```sql
mysql> SELECT name AS employee_name, age AS employee_age FROM employees;
+--------------+--------------+
| employee_name| employee_age |
+--------------+--------------+
| 张三         |           26 |
| 王五         |           28 |
| 赵六         |           32 |
| 孙七         |           27 |
+--------------+--------------+
4 rows in set (0.00 sec)
```

去除重复记录：
```sql
SELECT DISTINCT 字段列表 FROM 表名;
```

**详细示例：**

1. 去除单个字段的重复值：
```sql
SELECT DISTINCT gender FROM employees;
```

2. 去除多个字段的重复组合：
```sql
SELECT DISTINCT gender, age FROM employees;
```

3. 统计不重复值的数量：
```sql
SELECT COUNT(DISTINCT gender) FROM employees;
```

**执行示例：**
```sql
mysql> SELECT DISTINCT gender FROM employees;
+--------+
| gender |
+--------+
| 男     |
| 女     |
+--------+
2 rows in set (0.00 sec)
```

转义：
```sql
SELECT * FROM 表名 WHERE name LIKE '/_张三' ESCAPE '/'
```

**详细示例：**

1. 转义特殊字符：
```sql
-- 查找姓名中包含下划线的员工
SELECT * FROM employees WHERE name LIKE '%/_%' ESCAPE '/';

-- 查找姓名以"张_"开头的员工
SELECT * FROM employees WHERE name LIKE '张/_%' ESCAPE '/';
```

#### 条件查询

语法：
```sql
SELECT 字段列表 FROM 表名 WHERE 条件列表;
```

条件：

| 比较运算符          | 功能                                        |
| ------------------- | ------------------------------------------- |
| >                   | 大于                                        |
| >=                  | 大于等于                                    |
| <                   | 小于                                        |
| <=                  | 小于等于                                    |
| =                   | 等于                                        |
| <> 或 !=            | 不等于                                      |
| BETWEEN ... AND ... | 在某个范围内（含最小、最大值）              |
| IN(...)             | 在in之后的列表中的值，多选一                |
| LIKE 占位符         | 模糊匹配（\_匹配单个字符，%匹配任意个字符） |
| IS NULL             | 是NULL                                      |

| 逻辑运算符         | 功能                         |
| ------------------ | ---------------------------- |
| AND 或 &&          | 并且（多个条件同时成立）     |
| OR 或 &#124;&#124; | 或者（多个条件任意一个成立） |
| NOT 或 !           | 非，不是                     |

**详细示例：**

1. 基本比较运算：
```sql
-- 年龄等于30
SELECT * FROM employees WHERE age = 30;

-- 年龄小于30
SELECT * FROM employees WHERE age < 30;

-- 年龄小于等于30
SELECT * FROM employees WHERE age <= 30;

-- 年龄大于25
SELECT * FROM employees WHERE age > 25;

-- 年龄大于等于25
SELECT * FROM employees WHERE age >= 25;
```

2. 不等于查询：
```sql
-- 年龄不等于30
SELECT * FROM employees WHERE age != 30;
SELECT * FROM employees WHERE age <> 30;
```

3. 范围查询：
```sql
-- 年龄在20到30之间（包含边界值）
SELECT * FROM employees WHERE age BETWEEN 20 AND 30;

-- 等价于
SELECT * FROM employees WHERE age >= 20 AND age <= 30;

-- 注意：下面的语句不会报错，但查不到任何信息
SELECT * FROM employees WHERE age BETWEEN 30 AND 20;
```

4. IN查询：
```sql
-- 年龄等于25或30或35
SELECT * FROM employees WHERE age = 25 OR age = 30 OR age = 35;
-- 使用IN更简洁
SELECT * FROM employees WHERE age IN (25, 30, 35);

-- 部门在技术部、市场部、人事部
SELECT * FROM employees WHERE department_id IN (1, 2, 3);
```

5. NULL值查询：
```sql
-- 没有身份证号的员工
SELECT * FROM employees WHERE idcard IS NULL OR idcard = '';

-- 有身份证号的员工
SELECT * FROM employees WHERE idcard IS NOT NULL;

-- 简写方式（仅适用于非NULL且非空字符串）
SELECT * FROM employees WHERE idcard;
```

6. LIKE模糊查询：
```sql
-- 姓名为两个字（两个下划线）
SELECT * FROM employees WHERE name LIKE '__';

-- 姓名以"张"开头
SELECT * FROM employees WHERE name LIKE '张%';

-- 姓名包含"三"
SELECT * FROM employees WHERE name LIKE '%三%';

-- 身份证最后为X
SELECT * FROM employees WHERE idcard LIKE '%X';

-- 手机号以138开头
SELECT * FROM employees WHERE phone LIKE '138%';
```

7. 逻辑运算符：
```sql
-- 性别为女且年龄小于30
SELECT * FROM employees WHERE age < 30 AND gender = '女';

-- 年龄小于25或年龄大于35
SELECT * FROM employees WHERE age < 25 OR age > 35;

-- 性别不是男的员工
SELECT * FROM employees WHERE gender != '男';
SELECT * FROM employees WHERE NOT gender = '男';

-- 复杂条件组合
SELECT * FROM employees 
WHERE (gender = '女' AND age < 30) 
   OR (gender = '男' AND age > 35);
```

**执行示例：**
```sql
mysql> SELECT * FROM employees WHERE age BETWEEN 25 AND 30;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   26 | 男     | 2023-01-15|
|  2 | 王五   |   28 | 男     | 2023-03-10|
|  4 | 孙七   |   27 | 女     | 2023-02-20|
+----+--------+------+--------+-----------+
3 rows in set (0.00 sec)

mysql> SELECT * FROM employees WHERE name LIKE '张%';
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   26 | 男     | 2023-01-15|
+----+--------+------+--------+-----------+
1 row in set (0.00 sec)
```

#### 聚合查询（聚合函数）

常见聚合函数：

| 函数  | 功能     |
| ----- | -------- |
| count | 统计数量 |
| max   | 最大值   |
| min   | 最小值   |
| avg   | 平均值   |
| sum   | 求和     |

语法：
```sql
SELECT 聚合函数(字段列表) FROM 表名;
```

**详细示例：**

1. COUNT函数：
```sql
-- 统计员工总数
SELECT COUNT(*) FROM employees;
SELECT COUNT(id) FROM employees;

-- 统计有身份证号的员工数
SELECT COUNT(idcard) FROM employees;

-- 统计不重复的年龄数
SELECT COUNT(DISTINCT age) FROM employees;

-- 统计特定条件的员工数
SELECT COUNT(*) FROM employees WHERE age > 30;
```

2. MAX和MIN函数：
```sql
-- 查询最大年龄
SELECT MAX(age) FROM employees;

-- 查询最小年龄
SELECT MIN(age) FROM employees;

-- 查询最高薪资
SELECT MAX(salary) FROM employees;

-- 查询最早入职日期
SELECT MIN(hire_date) FROM employees;
```

3. AVG函数：
```sql
-- 计算平均年龄
SELECT AVG(age) FROM employees;

-- 计算男性员工平均年龄
SELECT AVG(age) FROM employees WHERE gender = '男';

-- 计算平均薪资（保留两位小数）
SELECT ROUND(AVG(salary), 2) FROM employees;
```

4. SUM函数：
```sql
-- 计算年龄总和
SELECT SUM(age) FROM employees;

-- 计算薪资总和
SELECT SUM(salary) FROM employees;

-- 计算特定部门薪资总和
SELECT SUM(salary) FROM employees WHERE department_id = 1;
```

5. 组合使用聚合函数：
```sql
-- 一次性获取多个统计信息
SELECT 
    COUNT(*) AS total_count,
    MAX(age) AS max_age,
    MIN(age) AS min_age,
    AVG(age) AS avg_age,
    SUM(age) AS total_age
FROM employees;
```

**执行示例：**
```sql
mysql> SELECT 
    -> COUNT(*) AS total_count,
    -> MAX(age) AS max_age,
    -> MIN(age) AS min_age,
    -> AVG(age) AS avg_age
    -> FROM employees;
+-------------+---------+---------+---------+
| total_count | max_age | min_age | avg_age |
+-------------+---------+---------+---------+
|           4 |      32 |      26 |  28.2500|
+-------------+---------+---------+---------+
1 row in set (0.00 sec)
```

#### 分组查询

语法：
```sql
SELECT 字段列表 FROM 表名 [ WHERE 条件 ] GROUP BY 分组字段名 [ HAVING 分组后的过滤条件 ];
```

where 和 having 的区别：

- 执行时机不同：where是分组之前进行过滤，不满足where条件不参与分组；having是分组后对结果进行过滤。
- 判断条件不同：where不能对聚合函数进行判断，而having可以。

**详细示例：**

1. 基本分组：
```sql
-- 根据性别分组，统计男性和女性数量（只显示分组数量）
SELECT COUNT(*) FROM employees GROUP BY gender;

-- 根据性别分组，统计男性和女性数量（显示性别）
SELECT gender, COUNT(*) FROM employees GROUP BY gender;
```

2. 多字段分组：
```sql
-- 按性别和年龄分组
SELECT gender, age, COUNT(*) FROM employees GROUP BY gender, age;

-- 按部门和性别分组
SELECT department_id, gender, COUNT(*) FROM employees GROUP BY department_id, gender;
```

3. 分组后使用聚合函数：
```sql
-- 根据性别分组，统计男性和女性的平均年龄
SELECT gender, AVG(age) AS avg_age FROM employees GROUP BY gender;

-- 根据部门分组，统计每个部门的员工数和平均薪资
SELECT department_id, 
       COUNT(*) AS emp_count,
       AVG(salary) AS avg_salary
FROM employees 
GROUP BY department_id;
```

4. WHERE和HAVING结合使用：
```sql
-- 年龄小于45，并根据工作地址分组
SELECT workaddress, COUNT(*) 
FROM employees 
WHERE age < 45 
GROUP BY workaddress;

-- 年龄小于45，并根据工作地址分组，获取员工数量大于等于3的工作地址
SELECT workaddress, COUNT(*) AS address_count 
FROM employees 
WHERE age < 45 
GROUP BY workaddress 
HAVING address_count >= 3;
```

5. 复杂分组查询：
```sql
-- 按部门分组，统计每个部门的员工信息
SELECT 
    department_id,
    COUNT(*) AS employee_count,
    MAX(age) AS max_age,
    MIN(age) AS min_age,
    AVG(age) AS avg_age,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department_id
ORDER BY employee_count DESC;
```

**执行示例：**
```sql
mysql> SELECT gender, COUNT(*) AS count, AVG(age) AS avg_age 
    -> FROM employees 
    -> GROUP BY gender;
+--------+-------+---------+
| gender | count | avg_age |
+--------+-------+---------+
| 男     |     3 | 28.6667 |
| 女     |     1 | 27.0000 |
+--------+-------+---------+
2 rows in set (0.00 sec)
```

##### 注意事项

- 执行顺序：where > 聚合函数 > having
- 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义
- GROUP BY 后的字段必须出现在 SELECT 中，除非使用聚合函数

#### 排序查询

语法：
```sql
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1, 字段2 排序方式2;
```

排序方式：

- ASC: 升序（默认）
- DESC: 降序

**详细示例：**

1. 单字段排序：
```sql
-- 根据年龄升序排序
SELECT * FROM employees ORDER BY age ASC;
SELECT * FROM employees ORDER BY age;  -- ASC可以省略

-- 根据年龄降序排序
SELECT * FROM employees ORDER BY age DESC;

-- 根据入职日期升序排序
SELECT * FROM employees ORDER BY hire_date;
```

2. 多字段排序：
```sql
-- 两字段排序，根据年龄升序排序，入职时间降序排序
SELECT * FROM employees ORDER BY age ASC, hire_date DESC;

-- 先按部门排序，同部门内按年龄排序
SELECT * FROM employees ORDER BY department_id ASC, age DESC;
```

3. 按别名排序：
```sql
-- 按计算字段排序
SELECT name, age, salary, salary * 12 AS annual_salary 
FROM employees 
ORDER BY annual_salary DESC;
```

4. 按表达式排序：
```sql
-- 按姓名长度排序
SELECT name, LENGTH(name) AS name_length 
FROM employees 
ORDER BY LENGTH(name) DESC;
```

5. 空值处理：
```sql
-- 空值排在最后（默认）
SELECT * FROM employees ORDER BY commission_pct DESC;

-- 空值排在最前
SELECT * FROM employees ORDER BY commission_pct IS NULL, commission_pct DESC;
```

**执行示例：**
```sql
mysql> SELECT * FROM employees ORDER BY age ASC, hire_date DESC;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   26 | 男     | 2023-01-15|
|  4 | 孙七   |   27 | 女     | 2023-02-20|
|  2 | 王五   |   28 | 男     | 2023-03-10|
|  3 | 赵六   |   32 | 男     | 2022-08-15|
+----+--------+------+--------+-----------+
4 rows in set (0.00 sec)
```

##### 注意事项

如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序

例子：

```mysql
-- 根据年龄升序排序
SELECT * FROM employee ORDER BY age ASC;
SELECT * FROM employee ORDER BY age;
-- 两字段排序，根据年龄升序排序，入职时间降序排序
SELECT * FROM employee ORDER BY age ASC, entrydate DESC;
```

##### 注意事项

如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序

#### 分页查询

语法：
```sql
SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;
```

**详细示例：**

1. 基本分页查询：
```sql
-- 查询第一页数据，展示10条
SELECT * FROM employees LIMIT 0, 10;

-- 查询第二页数据
SELECT * FROM employees LIMIT 10, 10;

-- 查询第三页数据
SELECT * FROM employees LIMIT 20, 10;
```

2. 简化写法（第一页）：
```sql
-- 如果查询的是第一页数据，起始索引可以省略
SELECT * FROM employees LIMIT 10;
```

3. 带条件的分页查询：
```sql
-- 查询年龄大于25的员工，分页显示
SELECT * FROM employees WHERE age > 25 LIMIT 0, 5;

-- 查询女性员工，按年龄降序分页
SELECT * FROM employees WHERE gender = '女' ORDER BY age DESC LIMIT 0, 3;
```

4. 动态分页公式：
```sql
-- 假设每页显示pageSize条记录，查询第pageNum页的数据
-- 起始索引 = (pageNum - 1) * pageSize

-- 示例：每页5条，查询第3页
-- 起始索引 = (3 - 1) * 5 = 10
SELECT * FROM employees LIMIT 10, 5;
```

5. 分页与排序结合：
```sql
-- 按薪资降序分页查询
SELECT * FROM employees ORDER BY salary DESC LIMIT 0, 10;

-- 按入职日期升序，年龄降序分页查询
SELECT * FROM employees ORDER BY hire_date ASC, age DESC LIMIT 5, 5;
```

6. 获取总记录数和总页数：
```sql
-- 获取总记录数
SELECT COUNT(*) AS total FROM employees;

-- 计算总页数（每页10条）
-- 总页数 = CEILING(总记录数 / 每页记录数)
SELECT CEILING(COUNT(*) / 10.0) AS total_pages FROM employees;
```

**执行示例：**
```sql
mysql> SELECT * FROM employees LIMIT 0, 2;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  1 | 张三   |   26 | 男     | 2023-01-15|
|  2 | 王五   |   28 | 男     | 2023-03-10|
+----+--------+------+--------+-----------+
2 rows in set (0.00 sec)

mysql> SELECT * FROM employees LIMIT 2, 2;
+----+--------+------+--------+-----------+
| id | name   | age  | gender | hire_date |
+----+--------+------+--------+-----------+
|  3 | 赵六   |   32 | 男     | 2022-08-15|
|  4 | 孙七   |   27 | 女     | 2023-02-20|
+----+--------+------+--------+-----------+
2 rows in set (0.00 sec)
```

##### 注意事项

- 起始索引从0开始，起始索引 = （查询页码 - 1） * 每页显示记录数
- 分页查询是数据库的方言，不同数据库有不同实现，MySQL是LIMIT
- 如果查询的是第一页数据，起始索引可以省略，直接简写 LIMIT 10
- 分页查询在大数据量时可能会有性能问题，需要优化索引

#### DQL执行顺序

FROM -> WHERE -> GROUP BY -> SELECT -> ORDER BY -> LIMIT

**详细说明：**

1. **FROM**: 指定要查询的表
2. **WHERE**: 过滤原始数据
3. **GROUP BY**: 对数据进行分组
4. **SELECT**: 选择要显示的字段
5. **ORDER BY**: 对结果进行排序
6. **LIMIT**: 限制返回的记录数

**示例：**
```sql
SELECT gender, COUNT(*) AS count
FROM employees
WHERE age > 25
GROUP BY gender
ORDER BY count DESC
LIMIT 1;
```

执行顺序：
1. FROM employees - 先确定要查询的表
2. WHERE age > 25 - 过滤出年龄大于25的员工
3. GROUP BY gender - 按性别分组
4. SELECT gender, COUNT(*) AS count - 选择性别和计数
5. ORDER BY count DESC - 按计数降序排序
6. LIMIT 1 - 只返回第一条记录

**注意事项：**
- 在WHERE子句中不能使用别名
- 在ORDER BY中可以使用SELECT中定义的别名
- 聚合函数只能在SELECT、HAVING、ORDER BY中使用

### DCL

#### 管理用户

查询用户：

```sql
USE mysql;
SELECT * FROM user;
```

**详细示例：**

1. 查询所有用户：
```sql
USE mysql;
SELECT host, user, authentication_string FROM user;
```

2. 查询特定用户：
```sql
SELECT host, user FROM user WHERE user = 'root';
```

3. 查看当前登录用户：
```sql
SELECT USER();
SELECT CURRENT_USER();
```

**执行示例：**
```sql
mysql> USE mysql;
Database changed

mysql> SELECT host, user FROM user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
4 rows in set (0.00 sec)
```

创建用户:
```sql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

**详细示例：**

1. 创建本地访问用户：
```sql
-- 创建用户test，只能在当前主机localhost访问
CREATE USER 'test'@'localhost' IDENTIFIED BY 'Test123456!';
```

2. 创建远程访问用户：
```sql
-- 创建用户test，能在任意主机访问
CREATE USER 'test'@'%' IDENTIFIED BY 'Test123456!';
```

3. 创建指定IP访问用户：
```sql
-- 创建用户，只能从特定IP访问
CREATE USER 'test'@'192.168.1.100' IDENTIFIED BY 'Test123456!';

-- 创建用户，可以从特定网段访问
CREATE USER 'test'@'192.168.1.%' IDENTIFIED BY 'Test123456!';
```

4. 使用MySQL 8推荐的身份验证方式：
```sql
-- 使用caching_sha2_password验证方式（MySQL 8默认）
CREATE USER 'test'@'localhost' IDENTIFIED BY 'Test123456!';

-- 使用mysql_native_password验证方式（兼容性更好）
CREATE USER 'test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Test123456!';
```

**执行示例：**
```sql
mysql> CREATE USER 'test'@'localhost' IDENTIFIED BY 'Test123456!';
Query OK, 0 rows affected (0.02 sec)

mysql> SELECT host, user FROM user WHERE user = 'test';
+-----------+------+
| host      | user |
+-----------+------+
| localhost | test |
+-----------+------+
1 row in set (0.00 sec)
```

修改用户密码：
```sql
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
```

**详细示例：**

1. 修改当前用户密码：
```sql
ALTER USER USER() IDENTIFIED BY 'NewPassword123!';
```

2. 修改指定用户密码：
```sql
-- 修改test用户的密码
ALTER USER 'test'@'localhost' IDENTIFIED BY 'NewPassword123!';

-- 使用mysql_native_password验证方式修改密码
ALTER USER 'test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'NewPassword123!';
```

3. 修改密码并设置过期策略：
```sql
-- 密码永不过期
ALTER USER 'test'@'localhost' IDENTIFIED BY 'NewPassword123!' PASSWORD EXPIRE NEVER;

-- 密码90天后过期
ALTER USER 'test'@'localhost' IDENTIFIED BY 'NewPassword123!' PASSWORD EXPIRE INTERVAL 90 DAY;

-- 首次登录后必须修改密码
ALTER USER 'test'@'localhost' IDENTIFIED BY 'NewPassword123!' PASSWORD EXPIRE;
```

**执行示例：**
```sql
mysql> ALTER USER 'test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'NewPassword123!';
Query OK, 0 rows affected (0.01 sec)
```

删除用户：
```sql
DROP USER '用户名'@'主机名';
```

**详细示例：**

1. 删除单个用户：
```sql
DROP USER 'test'@'localhost';
```

2. 删除多个用户：
```sql
DROP USER 'test'@'localhost', 'admin'@'%';
```

3. 使用IF EXISTS避免删除不存在的用户时报错：
```sql
DROP USER IF EXISTS 'test'@'localhost';
```

**执行示例：**
```sql
mysql> DROP USER 'test'@'localhost';
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT host, user FROM user WHERE user = 'test';
Empty set (0.00 sec)
```

##### 注意事项

- 主机名可以使用 % 通配符，代表任意主机
- 用户名和主机名的组合必须唯一
- 删除用户前，建议先撤销该用户的所有权限
- MySQL 8默认使用caching_sha2_password身份验证插件

#### 权限控制

常用权限：

| 权限                | 说明               |
| ------------------- | ------------------ |
| ALL, ALL PRIVILEGES | 所有权限           |
| SELECT              | 查询数据           |
| INSERT              | 插入数据           |
| UPDATE              | 修改数据           |
| DELETE              | 删除数据           |
| ALTER               | 修改表             |
| DROP                | 删除数据库/表/视图 |
| CREATE              | 创建数据库/表      |
| REFERENCES          | 外键约束           |
| INDEX               | 创建索引           |
| CREATE TEMPORARY TABLES | 创建临时表     |
| LOCK TABLES         | 锁定表             |
| EXECUTE             | 执行存储过程       |
| REPLICATION CLIENT  | 查询复制状态       |
| REPLICATION SLAVE   | 复制从服务器       |
| SHOW DATABASES      | 查看数据库列表     |
| SUPER               | 超级用户权限       |

更多权限请看[权限一览表](#权限一览表 "权限一览表")

查询权限：
```sql
SHOW GRANTS FOR '用户名'@'主机名';
```

**详细示例：**

1. 查看当前用户权限：
```sql
SHOW GRANTS;
SHOW GRANTS FOR CURRENT_USER();
```

2. 查看指定用户权限：
```sql
SHOW GRANTS FOR 'test'@'localhost';
```

**执行示例：**
```sql
mysql> SHOW GRANTS FOR 'test'@'localhost';
+-------------------------------------------+
| Grants for test@localhost                 |
+-------------------------------------------+
| GRANT USAGE ON *.* TO `test`@`localhost`  |
+-------------------------------------------+
1 row in set (0.00 sec)
```

授予权限：
```sql
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
```

**详细示例：**

1. 授予所有权限（谨慎使用）：
```sql
-- 授予所有数据库的所有权限
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;

-- 授予指定数据库的所有权限
GRANT ALL PRIVILEGES ON mycompany.* TO 'developer'@'localhost';
```

2. 授予特定权限：
```sql
-- 授予查询权限
GRANT SELECT ON mycompany.employees TO 'readonly'@'localhost';

-- 授予多个权限
GRANT SELECT, INSERT, UPDATE ON mycompany.employees TO 'operator'@'localhost';

-- 授予创建和修改权限
GRANT CREATE, ALTER ON mycompany.* TO 'developer'@'localhost';
```

3. 授予特定级别的权限：
```sql
-- 全局级别（所有数据库的所有表）
GRANT SELECT ON *.* TO 'readonly'@'localhost';

-- 数据库级别（特定数据库的所有表）
GRANT ALL PRIVILEGES ON mycompany.* TO 'developer'@'localhost';

-- 表级别（特定表的特定权限）
GRANT SELECT, INSERT, UPDATE ON mycompany.employees TO 'operator'@'localhost';

-- 列级别（特定表的特定列）
GRANT SELECT(name, age) ON mycompany.employees TO 'limited'@'localhost';
```

4. 带GRANT OPTION的授权：
```sql
-- 允许用户将自己的权限授予其他用户
GRANT ALL PRIVILEGES ON mycompany.* TO 'manager'@'localhost' WITH GRANT OPTION;
```

5. 授权后刷新权限：
```sql
FLUSH PRIVILEGES;
```

**执行示例：**
```sql
mysql> GRANT SELECT, INSERT, UPDATE ON mycompany.employees TO 'operator'@'localhost';
Query OK, 0 rows affected (0.01 sec)

mysql> SHOW GRANTS FOR 'operator'@'localhost';
+--------------------------------------------------------+
| Grants for operator@localhost                          |
+--------------------------------------------------------+
| GRANT USAGE ON *.* TO `operator`@`localhost`           |
| GRANT SELECT, INSERT, UPDATE ON `mycompany`.`employees` |
+--------------------------------------------------------+
2 rows in set (0.00 sec)
```

撤销权限：
```sql
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

**详细示例：**

1. 撤销特定权限：
```sql
-- 撤销插入权限
REVOKE INSERT ON mycompany.employees FROM 'operator'@'localhost';

-- 撤销多个权限
REVOKE INSERT, UPDATE ON mycompany.employees FROM 'operator'@'localhost';
```

2. 撤销所有权限：
```sql
-- 撤销特定数据库的所有权限
REVOKE ALL PRIVILEGES ON mycompany.* FROM 'developer'@'localhost';

-- 撤销所有权限
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'admin'@'localhost';
```

3. 撤销GRANT OPTION：
```sql
-- 撤销授权权限
REVOKE GRANT OPTION ON mycompany.* FROM 'manager'@'localhost';
```

**执行示例：**
```sql
mysql> REVOKE INSERT, UPDATE ON mycompany.employees FROM 'operator'@'localhost';
Query OK, 0 rows affected (0.01 sec)

mysql> SHOW GRANTS FOR 'operator'@'localhost';
+------------------------------------------------+
| Grants for operator@localhost                  |
+------------------------------------------------+
| GRANT USAGE ON *.* TO `operator`@`localhost`   |
| GRANT SELECT ON `mycompany`.`employees`        |
+------------------------------------------------+
2 rows in set (0.00 sec)
```

##### 注意事项

- 多个权限用逗号分隔
- 授权时，数据库名和表名可以用 * 进行通配，代表所有
- REVOKE和GRANT的语法类似，但关键字不同
- 撤销权限不会删除用户，只是移除权限
- 建议遵循最小权限原则，只授予必要的权限

**权限管理最佳实践：**

1. **最小权限原则**：只授予用户完成工作所需的最小权限
2. **定期审查权限**：定期检查和清理不必要的权限
3. **使用角色管理**：MySQL 8.0支持角色，可以通过角色管理权限
4. **密码安全**：强制使用强密码，定期更换密码
5. **审计日志**：启用审计日志，监控用户操作

**MySQL 8 角色管理示例：**

```sql
-- 创建角色
CREATE ROLE 'app_developer', 'app_readonly', 'app_admin';

-- 为角色授权
GRANT ALL PRIVILEGES ON mycompany.* TO 'app_admin';
GRANT SELECT, INSERT, UPDATE, DELETE ON mycompany.* TO 'app_developer';
GRANT SELECT ON mycompany.* TO 'app_readonly';

-- 将角色授予用户
GRANT 'app_developer' TO 'developer1'@'localhost';
GRANT 'app_readonly' TO 'readonly_user'@'%';

-- 激活角色
SET DEFAULT ROLE ALL TO 'developer1'@'localhost';

-- 查看角色
SHOW GRANTS FOR 'app_developer';
```

## 函数

- 字符串函数
- 数值函数
- 日期函数
- 流程函数

### 字符串函数

常用函数：

| 函数  | 功能  |
| ------------ | ------------ |
| CONCAT(s1, s2, ..., sn)  | 字符串拼接，将s1, s2, ..., sn拼接成一个字符串  |
| LOWER(str)  | 将字符串全部转为小写  |
| UPPER(str)  | 将字符串全部转为大写  |
| LPAD(str, n, pad)  | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度  |
| RPAD(str, n, pad)  | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度  |
| TRIM(str)  | 去掉字符串头部和尾部的空格  |
| SUBSTRING(str, start, len)  | 返回从字符串str从start位置起的len个长度的字符串  |
| REPLACE(column, source, replace)  | 替换字符串  |
| LENGTH(str)  | 返回字符串的长度（字节数）  |
| CHAR_LENGTH(str)  | 返回字符串的长度（字符数）  |
| REVERSE(str)  | 反转字符串  |
| LEFT(str, len)  | 从字符串左边开始返回len个字符  |
| RIGHT(str, len)  | 从字符串右边开始返回len个字符  |
| LOCATE(substr, str)  | 返回子字符串在字符串中第一次出现的位置  |
| INSTR(str, substr)  | 返回子字符串在字符串中第一次出现的位置  |

**详细示例：**

1. **CONCAT - 字符串拼接**
```sql
-- 基本拼接
SELECT CONCAT('Hello', 'World');  -- HelloWorld

-- 拼接多个字符串
SELECT CONCAT('Hello', ' ', 'World', '!');  -- Hello World!

-- 拼接字段
SELECT CONCAT(name, '(', age, '岁)') AS employee_info 
FROM employees;

-- 处理NULL值（如果任何参数为NULL，结果为NULL）
SELECT CONCAT('Hello', NULL, 'World');  -- NULL

-- 使用CONCAT_WS处理NULL值
SELECT CONCAT_WS(',', 'Hello', NULL, 'World');  -- Hello,World
```

2. **LOWER/UPPER - 大小写转换**
```sql
-- 转为小写
SELECT LOWER('Hello World');  -- hello world

-- 转为大写
SELECT UPPER('Hello World');  -- HELLO WORLD

-- 实际应用：统一格式后比较
SELECT * FROM employees 
WHERE LOWER(name) = 'zhang san';

-- 首字母大写
SELECT CONCAT(UPPER(LEFT(name, 1)), LOWER(SUBSTRING(name, 2))) AS formatted_name
FROM employees;
```

3. **LPAD/RPAD - 填充字符串**
```sql
-- 左填充
SELECT LPAD('01', 5, '-');  -- ---01
SELECT LPAD('123', 6, '0');  -- 000123

-- 右填充
SELECT RPAD('01', 5, '-');  -- 01---
SELECT RPAD('Hello', 10, '*');  -- Hello*****

-- 实际应用：格式化编号
SELECT LPAD(id, 5, '0') AS formatted_id FROM employees;

-- 对齐文本输出
SELECT RPAD(name, 20, ' ') AS name_column, age FROM employees;
```

4. **TRIM - 去除空格**
```sql
-- 去除两端空格
SELECT TRIM('  Hello World  ');  -- Hello World

-- 去除特定字符
SELECT TRIM('x' FROM 'xxxHello Worldxxx');  -- Hello World

-- 只去除左边空格
SELECT LTRIM('  Hello World  ');  -- Hello World  

-- 只去除右边空格
SELECT RTRIM('  Hello World  ');  --   Hello World

-- 实际应用：清理用户输入
SELECT TRIM(name) FROM employees WHERE name = ' 张三 ';
```

5. **SUBSTRING - 字符串截取**
```sql
-- 基本截取（起始索引为1）
SELECT SUBSTRING('Hello World', 1, 5);  -- Hello

-- 从指定位置开始到结尾
SELECT SUBSTRING('Hello World', 7);  -- World

-- 使用负数（从末尾开始）
SELECT SUBSTRING('Hello World', -6);  -- World

-- 实际应用：提取手机号后四位
SELECT name, 
       phone, 
       SUBSTRING(phone, -4) AS phone_last4
FROM employees;

-- 提取文件扩展名
SELECT SUBSTRING('document.pdf', LOCATE('.', 'document.pdf') + 1) AS extension;
```

6. **REPLACE - 字符串替换**
```sql
-- 基本替换
SELECT REPLACE('Hello World', 'World', 'MySQL');  -- Hello MySQL

-- 替换字段中的内容
SELECT REPLACE(phone, '138', '139') AS new_phone FROM employees;

-- 删除特定字符
SELECT REPLACE('Hello-World', '-', '');  -- HelloWorld

-- 实际应用：脱敏处理
SELECT name, 
       REPLACE(SUBSTRING(phone, 1, 7), SUBSTRING(phone, 1, 7), '****') AS masked_phone
FROM employees;
```

7. **LENGTH/CHAR_LENGTH - 字符串长度**
```sql
-- 字节长度（UTF8编码中，中文字符占3字节）
SELECT LENGTH('Hello');  -- 5
SELECT LENGTH('你好');   -- 6

-- 字符长度
SELECT CHAR_LENGTH('Hello');  -- 5
SELECT CHAR_LENGTH('你好');   -- 2

-- 实际应用：验证输入长度
SELECT name FROM employees 
WHERE CHAR_LENGTH(name) > 10;

-- 计算字段存储空间
SELECT name, 
       LENGTH(name) AS bytes, 
       CHAR_LENGTH(name) AS chars
FROM employees;
```

8. **其他实用字符串函数**
```sql
-- 反转字符串
SELECT REVERSE('Hello');  -- olleH

-- 从左边取字符
SELECT LEFT('Hello World', 5);  -- Hello

-- 从右边取字符
SELECT RIGHT('Hello World', 5);  -- World

-- 查找子字符串位置
SELECT LOCATE('World', 'Hello World');  -- 7
SELECT INSTR('Hello World', 'World');   -- 7

-- 实际应用：判断是否包含特定内容
SELECT name, email 
FROM employees 
WHERE LOCATE('@company.com', email) > 0;

-- 提取域名
SELECT SUBSTRING(email, LOCATE('@', email) + 1) AS domain
FROM employees;
```

**执行示例：**
```sql
mysql> SELECT CONCAT('Hello', ' ', 'World', '!');
+-----------------------------------+
| CONCAT('Hello', ' ', 'World', '!')|
+-----------------------------------+
| Hello World!                      |
+-----------------------------------+
1 row in set (0.00 sec)

mysql> SELECT CONCAT(name, '(', age, '岁)') AS employee_info FROM employees LIMIT 2;
+------------------+
| employee_info    |
+------------------+
| 张三(26岁)       |
| 王五(28岁)       |
+------------------+
2 rows in set (0.00 sec)
```

### 数值函数

常见函数：

| 函数  | 功能  |
| ------------ | ------------ |
| CEIL(x)  | 向上取整  |
| FLOOR(x)  | 向下取整  |
| MOD(x, y)  | 返回x/y的模  |
| RAND() | 返回0~1内的随机数 |
| ROUND(x, y) | 求参数x的四舍五入值，保留y位小数 |
| ABS(x) | 返回x的绝对值 |
| POW(x, y) | 返回x的y次方 |
| SQRT(x) | 返回x的平方根 |
| SIGN(x) | 返回参数的符号（正数返回1，负数返回-1，0返回0） |
| GREATEST(x1, x2, ...) | 返回参数中的最大值 |
| LEAST(x1, x2, ...) | 返回参数中的最小值 |
| FORMAT(x, d) | 将数字格式化为千分位字符串，保留d位小数 |

**详细示例：**

1. **CEIL/FLOOR - 取整函数**
```sql
-- 向上取整
SELECT CEIL(3.14);   -- 4
SELECT CEIL(-3.14);  -- -3
SELECT CEIL(3.0);    -- 3

-- 向下取整
SELECT FLOOR(3.14);  -- 3
SELECT FLOOR(-3.14); -- -4
SELECT FLOOR(3.9);   -- 3

-- 实际应用：计算页数
SELECT CEIL(total_records / 10.0) AS total_pages 
FROM (SELECT COUNT(*) AS total_records FROM employees) AS t;
```

2. **ROUND - 四舍五入**
```sql
-- 基本四舍五入
SELECT ROUND(3.14);     -- 3
SELECT ROUND(3.54);     -- 4
SELECT ROUND(-3.54);    -- -4

-- 保留指定小数位
SELECT ROUND(3.14159, 2);  -- 3.14
SELECT ROUND(3.14559, 2);  -- 3.15
SELECT ROUND(123.456, -1); -- 120

-- 实际应用：薪资计算
SELECT name, 
       salary, 
       ROUND(salary * 0.08, 2) AS tax,
       ROUND(salary * 1.12, 2) AS annual_salary
FROM employees;
```

3. **MOD - 取模运算**
```sql
-- 基本取模
SELECT MOD(10, 3);   -- 1
SELECT MOD(10, 5);   -- 0
SELECT 10 % 3;       -- 1（使用运算符）

-- 实际应用：判断奇偶数
SELECT name, age,
       CASE WHEN MOD(age, 2) = 0 THEN '偶数' ELSE '奇数' END AS age_type
FROM employees;

-- 分组计算（每3个一组）
SELECT name, 
       CEIL(id / 3.0) AS group_number
FROM employees;
```

4. **RAND - 随机数**
```sql
-- 生成0-1之间的随机数
SELECT RAND();  -- 0.123456789...

-- 生成指定范围的随机数
SELECT FLOOR(RAND() * 100);  -- 0-99之间的随机整数
SELECT FLOOR(RAND() * 50) + 1;  -- 1-50之间的随机整数

-- 实际应用：随机抽样
SELECT * FROM employees 
ORDER BY RAND() 
LIMIT 3;

-- 随机排序
SELECT name FROM employees 
ORDER BY RAND();
```

5. **ABS - 绝对值**
```sql
-- 基本用法
SELECT ABS(10);    -- 10
SELECT ABS(-10);   -- 10
SELECT ABS(-3.14); -- 3.14

-- 实际应用：计算差值
SELECT name, age,
       ABS(age - 30) AS age_difference
FROM employees;
```

6. **POW/SQRT - 幂运算和平方根**
```sql
-- 幂运算
SELECT POW(2, 3);   -- 8
SELECT POW(10, 2);  -- 100
SELECT 2 ** 3;      -- 8（使用运算符）

-- 平方根
SELECT SQRT(16);    -- 4
SELECT SQRT(2);     -- 1.4142135623730951

-- 实际应用：计算面积
SELECT radius,
       POW(radius, 2) * 3.14159 AS area
FROM circles;
```

7. **FORMAT - 数字格式化**
```sql
-- 基本格式化
SELECT FORMAT(1234567.8912, 2);  -- 1,234,567.89
SELECT FORMAT(1234567.8912, 0);  -- 1,234,568

-- 实际应用：金额显示
SELECT name, 
       salary,
       FORMAT(salary, 2) AS formatted_salary,
       FORMAT(salary * 12, 2) AS annual_salary
FROM employees;
```

**执行示例：**
```sql
mysql> SELECT ROUND(123.456, 2);
+-------------------+
| ROUND(123.456, 2) |
+-------------------+
|            123.46 |
+-------------------+
1 row in set (0.00 sec)

mysql> SELECT FORMAT(1234567.8912, 2);
+-------------------------+
| FORMAT(1234567.8912, 2) |
+-------------------------+
| 1,234,567.89             |
+-------------------------+
1 row in set (0.00 sec)
```

### 日期函数

常用函数：

| 函数  | 功能  |
| ------------ | ------------ |
| CURDATE()  | 返回当前日期  |
| CURTIME()  | 返回当前时间  |
| NOW()  | 返回当前日期和时间  |
| YEAR(date)  | 获取指定date的年份  |
| MONTH(date)  | 获取指定date的月份  |
| DAY(date)  | 获取指定date的日期  |
| DATE_ADD(date, INTERVAL expr type)  | 返回一个日期/时间值加上一个时间间隔expr后的时间值  |
| DATEDIFF(date1, date2)  | 返回起始时间date1和结束时间date2之间的天数  |
| DATE_FORMAT(date, format)  | 按照指定格式格式化日期  |
| DATE(date)  | 提取日期部分  |
| TIME(date)  | 提取时间部分  |
| TIMESTAMPDIFF(unit, date1, date2)  | 返回两个日期之间的时间差，单位由unit指定  |
| LAST_DAY(date)  | 返回指定日期所在月份的最后一天  |
| DAYNAME(date)  | 返回指定日期是星期几的名称  |
| DAYOFWEEK(date)  | 返回指定日期是星期几（1=星期日，7=星期六）  |
| DAYOFYEAR(date)  | 返回指定日期是一年中的第几天  |

**详细示例：**

1. **CURDATE/CURTIME/NOW - 当前日期时间**
```sql
-- 当前日期
SELECT CURDATE();  -- 2023-06-25
SELECT CURRENT_DATE();  -- 2023-06-25
SELECT CURRENT_DATE;  -- 2023-06-25

-- 当前时间
SELECT CURTIME();  -- 14:30:45
SELECT CURRENT_TIME();  -- 14:30:45

-- 当前日期和时间
SELECT NOW();  -- 2023-06-25 14:30:45
SELECT CURRENT_TIMESTAMP();  -- 2023-06-25 14:30:45
SELECT SYSDATE();  -- 2023-06-25 14:30:45
```

2. **YEAR/MONTH/DAY - 提取日期部分**
```sql
-- 基本提取
SELECT YEAR('2023-06-25');  -- 2023
SELECT MONTH('2023-06-25');  -- 6
SELECT DAY('2023-06-25');   -- 25

-- 实际应用：按年月分组统计
SELECT YEAR(hire_date) AS hire_year,
       MONTH(hire_date) AS hire_month,
       COUNT(*) AS hire_count
FROM employees
GROUP BY YEAR(hire_date), MONTH(hire_date)
ORDER BY hire_year, hire_month;

-- 查询特定年份的员工
SELECT * FROM employees 
WHERE YEAR(hire_date) = 2023;

-- 查询特定月份的员工
SELECT * FROM employees 
WHERE MONTH(hire_date) = 6;
```

3. **DATE_ADD/DATE_SUB - 日期加减**
```sql
-- 基本加法
SELECT DATE_ADD(NOW(), INTERVAL 1 DAY);    -- 明天
SELECT DATE_ADD(NOW(), INTERVAL 1 WEEK);   -- 下周
SELECT DATE_ADD(NOW(), INTERVAL 1 MONTH);  -- 下个月
SELECT DATE_ADD(NOW(), INTERVAL 1 YEAR);   -- 明年

-- 基本减法
SELECT DATE_SUB(NOW(), INTERVAL 1 DAY);    -- 昨天
SELECT DATE_SUB(NOW(), INTERVAL 1 WEEK);   -- 上周

-- 复杂计算
SELECT DATE_ADD(NOW(), INTERVAL '3-2' YEAR_MONTH);  -- 3年2个月后
SELECT DATE_ADD(NOW(), INTERVAL '1:30' HOUR_MINUTE); -- 1小时30分钟后

-- 实际应用：计算试用期结束日期
SELECT name, 
       hire_date,
       DATE_ADD(hire_date, INTERVAL 3 MONTH) AS probation_end_date
FROM employees;

-- 查询合同即将到期的员工（30天内）
SELECT name, 
       contract_end_date
FROM employees
WHERE contract_end_date BETWEEN NOW() AND DATE_ADD(NOW(), INTERVAL 30 DAY);
```

4. **DATEDIFF/TIMESTAMPDIFF - 日期差值**
```sql
-- DATEDIFF返回天数差
SELECT DATEDIFF('2023-06-25', '2023-06-20');  -- 5
SELECT DATEDIFF('2023-06-20', '2023-06-25');  -- -5

-- TIMESTAMPDIFF返回指定单位的差值
SELECT TIMESTAMPDIFF(DAY, '2023-06-20', '2023-06-25');  -- 5
SELECT TIMESTAMPDIFF(HOUR, '2023-06-20 10:00:00', '2023-06-25 14:00:00');  -- 124
SELECT TIMESTAMPDIFF(MONTH, '2023-01-01', '2023-06-01');  -- 5

-- 实际应用：计算工龄
SELECT name,
       hire_date,
       TIMESTAMPDIFF(YEAR, hire_date, CURDATE()) AS work_years,
       TIMESTAMPDIFF(MONTH, hire_date, CURDATE()) AS work_months
FROM employees
ORDER BY work_years DESC;

-- 计算年龄
SELECT name,
       birth_date,
       TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) AS age
FROM employees;
```

5. **DATE_FORMAT - 日期格式化**
```sql
-- 常用格式
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d');           -- 2023-06-25
SELECT DATE_FORMAT(NOW(), '%Y年%m月%d日');       -- 2023年06月25日
SELECT DATE_FORMAT(NOW(), '%H:%i:%s');           -- 14:30:45
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s');  -- 2023-06-25 14:30:45

-- 格式化符号说明
-- %Y: 四位年份  %y: 两位年份
-- %m: 两位月份  %c: 一位月份
-- %d: 两位日期  %e: 一位日期
-- %H: 24小时制  %h: 12小时制
-- %i: 分钟      %s: 秒
-- %W: 星期名称  %w: 星期数字(0=周日)

-- 实际应用：格式化显示
SELECT name,
       DATE_FORMAT(hire_date, '%Y年%m月%d日') AS formatted_hire_date,
       DATE_FORMAT(hire_date, '%Y-%m') AS hire_month
FROM employees;

-- 按格式化日期分组
SELECT DATE_FORMAT(hire_date, '%Y-%m') AS hire_month,
       COUNT(*) AS hire_count
FROM employees
GROUP BY DATE_FORMAT(hire_date, '%Y-%m')
ORDER BY hire_month;
```

6. **其他实用日期函数**
```sql
-- 获取月份的最后一天
SELECT LAST_DAY('2023-02-15');  -- 2023-02-28
SELECT LAST_DAY(NOW());          -- 当前月的最后一天

-- 获取星期几
SELECT DAYNAME('2023-06-25');   -- Sunday
SELECT DAYOFWEEK('2023-06-25'); -- 1 (星期日)
SELECT DAYOFWEEK('2023-06-26'); -- 2 (星期一)

-- 获取一年中的第几天
SELECT DAYOFYEAR('2023-06-25');  -- 176

-- 实际应用：查询周末入职的员工
SELECT name, hire_date, DAYNAME(hire_date) AS weekday
FROM employees
WHERE DAYOFWEEK(hire_date) IN (1, 7);

-- 计算季度
SELECT name,
       hire_date,
       QUARTER(hire_date) AS quarter,
       CONCAT(YEAR(hire_date), 'Q', QUARTER(hire_date)) AS year_quarter
FROM employees;
```

**执行示例：**
```sql
mysql> SELECT NOW(), DATE_FORMAT(NOW(), '%Y年%m月%d日 %H时%i分%s秒');
+---------------------+--------------------------------------------------+
| NOW()               | DATE_FORMAT(NOW(), '%Y年%m月%d日 %H时%i分%s秒') |
+---------------------+--------------------------------------------------+
| 2023-06-25 14:30:45 | 2023年06月25日 14时30分45秒                      |
+---------------------+--------------------------------------------------+
1 row in set (0.00 sec)

mysql> SELECT DATEDIFF('2023-06-25', '2023-06-20');
+--------------------------------------+
| DATEDIFF('2023-06-25', '2023-06-20') |
+--------------------------------------+
|                                    5 |
+--------------------------------------+
1 row in set (0.00 sec)
```

### 流程函数

常用函数：

| 函数  | 功能  |
| ------------ | ------------ |
| IF(value, t, f)  | 如果value为true，则返回t，否则返回f  |
| IFNULL(value1, value2)  | 如果value1不为空，返回value1，否则返回value2  |
| CASE WHEN [ val1 ] THEN [ res1 ] ... ELSE [ default ] END  | 如果val1为true，返回res1，... 否则返回default默认值  |
| CASE [ expr ] WHEN [ val1 ] THEN [ res1 ] ... ELSE [ default ] END  | 如果expr的值等于val1，返回res1，... 否则返回default默认值  |
| NULLIF(expr1, expr2)  | 如果expr1等于expr2，返回NULL，否则返回expr1  |
| COALESCE(value1, value2, ...)  | 返回参数中第一个非NULL的值  |

**详细示例：**

1. **IF - 条件判断**
```sql
-- 基本用法
SELECT IF(1 > 0, '真', '假');  -- 真
SELECT IF(1 < 0, '真', '假');  -- 假

-- 实际应用：年龄分类
SELECT name,
       age,
       IF(age >= 60, '老年', 
          IF(age >= 40, '中年', 
             IF(age >= 20, '青年', '少年'))) AS age_group
FROM employees;

-- 判断是否有奖金
SELECT name,
       salary,
       bonus,
       IF(bonus IS NULL, '无奖金', CONCAT('奖金：', bonus)) AS bonus_info
FROM employees;

-- 条件计算
SELECT name,
       salary,
       IF(salary > 10000, salary * 1.1, salary * 1.05) AS new_salary
FROM employees;
```

2. **IFNULL - NULL值处理**
```sql
-- 基本用法
SELECT IFNULL(NULL, '默认值');  -- 默认值
SELECT IFNULL('Hello', '默认值');  -- Hello

-- 实际应用：处理NULL值
SELECT name,
       IFNULL(commission_pct, 0) AS commission_rate
FROM employees;

-- 拼接时处理NULL
SELECT name,
       CONCAT(IFNULL(first_name, ''), ' ', IFNULL(last_name, '')) AS full_name
FROM employees;

-- 计算时处理NULL
SELECT name,
       salary,
       IFNULL(bonus, 0) AS bonus,
       salary + IFNULL(bonus, 0) AS total_income
FROM employees;
```

3. **CASE WHEN - 多条件判断**
```sql
-- 基本语法1：搜索CASE
SELECT name,
       age,
       CASE 
           WHEN age >= 60 THEN '老年'
           WHEN age >= 40 THEN '中年'
           WHEN age >= 20 THEN '青年'
           ELSE '少年'
       END AS age_group
FROM employees;

-- 基本语法2：简单CASE
SELECT name,
       gender,
       CASE gender
           WHEN '男' THEN 'Male'
           WHEN '女' THEN 'Female'
           ELSE 'Other'
       END AS gender_en
FROM employees;

-- 实际应用：薪资等级
SELECT name,
       salary,
       CASE 
           WHEN salary >= 20000 THEN '高级'
           WHEN salary >= 15000 THEN '中级'
           WHEN salary >= 10000 THEN '初级'
           ELSE '实习'
       END AS salary_level
FROM employees;

-- 复杂条件判断
SELECT name,
       department_id,
       salary,
       CASE 
           WHEN department_id = 1 AND salary > 15000 THEN '技术骨干'
           WHEN department_id = 2 AND salary > 12000 THEN '销售精英'
           WHEN salary > 10000 THEN '优秀员工'
           ELSE '普通员工'
       END AS employee_level
FROM employees;
```

4. **NULLIF - 比较返回NULL**
```sql
-- 基本用法
SELECT NULLIF(10, 10);  -- NULL
SELECT NULLIF(10, 5);   -- 10

-- 实际应用：避免除零错误
SELECT name,
       sales_amount,
       target_amount,
       sales_amount / NULLIF(target_amount, 0) AS completion_rate
FROM sales_records;

-- 数据清洗
SELECT name,
       old_value,
       new_value,
       NULLIF(old_value, new_value) AS changed_value
FROM data_records;
```

5. **COALESCE - 返回第一个非NULL值**
```sql
-- 基本用法
SELECT COALESCE(NULL, NULL, '第三', '第四');  -- 第三
SELECT COALESCE('第一', '第二', '第三');  -- 第一

-- 实际应用：多字段备选
SELECT name,
       COALESCE(work_phone, mobile_phone, home_phone) AS contact_phone
FROM employees;

-- 设置默认值
SELECT name,
       COALESCE(nickname, name, '用户') AS display_name
FROM users;

-- 复杂默认值逻辑
SELECT product_name,
       COALESCE(special_price, discount_price, regular_price) AS current_price
FROM products;
```

**执行示例：**
```sql
mysql> SELECT name, age,
    -> CASE 
    ->     WHEN age >= 60 THEN '老年'
    ->     WHEN age >= 40 THEN '中年'
    ->     WHEN age >= 20 THEN '青年'
    ->     ELSE '少年'
    -> END AS age_group
    -> FROM employees
    -> LIMIT 3;
+--------+------+----------+
| name   | age  | age_group|
+--------+------+----------+
| 张三   |   26 | 青年     |
| 王五   |   28 | 青年     |
| 赵六   |   32 | 青年     |
+--------+------+----------+
3 rows in set (0.00 sec)

mysql> SELECT IFNULL(NULL, '默认值'), IFNULL('Hello', '默认值');
+-------------------------+----------------------------+
| IFNULL(NULL, '默认值')  | IFNULL('Hello', '默认值')  |
+-------------------------+----------------------------+
| 默认值                  | Hello                      |
+-------------------------+----------------------------+
1 row in set (0.00 sec)
```

## 约束

分类：

| 约束  | 描述  | 关键字  |
| ------------ | ------------ | ------------ |
| 非空约束  | 限制该字段的数据不能为null  | NOT NULL  |
| 唯一约束  | 保证该字段的所有数据都是唯一、不重复的  | UNIQUE  |
| 主键约束  | 主键是一行数据的唯一标识，要求非空且唯一  | PRIMARY KEY  |
| 默认约束  | 保存数据时，如果未指定该字段的值，则采用默认值  | DEFAULT  |
| 检查约束（8.0.1版本后）  | 保证字段值满足某一个条件  | CHECK  |
| 外键约束  | 用来让两张表的数据之间建立连接，保证数据的一致性和完整性  | FOREIGN KEY  |

约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束。

### 常用约束

| 约束条件  | 关键字  |
| ------------ | ------------ |
| 主键  | PRIMARY KEY  |
| 自动增长  | AUTO_INCREMENT  |
| 不为空  | NOT NULL  |
| 唯一  | UNIQUE  |
| 逻辑条件  | CHECK  |
| 默认值  | DEFAULT  |

**详细示例：**

1. **创建包含各种约束的表**
```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID',
    username VARCHAR(30) NOT NULL UNIQUE COMMENT '用户名',
    password VARCHAR(100) NOT NULL COMMENT '密码',
    email VARCHAR(100) UNIQUE COMMENT '邮箱',
    age INT CHECK (age >= 18 AND age <= 100) COMMENT '年龄',
    status TINYINT DEFAULT 1 COMMENT '状态：1-正常，0-禁用',
    create_time DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    update_time DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'
) COMMENT '用户信息表';
```

2. **主键约束**
```sql
-- 单字段主键
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL
);

-- 多字段联合主键
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);

-- 添加主键约束
ALTER TABLE employees ADD PRIMARY KEY (id);

-- 删除主键约束
ALTER TABLE employees DROP PRIMARY KEY;
```

3. **非空约束**
```sql
-- 创建表时指定非空约束
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT
);

-- 修改表添加非空约束
ALTER TABLE employees MODIFY name VARCHAR(50) NOT NULL;

-- 删除非空约束
ALTER TABLE employees MODIFY name VARCHAR(50) NULL;
```

4. **唯一约束**
```sql
-- 创建表时指定唯一约束
CREATE TABLE employees (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(20)
);

-- 添加唯一约束
ALTER TABLE employees ADD CONSTRAINT uk_email UNIQUE (email);

-- 删除唯一约束
ALTER TABLE employees DROP INDEX uk_email;

-- 组合唯一约束
ALTER TABLE employees ADD CONSTRAINT uk_name_phone UNIQUE (name, phone);
```

5. **默认约束**
```sql
-- 创建表时指定默认值
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    status VARCHAR(20) DEFAULT 'active',
    create_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 修改表添加默认值
ALTER TABLE employees MODIFY status VARCHAR(20) DEFAULT 'active';

-- 删除默认值
ALTER TABLE employees MODIFY status VARCHAR(20) DEFAULT NULL;
```

6. **检查约束（MySQL 8.0.1+）**
```sql
-- 创建表时指定检查约束
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT CHECK (age >= 18 AND age <= 65),
    salary DECIMAL(10,2) CHECK (salary >= 0),
    email VARCHAR(100) CHECK (email LIKE '%@%')
);

-- 添加检查约束
ALTER TABLE employees 
ADD CONSTRAINT chk_age CHECK (age >= 18 AND age <= 65);

-- 删除检查约束
ALTER TABLE employees DROP CONSTRAINT chk_age;

-- 复杂检查约束
ALTER TABLE employees 
ADD CONSTRAINT chk_salary_age 
CHECK (CASE WHEN age < 25 THEN salary < 5000 ELSE salary > 3000 END);
```

7. **自动增长约束**
```sql
-- 创建表时指定自动增长
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL
);

-- 修改表添加自动增长
ALTER TABLE employees MODIFY id INT AUTO_INCREMENT;

-- 删除自动增长
ALTER TABLE employees MODIFY id INT;

-- 设置自动增长起始值
ALTER TABLE employees AUTO_INCREMENT = 1000;
```

**执行示例：**
```sql
mysql> CREATE TABLE users (
    -> id INT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID',
    -> username VARCHAR(30) NOT NULL UNIQUE COMMENT '用户名',
    -> password VARCHAR(100) NOT NULL COMMENT '密码',
    -> age INT CHECK (age >= 18 AND age <= 100) COMMENT '年龄',
    -> status TINYINT DEFAULT 1 COMMENT '状态：1-正常，0-禁用'
    -> ) COMMENT '用户信息表';
Query OK, 0 rows affected (0.03 sec)

mysql> DESC users;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | int          | NO   | PRI | NULL    | auto_increment |
| username | varchar(30)  | NO   | UNI | NULL    |                |
| password | varchar(100) | NO   |     | NULL    |                |
| age      | int          | YES  |     | NULL    |                |
| status   | tinyint      | YES  |     | 1       |                |
+----------+--------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)
```

### 外键约束

添加外键：

```sql
CREATE TABLE 表名(
	字段名 字段类型,
	...
	[CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
);

ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);
```

**详细示例：**

1. **创建表时添加外键约束**
```sql
-- 创建主表（部门表）
CREATE TABLE departments (
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT '部门ID',
    dept_name VARCHAR(50) NOT NULL COMMENT '部门名称',
    location VARCHAR(100) COMMENT '部门位置'
) COMMENT '部门信息表';

-- 创建从表（员工表）并添加外键约束
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT '员工ID',
    name VARCHAR(50) NOT NULL COMMENT '员工姓名',
    age INT COMMENT '员工年龄',
    department_id INT COMMENT '部门ID',
    hire_date DATE COMMENT '入职日期',
    CONSTRAINT fk_emp_dept FOREIGN KEY (department_id) REFERENCES departments(id)
) COMMENT '员工信息表';
```

2. **修改表添加外键约束**
```sql
-- 方式1：简单语法
ALTER TABLE employees 
ADD FOREIGN KEY (department_id) REFERENCES departments(id);

-- 方式2：指定外键名称
ALTER TABLE employees 
ADD CONSTRAINT fk_emp_dept FOREIGN KEY (department_id) REFERENCES departments(id);

-- 方式3：指定删除和更新行为
ALTER TABLE employees 
ADD CONSTRAINT fk_emp_dept 
FOREIGN KEY (department_id) REFERENCES departments(id)
ON UPDATE CASCADE
ON DELETE RESTRICT;
```

3. **删除外键约束**
```sql
-- 删除外键约束
ALTER TABLE employees DROP FOREIGN KEY fk_emp_dept;

-- 注意：如果有外键索引，也需要删除
ALTER TABLE employees DROP INDEX fk_emp_dept;
```

4. **外键的删除/更新行为**

| 行为  | 说明  |
| ------------ | ------------ |
| NO ACTION  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与RESTRICT一致）  |
| RESTRICT  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与NO ACTION一致）  |
| CASCADE  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录  |
| SET NULL  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（要求该外键允许为null）  |
| SET DEFAULT  | 父表有变更时，子表将外键设为一个默认值（Innodb不支持）  |

**详细示例：**

```sql
-- CASCADE：级联操作
ALTER TABLE employees 
ADD CONSTRAINT fk_emp_dept 
FOREIGN KEY (department_id) REFERENCES departments(id)
ON UPDATE CASCADE ON DELETE CASCADE;

-- 测试：当更新部门ID时，员工表的部门ID也会自动更新
UPDATE departments SET id = 10 WHERE id = 1;

-- 测试：当删除部门时，该部门的所有员工也会被删除
DELETE FROM departments WHERE id = 10;

-- SET NULL：设置为NULL
ALTER TABLE employees 
ADD CONSTRAINT fk_emp_dept 
FOREIGN KEY (department_id) REFERENCES departments(id)
ON UPDATE SET NULL ON DELETE SET NULL;

-- 注意：使用SET NULL时，外键字段必须允许NULL
ALTER TABLE employees MODIFY department_id INT NULL;

-- RESTRICT：限制操作（默认行为）
ALTER TABLE employees 
ADD CONSTRAINT fk_emp_dept 
FOREIGN KEY (department_id) REFERENCES departments(id)
ON UPDATE RESTRICT ON DELETE RESTRICT;

-- 测试：如果有员工属于该部门，则不允许删除
DELETE FROM departments WHERE id = 1;
-- 错误：Cannot delete or update a parent row: a foreign key constraint fails
```

5. **外键约束的实际应用场景**

```sql
-- 场景1：订单和订单明细
CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2) DEFAULT 0.00
) COMMENT '订单表';

CREATE TABLE order_items (
    item_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL DEFAULT 1,
    unit_price DECIMAL(10,2) NOT NULL,
    CONSTRAINT fk_order_items_order 
    FOREIGN KEY (order_id) REFERENCES orders(order_id)
    ON DELETE CASCADE
) COMMENT '订单明细表';

-- 场景2：学生和课程（多对多关系）
CREATE TABLE students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    grade VARCHAR(20)
) COMMENT '学生表';

CREATE TABLE courses (
    course_id INT PRIMARY KEY AUTO_INCREMENT,
    course_name VARCHAR(100) NOT NULL,
    credit INT NOT NULL
) COMMENT '课程表';

CREATE TABLE student_courses (
    id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    enrollment_date DATE,
    CONSTRAINT fk_sc_student 
    FOREIGN KEY (student_id) REFERENCES students(student_id)
    ON DELETE CASCADE,
    CONSTRAINT fk_sc_course 
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
    ON DELETE CASCADE
) COMMENT '学生选课表';
```

6. **查看外键约束**
```sql
-- 查看表的外键约束
SELECT 
    CONSTRAINT_NAME,
    TABLE_NAME,
    COLUMN_NAME,
    REFERENCED_TABLE_NAME,
    REFERENCED_COLUMN_NAME
FROM 
    INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE 
    TABLE_SCHEMA = 'mycompany' 
    AND TABLE_NAME = 'employees'
    AND REFERENCED_TABLE_NAME IS NOT NULL;

-- 使用SHOW CREATE TABLE查看
SHOW CREATE TABLE employees;
```

**执行示例：**
```sql
mysql> CREATE TABLE departments (
    -> id INT PRIMARY KEY AUTO_INCREMENT COMMENT '部门ID',
    -> dept_name VARCHAR(50) NOT NULL COMMENT '部门名称',
    -> location VARCHAR(100) COMMENT '部门位置'
    -> ) COMMENT '部门信息表';
Query OK, 0 rows affected (0.02 sec)

mysql> CREATE TABLE employees (
    -> id INT PRIMARY KEY AUTO_INCREMENT COMMENT '员工ID',
    -> name VARCHAR(50) NOT NULL COMMENT '员工姓名',
    -> department_id INT COMMENT '部门ID',
    -> CONSTRAINT fk_emp_dept FOREIGN KEY (department_id) REFERENCES departments(id)
    -> ) COMMENT '员工信息表';
Query OK, 0 rows affected (0.03 sec)

mysql> INSERT INTO departments (dept_name, location) VALUES ('技术部', '北京');
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO employees (name, department_id) VALUES ('张三', 1);
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO employees (name, department_id) VALUES ('李四', 99);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails 
(`mycompany`.`employees`, CONSTRAINT `fk_emp_dept` FOREIGN KEY (`department_id`) 
REFERENCES `departments` (`id`))
```

**外键约束的注意事项：**

1. **性能考虑**：外键约束会影响插入、更新、删除操作的性能
2. **设计原则**：合理使用外键，避免过度使用导致性能问题
3. **数据一致性**：外键约束有助于维护数据的一致性和完整性
4. **级联操作**：谨慎使用CASCADE，避免意外删除大量数据
5. **索引要求**：外键字段必须建立索引，MySQL会自动创建
6. **存储引擎**：只有InnoDB存储引擎支持外键约束
alter table emp add constraint fk_emp_dept_id foreign key(dept_id) references dept(id);
```

删除外键：
`ALTER TABLE 表名 DROP FOREIGN KEY 外键名;`

#### 删除/更新行为

| 行为  | 说明  |
| ------------ | ------------ |
| NO ACTION  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与RESTRICT一致）  |
| RESTRICT  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与NO ACTION一致）  |
| CASCADE  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录  |
| SET NULL  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（要求该外键允许为null）  |
| SET DEFAULT  | 父表有变更时，子表将外键设为一个默认值（Innodb不支持）  |

更改删除/更新行为：
`ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) ON UPDATE 行为 ON DELETE 行为;`

## 多表查询

### 多表关系

- 一对多（多对一）
- 多对多
- 一对一

#### 一对多

案例：部门与员工
关系：一个部门对应多个员工，一个员工对应一个部门
实现：在多的一方建立外键，指向一的一方的主键

**详细示例：**

```sql
-- 创建部门表（一的一方）
CREATE TABLE departments (
    id INT PRIMARY KEY AUTO_INCREMENT,
    dept_name VARCHAR(50) NOT NULL,
    location VARCHAR(100)
);

-- 创建员工表（多的一方）
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    department_id INT,
    CONSTRAINT fk_emp_dept FOREIGN KEY (department_id) REFERENCES departments(id)
);

-- 插入测试数据
INSERT INTO departments (dept_name, location) VALUES 
('技术部', '北京'),
('市场部', '上海'),
('人事部', '广州');

INSERT INTO employees (name, department_id) VALUES 
('张三', 1),
('李四', 1),
('王五', 2),
('赵六', NULL);
```

#### 多对多

案例：学生与课程
关系：一个学生可以选多门课程，一门课程也可以供多个学生选修
实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

**详细示例：**

```sql
-- 创建学生表
CREATE TABLE students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    grade VARCHAR(20)
);

-- 创建课程表
CREATE TABLE courses (
    course_id INT PRIMARY KEY AUTO_INCREMENT,
    course_name VARCHAR(100) NOT NULL,
    credit INT NOT NULL
);

-- 创建选课中间表
CREATE TABLE student_courses (
    id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    enrollment_date DATE,
    CONSTRAINT fk_sc_student FOREIGN KEY (student_id) REFERENCES students(student_id),
    CONSTRAINT fk_sc_course FOREIGN KEY (course_id) REFERENCES courses(course_id)
);

-- 插入测试数据
INSERT INTO students (name, grade) VALUES 
('张三', '2021级'),
('李四', '2021级'),
('王五', '2022级');

INSERT INTO courses (course_name, credit) VALUES 
('数据库原理', 4),
('算法设计', 3),
('操作系统', 4);

INSERT INTO student_courses (student_id, course_id, enrollment_date) VALUES 
(1, 1, '2023-09-01'),
(1, 2, '2023-09-01'),
(2, 1, '2023-09-01'),
(2, 3, '2023-09-01'),
(3, 2, '2023-09-01');
```

#### 一对一

案例：用户与用户详情
关系：一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率
实现：在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的（UNIQUE）

**详细示例：**

```sql
-- 创建用户基本信息表
CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(30) NOT NULL UNIQUE,
    password VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    create_time DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- 创建用户详细信息表
CREATE TABLE user_profiles (
    profile_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL UNIQUE,
    real_name VARCHAR(50),
    phone VARCHAR(20),
    address VARCHAR(200),
    birthday DATE,
    CONSTRAINT fk_profile_user FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- 插入测试数据
INSERT INTO users (username, password, email) VALUES 
('zhangsan', 'password123', 'zhangsan@example.com'),
('lisi', 'password456', 'lisi@example.com');

INSERT INTO user_profiles (user_id, real_name, phone, address, birthday) VALUES 
(1, '张三', '13800138000', '北京市朝阳区', '1990-01-01'),
(2, '李四', '13900139000', '上海市浦东新区', '1992-05-15');
```

### 查询

#### 笛卡尔积查询

合并查询（笛卡尔积，会展示所有组合结果）：

```sql
SELECT * FROM employees, departments;
```

**详细示例：**

```sql
-- 笛卡尔积查询（不推荐）
SELECT * FROM employees, departments;
-- 如果employees有4条记录，departments有3条记录，结果会有12条记录

-- 查看笛卡尔积的结果
SELECT e.name AS employee_name, d.dept_name AS department_name
FROM employees e, departments d;
```

> 笛卡尔积：两个集合A集合和B集合的所有组合情况（在多表查询时，需要消除无效的笛卡尔积）

#### 消除无效笛卡尔积

```sql
SELECT * FROM employees, departments WHERE employees.department_id = departments.id;
```

**详细示例：**

```sql
-- 基本连接查询
SELECT e.*, d.dept_name, d.location
FROM employees e, departments d
WHERE e.department_id = d.id;

-- 使用表别名简化查询
SELECT e.name, e.age, d.dept_name
FROM employees e, departments d
WHERE e.department_id = d.id;

-- 带条件的连接查询
SELECT e.name, d.dept_name
FROM employees e, departments d
WHERE e.department_id = d.id
  AND e.age > 25;
```

### 内连接查询

内连接查询的是两张表交集的部分

隐式内连接：
```sql
SELECT 字段列表 FROM 表1, 表2 WHERE 条件 ...;
```

显式内连接：
```sql
SELECT 字段列表 FROM 表1 [ INNER ] JOIN 表2 ON 连接条件 ...;
```

显式性能比隐式高

**详细示例：**

1. **基本内连接**
```sql
-- 隐式内连接
SELECT e.name, d.dept_name
FROM employees e, departments d
WHERE e.department_id = d.id;

-- 显式内连接（推荐）
SELECT e.name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;

-- 简写INNER JOIN
SELECT e.name, d.dept_name
FROM employees e
JOIN departments d ON e.department_id = d.id;
```

2. **多表内连接**
```sql
-- 三表连接：员工-部门-职位
SELECT e.name, d.dept_name, p.position_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
INNER JOIN positions p ON e.position_id = p.id;

-- 四表连接：订单-客户-产品-供应商
SELECT o.order_id, c.customer_name, p.product_name, s.supplier_name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
INNER JOIN order_items oi ON o.order_id = oi.order_id
INNER JOIN products p ON oi.product_id = p.product_id
INNER JOIN suppliers s ON p.supplier_id = s.supplier_id;
```

3. **带条件的内连接**
```sql
-- 查询技术部年龄大于30的员工
SELECT e.name, e.age, d.dept_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
WHERE d.dept_name = '技术部' AND e.age > 30;

-- 查询2023年入职的员工及其部门信息
SELECT e.name, e.hire_date, d.dept_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
WHERE YEAR(e.hire_date) = 2023;
```

4. **内连接与聚合函数**
```sql
-- 统计每个部门的员工数量
SELECT d.dept_name, COUNT(e.id) AS employee_count
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.dept_name;

-- 统计每个部门的平均薪资
SELECT d.dept_name, AVG(e.salary) AS avg_salary
FROM departments d
INNER JOIN employees e ON d.id = e.department_id
GROUP BY d.dept_name
ORDER BY avg_salary DESC;
```

**执行示例：**
```sql
mysql> SELECT e.name, d.dept_name
    -> FROM employees e
    -> INNER JOIN departments d ON e.department_id = d.id;
+--------+----------+
| name   | dept_name|
+--------+----------+
| 张三   | 技术部   |
| 李四   | 技术部   |
| 王五   | 市场部   |
+--------+----------+
3 rows in set (0.00 sec)
```

### 外连接查询

左外连接：
查询左表所有数据，以及两张表交集部分数据
```sql
SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ...;
```
相当于查询表1的所有数据，包含表1和表2交集部分数据

右外连接：
查询右表所有数据，以及两张表交集部分数据
```sql
SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ...;
```

**详细示例：**

1. **左外连接**
```sql
-- 基本左外连接
SELECT e.*, d.dept_name
FROM employees e
LEFT OUTER JOIN departments d ON e.department_id = d.id;

-- 简写LEFT JOIN
SELECT e.name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;

-- 查询没有部门的员工
SELECT e.name, e.department_id
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id
WHERE d.id IS NULL;
```

2. **右外连接**
```sql
-- 基本右外连接
SELECT d.dept_name, e.name
FROM employees e
RIGHT OUTER JOIN departments d ON e.department_id = d.id;

-- 简写RIGHT JOIN
SELECT d.dept_name, e.name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;

-- 查询没有员工的部门
SELECT d.dept_name, d.id
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id
WHERE e.id IS NULL;
```

3. **左连接 vs 右连接**
```sql
-- 左连接：以左表为主
SELECT d.name, e.* FROM departments d
LEFT JOIN employees e ON e.department_id = d.id;

-- 右连接：以右表为主（与上面效果相同）
SELECT d.name, e.* FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
```

4. **外连接的实际应用**
```sql
-- 查询所有员工的订单信息（包括没有订单的员工）
SELECT e.name, e.email, COUNT(o.order_id) AS order_count
FROM employees e
LEFT JOIN orders o ON e.id = o.employee_id
GROUP BY e.id, e.name, e.email
ORDER BY order_count DESC;

-- 查询所有产品的销售情况（包括没有销售的产品）
SELECT p.product_name, p.stock, SUM(oi.quantity) AS sold_quantity
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY p.product_id, p.product_name, p.stock;
```

**执行示例：**
```sql
mysql> SELECT e.name, d.dept_name
    -> FROM employees e
    -> LEFT JOIN departments d ON e.department_id = d.id;
+--------+----------+
| name   | dept_name|
+--------+----------+
| 张三   | 技术部   |
| 李四   | 技术部   |
| 王五   | 市场部   |
| 赵六   | NULL     |
+--------+----------+
4 rows in set (0.00 sec)

mysql> SELECT e.name, d.dept_name
    -> FROM employees e
    -> RIGHT JOIN departments d ON e.department_id = d.id;
+--------+----------+
| name   | dept_name|
+--------+----------+
| 张三   | 技术部   |
| 李四   | 技术部   |
| 王五   | 市场部   |
| NULL   | 人事部   |
+--------+----------+
4 rows in set (0.00 sec)
```

### 自连接查询

当前表与自身的连接查询，自连接必须使用表别名

语法：
```sql
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ...;
```

自连接查询，可以是内连接查询，也可以是外连接查询

**详细示例：**

1. **基本自连接**
```sql
-- 查询员工及其所属领导的名字
SELECT 
    e.name AS employee_name,
    m.name AS manager_name
FROM employees e
INNER JOIN employees m ON e.manager_id = m.id;

-- 使用表别名简化
SELECT a.name AS employee, b.name AS manager
FROM employees a, employees b
WHERE a.manager_id = b.id;
```

2. **左外连接自连接**
```sql
-- 查询所有员工及其领导（包括没有领导的员工）
SELECT 
    e.name AS employee_name,
    m.name AS manager_name
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;

-- 查询没有领导的员工
SELECT e.name, e.position
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id
WHERE m.id IS NULL;
```

3. **多层级自连接**
```sql
-- 查询员工的领导及其领导的领导（三级结构）
SELECT 
    e.name AS employee,
    m.name AS manager,
    mm.name AS grand_manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id
LEFT JOIN employees mm ON m.manager_id = mm.id;
```

4. **实际应用场景**
```sql
-- 场景1：组织架构查询
-- 查询员工及其上级的完整路径
SELECT 
    e.name AS employee_name,
    CONCAT(
        COALESCE(mm.name, ''), ' -> ',
        COALESCE(m.name, ''), ' -> ',
        e.name
    ) AS management_chain
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id
LEFT JOIN employees mm ON m.manager_id = mm.id;

-- 场景2：地区层级查询
SELECT 
    c1.city_name AS city,
    c2.city_name AS province,
    c3.city_name AS country
FROM cities c1
LEFT JOIN cities c2 ON c1.parent_id = c2.city_id
LEFT JOIN cities c3 ON c2.parent_id = c3.city_id;
```

**执行示例：**
```sql
mysql> SELECT e.name AS employee_name, m.name AS manager_name
    -> FROM employees e
    -> LEFT JOIN employees m ON e.manager_id = m.id;
+--------------+--------------+
| employee_name| manager_name |
+--------------+--------------+
| 张三         | 王总        |
| 李四         | 王总        |
| 王五         | NULL        |
| 赵六         | 李四        |
+--------------+--------------+
4 rows in set (0.00 sec)
```

### 联合查询 UNION, UNION ALL

把多次查询的结果合并，形成一个新的查询集

语法：

```sql
SELECT 字段列表 FROM 表A ...
UNION [ALL]
SELECT 字段列表 FROM 表B ...
```

**详细示例：**

1. **UNION vs UNION ALL**
```sql
-- UNION：去除重复记录
SELECT name FROM employees WHERE department_id = 1
UNION
SELECT name FROM employees WHERE age > 30;

-- UNION ALL：保留所有记录（包括重复的）
SELECT name FROM employees WHERE department_id = 1
UNION ALL
SELECT name FROM employees WHERE age > 30;
```

2. **基本联合查询**
```sql
-- 查询北京和上海的所有员工
SELECT name, '北京' AS location FROM employees WHERE location = '北京'
UNION
SELECT name, '上海' AS location FROM employees WHERE location = '上海';

-- 查询不同表的数据合并
SELECT name, email, 'employee' AS user_type FROM employees
UNION
SELECT name, email, 'customer' AS user_type FROM customers;
```

3. **带排序的联合查询**
```sql
-- 对联合查询结果排序
SELECT name, salary FROM employees WHERE department_id = 1
UNION
SELECT name, salary FROM employees WHERE department_id = 2
ORDER BY salary DESC;

-- 注意：ORDER BY要放在最后一个查询后面
```

4. **实际应用场景**
```sql
-- 场景1：数据报表合并
SELECT '2023-01' AS month, SUM(amount) AS total_amount FROM sales WHERE sale_date LIKE '2023-01%'
UNION ALL
SELECT '2023-02' AS month, SUM(amount) AS total_amount FROM sales WHERE sale_date LIKE '2023-02%'
UNION ALL
SELECT '2023-03' AS month, SUM(amount) AS total_amount FROM sales WHERE sale_date LIKE '2023-03%';

-- 场景2：多条件数据汇总
SELECT 
    department_id, 
    COUNT(*) AS employee_count, 
    'active' AS status
FROM employees 
WHERE status = 'active'
GROUP BY department_id
UNION ALL
SELECT 
    department_id, 
    COUNT(*) AS employee_count, 
    'inactive' AS status
FROM employees 
WHERE status = 'inactive'
GROUP BY department_id;
```

#### 注意事项

- UNION ALL 会有重复结果，UNION 不会
- 联合查询比使用or效率高，不会使索引失效
- UNION查询的列数和列类型必须相同
- UNION结果集的列名以第一个查询为准
- ORDER BY只能对最终结果集排序

### 子查询

SQL语句中嵌套SELECT语句，称为嵌套查询，又称子查询。

```sql
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
```

**子查询外部的语句可以是 INSERT / UPDATE / DELETE / SELECT 的任何一个**

根据子查询结果可以分为：

- 标量子查询（子查询结果为单个值）
- 列子查询（子查询结果为一列）
- 行子查询（子查询结果为一行）
- 表子查询（子查询结果为多行多列）

根据子查询位置可分为：

- WHERE 之后
- FROM 之后
- SELECT 之后

#### 标量子查询

子查询返回的结果是单个值（数字、字符串、日期等）。
常用操作符：=, <>, >, >=, <, <=

**详细示例：**

1. **基本标量子查询**
```sql
-- 查询销售部所有员工
-- 分步查询：
-- 第一步：查询销售部的部门ID
SELECT id FROM departments WHERE dept_name = '销售部';
-- 第二步：根据部门ID查询员工信息
SELECT * FROM employees WHERE department_id = 4;
-- 合并为子查询：
SELECT * FROM employees WHERE department_id = (SELECT id FROM departments WHERE dept_name = '销售部');

-- 查询比平均薪资高的员工
SELECT * FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);

-- 查询入职日期最早的员工
SELECT * FROM employees WHERE hire_date = (SELECT MIN(hire_date) FROM employees);
```

2. **实际应用场景**
```sql
-- 查询特定条件的员工
SELECT * FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees WHERE department_id = 1);

-- 查询与某员工同一天入职的其他员工
SELECT * FROM employees 
WHERE hire_date = (SELECT hire_date FROM employees WHERE name = '张三')
  AND name != '张三';

-- 查询年龄大于部门平均年龄的员工
SELECT e.name, e.age, e.department_id
FROM employees e
WHERE e.age > (
    SELECT AVG(e2.age) 
    FROM employees e2 
    WHERE e2.department_id = e.department_id
);
```

#### 列子查询

返回的结果是一列（可以是多行）。

常用操作符：

| 操作符  | 描述  |
| ------------ | ------------ |
| IN  | 在指定的集合范围内，多选一  |
| NOT IN  | 不在指定的集合范围内  |
| ANY  | 子查询返回列表中，有任意一个满足即可  |
| SOME  | 与ANY等同，使用SOME的地方都可以使用ANY  |
| ALL  | 子查询返回列表的所有值都必须满足  |

**详细示例：**

1. **IN 和 NOT IN**
```sql
-- 查询销售部和市场部的所有员工信息
SELECT * FROM employees 
WHERE department_id IN (SELECT id FROM departments WHERE dept_name IN ('销售部', '市场部'));

-- 查询不在特定部门的员工
SELECT * FROM employees 
WHERE department_id NOT IN (SELECT id FROM departments WHERE location = '北京');
```

2. **ANY 和 SOME**
```sql
-- 查询比研发部任意一人工资高的员工信息
SELECT * FROM employees 
WHERE salary > ANY (SELECT salary FROM employees WHERE department_id = (SELECT id FROM departments WHERE dept_name = '研发部'));

-- 等价于
SELECT * FROM employees 
WHERE salary > (SELECT MIN(salary) FROM employees WHERE department_id = (SELECT id FROM departments WHERE dept_name = '研发部'));
```

3. **ALL**
```sql
-- 查询比财务部所有人工资都高的员工信息
SELECT * FROM employees 
WHERE salary > ALL (SELECT salary FROM employees WHERE department_id = (SELECT id FROM departments WHERE dept_name = '财务部'));

-- 等价于
SELECT * FROM employees 
WHERE salary > (SELECT MAX(salary) FROM employees WHERE department_id = (SELECT id FROM departments WHERE dept_name = '财务部'));
```

4. **实际应用场景**
```sql
-- 查询有订单的客户
SELECT * FROM customers 
WHERE customer_id IN (SELECT DISTINCT customer_id FROM orders);

-- 查询没有订单的客户
SELECT * FROM customers 
WHERE customer_id NOT IN (SELECT DISTINCT customer_id FROM orders);

-- 查询薪资高于任意部门平均薪资的员工
SELECT e.name, e.salary, e.department_id
FROM employees e
WHERE e.salary > ANY (
    SELECT AVG(e2.salary) 
    FROM employees e2 
    GROUP BY e2.department_id
);
```

#### 行子查询

返回的结果是一行（可以是多列）。
常用操作符：=, <>, >, <, IN, NOT IN

**详细示例：**

1. **基本行子查询**
```sql
-- 查询与张三的薪资及直属领导相同的员工信息
SELECT * FROM employees 
WHERE (salary, manager_id) = (SELECT salary, manager_id FROM employees WHERE name = '张三');

-- 查询与特定员工部门和职位相同的员工
SELECT * FROM employees 
WHERE (department_id, position_id) = (
    SELECT department_id, position_id 
    FROM employees 
    WHERE name = '李四'
);
```

2. **实际应用场景**
```sql
-- 查询与最佳销售员销售额相同的其他销售员
SELECT * FROM salespeople 
WHERE (sales_amount, region) = (
    SELECT sales_amount, region 
    FROM salespeople 
    ORDER BY sales_amount DESC 
    LIMIT 1
);

-- 查询符合特定组合条件的记录
SELECT * FROM products 
WHERE (category_id, price) = (
    SELECT category_id, MAX(price) 
    FROM products 
    GROUP BY category_id 
    ORDER BY MAX(price) DESC 
    LIMIT 1
);
```

#### 表子查询

返回的结果是多行多列
常用操作符：IN

**详细示例：**

1. **基本表子查询**
```sql
-- 查询薪资等级为A的员工信息
SELECT * FROM employees 
WHERE (salary, department_id) IN (
    SELECT salary, department_id 
    FROM salary_grades 
    WHERE grade = 'A'
);

-- 查询每个部门薪资最高的员工
SELECT e.name, e.salary, e.department_id
FROM employees e
WHERE (e.department_id, e.salary) IN (
    SELECT department_id, MAX(salary)
    FROM employees
    GROUP BY department_id
);
```

2. **FROM子句中的表子查询**
```sql
-- 查询平均薪资大于公司平均薪资的部门
SELECT d.dept_name, avg_dept_salary.avg_salary
FROM departments d
INNER JOIN (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) avg_dept_salary ON d.id = avg_dept_salary.department_id
WHERE avg_dept_salary.avg_salary > (SELECT AVG(salary) FROM employees);

-- 使用表子查询进行复杂统计
SELECT 
    employee_count,
    COUNT(*) AS department_count
FROM (
    SELECT department_id, COUNT(*) AS employee_count
    FROM employees
    GROUP BY department_id
) dept_stats
GROUP BY employee_count;
```

3. **实际应用场景**
```sql
-- 场景1：排名查询
SELECT * FROM (
    SELECT 
        name, 
        salary, 
        RANK() OVER (ORDER BY salary DESC) AS salary_rank
    FROM employees
) ranked_employees
WHERE salary_rank <= 10;

-- 场景2：复杂条件筛选
SELECT * FROM products p
WHERE p.product_id IN (
    SELECT product_id FROM (
        SELECT product_id, SUM(quantity) AS total_sold
        FROM order_items
        GROUP BY product_id
        HAVING total_sold > 100
        ORDER BY total_sold DESC
        LIMIT 10
    ) top_products
);
```

**子查询性能优化建议：**

1. **优先使用JOIN替代子查询**
```sql
-- 不推荐：子查询
SELECT * FROM employees 
WHERE department_id IN (SELECT id FROM departments WHERE location = '北京');

-- 推荐：JOIN
SELECT e.* FROM employees e
INNER JOIN departments d ON e.department_id = d.id
WHERE d.location = '北京';
```

2. **使用EXISTS替代IN**
```sql
-- 不推荐：IN子查询
SELECT * FROM employees e
WHERE e.department_id IN (SELECT id FROM departments WHERE location = '北京');

-- 推荐：EXISTS
SELECT * FROM employees e
WHERE EXISTS (SELECT 1 FROM departments d WHERE d.id = e.department_id AND d.location = '北京');
```

3. **避免相关子查询**
```sql
-- 不推荐：相关子查询（每行都要执行一次子查询）
SELECT e.name, e.salary
FROM employees e
WHERE e.salary > (SELECT AVG(e2.salary) FROM employees e2 WHERE e2.department_id = e.department_id);

-- 推荐：使用JOIN
SELECT e.name, e.salary, d.avg_salary
FROM employees e
INNER JOIN (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) d ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```
-- 查询与xxx1，xxx2的职位和薪资相同的员工
select * from employee where (job, salary) in (select job, salary from employee where name = 'xxx1' or name = 'xxx2');
-- 查询入职日期是2006-01-01之后的员工，及其部门信息
select e.*, d.* from (select * from employee where entrydate > '2006-01-01') as e left join dept as d on e.dept = d.id;
```

## 事务

事务是一组操作的集合，事务会把所有操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

基本操作：

```mysql
-- 1. 查询张三账户余额
select * from account where name = '张三';
-- 2. 将张三账户余额-1000
update account set money = money - 1000 where name = '张三';
-- 此语句出错后张三钱减少但是李四钱没有增加
模拟sql语句错误
-- 3. 将李四账户余额+1000
update account set money = money + 1000 where name = '李四';

-- 查看事务提交方式
SELECT @@AUTOCOMMIT;
-- 设置事务提交方式，1为自动提交，0为手动提交，该设置只对当前会话有效
SET @@AUTOCOMMIT = 0;
-- 提交事务
COMMIT;
-- 回滚事务
ROLLBACK;

-- 设置手动提交后上面代码改为：
select * from account where name = '张三';
update account set money = money - 1000 where name = '张三';
update account set money = money + 1000 where name = '李四';
commit;
```

操作方式二：

开启事务：
`START TRANSACTION 或 BEGIN TRANSACTION;`
提交事务：
`COMMIT;`
回滚事务：
`ROLLBACK;`

操作实例：

```mysql
start transaction;
select * from account where name = '张三';
update account set money = money - 1000 where name = '张三';
update account set money = money + 1000 where name = '李四';
commit;
```

### 四大特性ACID

- 原子性(Atomicity)：事务是不可分割的最小操作但愿，要么全部成功，要么全部失败
- 一致性(Consistency)：事务完成时，必须使所有数据都保持一致状态
- 隔离性(Isolation)：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
- 持久性(Durability)：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

### 并发事务

| 问题  | 描述  |
| ------------ | ------------ |
| 脏读  | 一个事务读到另一个事务还没提交的数据  |
| 不可重复读  | 一个事务先后读取同一条记录，但两次读取的数据不同  |
| 幻读  | 一个事务按照条件查询数据时，没有对应的数据行，但是再插入数据时，又发现这行数据已经存在  |

> 这三个问题的详细演示：https://www.bilibili.com/video/BV1Kr4y1i7ru?p=55cd 

并发事务隔离级别：

| 隔离级别  | 脏读  | 不可重复读  | 幻读  |
| ------------ | ------------ | ------------ | ------------ |
| Read uncommitted  | √  | √  | √  |
| Read committed  | ×  | √  | √  |
| Repeatable Read(默认)  | ×  | ×  | √  |
| Serializable  | ×  | ×  | ×  |

- √表示在当前隔离级别下该问题会出现
- Serializable 性能最低；Read uncommitted 性能最高，数据安全性最差

查看事务隔离级别：
`SELECT @@TRANSACTION_ISOLATION;`
设置事务隔离级别：
`SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE };`
SESSION 是会话级别，表示只针对当前会话有效，GLOBAL 表示对所有会话有效

# 进阶篇

## 存储引擎

MySQL体系结构：

![结构图](https://dhc.pythonanywhere.com/media/editor/MySQL体系结构_20220315034329549927.png "结构图")
![层级描述](https://dhc.pythonanywhere.com/media/editor/MySQL体系结构层级含义_20220315034359342837.png "层级描述")

存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式。存储引擎是基于表而不是基于库的，所以存储引擎也可以被称为表引擎。
默认存储引擎是InnoDB。

相关操作：

```mysql
-- 查询建表语句
show create table account;
-- 建表时指定存储引擎
CREATE TABLE 表名(
	...
) ENGINE=INNODB;
-- 查看当前数据库支持的存储引擎
show engines;
```

### InnoDB

InnoDB 是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL 5.5 之后，InnoDB 是默认的 MySQL 引擎。

特点：

- DML 操作遵循 ACID 模型，支持**事务**
- **行级锁**，提高并发访问性能
- 支持**外键**约束，保证数据的完整性和正确性

文件：

- xxx.ibd: xxx代表表名，InnoDB 引擎的每张表都会对应这样一个表空间文件，存储该表的表结构（frm、sdi）、数据和索引。

参数：innodb_file_per_table，决定多张表共享一个表空间还是每张表对应一个表空间

知识点：

查看 Mysql 变量：
`show variables like 'innodb_file_per_table';`

从idb文件提取表结构数据：
（在cmd运行）
`ibd2sdi xxx.ibd`

InnoDB 逻辑存储结构：
![InnoDB逻辑存储结构](https://dhc.pythonanywhere.com/media/editor/逻辑存储结构_20220316030616590001.png "InnoDB逻辑存储结构")

### MyISAM

MyISAM 是 MySQL 早期的默认存储引擎。

特点：

- 不支持事务，不支持外键
- 支持表锁，不支持行锁
- 访问速度快

文件：

- xxx.sdi: 存储表结构信息
- xxx.MYD: 存储数据
- xxx.MYI: 存储索引

### Memory

Memory 引擎的表数据是存储在内存中的，受硬件问题、断电问题的影响，只能将这些表作为临时表或缓存使用。

特点：

- 存放在内存中，速度快
- hash索引（默认）

文件：

- xxx.sdi: 存储表结构信息

### 存储引擎特点

| 特点  | InnoDB  | MyISAM  | Memory  |
| ------------ | ------------ | ------------ | ------------ |
| 存储限制  | 64TB  | 有  | 有  |
| 事务安全  | 支持  | -  | -  |
| 锁机制  | 行锁  | 表锁  | 表锁  |
| B+tree索引  | 支持  | 支持  | 支持  |
| Hash索引  | -  | -  | 支持  |
| 全文索引  | 支持（5.6版本之后）  | 支持  | -  |
| 空间使用  | 高  | 低  | N/A  |
| 内存使用  | 高  | 低  | 中等  |
| 批量插入速度  | 低  | 高  | 高  |
| 支持外键  | 支持  | -  | -  |

### 存储引擎的选择

在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合。

- InnoDB: 如果应用对事物的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包含很多的更新、删除操作，则 InnoDB 是比较合适的选择
- MyISAM: 如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不高，那这个存储引擎是非常合适的。
- Memory: 将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。Memory 的缺陷是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性

电商中的足迹和评论适合使用 MyISAM 引擎，缓存适合使用 Memory 引擎。

## 性能分析

### 查看执行频次

查看当前数据库的 INSERT, UPDATE, DELETE, SELECT 访问频次：
`SHOW GLOBAL STATUS LIKE 'Com_______';` 或者 `SHOW SESSION STATUS LIKE 'Com_______';`
例：`show global status like 'Com_______'`

### 慢查询日志

慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句的日志。
MySQL的慢查询日志默认没有开启，需要在MySQL的配置文件（/etc/my.cnf）中配置如下信息：
	# 开启慢查询日志开关
	slow_query_log=1
	# 设置慢查询日志的时间为2秒，SQL语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
	long_query_time=2
更改后记得重启MySQL服务，日志文件位置：/var/lib/mysql/localhost-slow.log

查看慢查询日志开关状态：
`show variables like 'slow_query_log';`

### profile

show profile 能在做SQL优化时帮我们了解时间都耗费在哪里。通过 have_profiling 参数，能看到当前 MySQL 是否支持 profile 操作：
`SELECT @@have_profiling;`
profiling 默认关闭，可以通过set语句在session/global级别开启 profiling：
`SET profiling = 1;`
查看所有语句的耗时：
`show profiles;`
查看指定query_id的SQL语句各个阶段的耗时：
`show profile for query query_id;`
查看指定query_id的SQL语句CPU的使用情况
`show profile cpu for query query_id;`

### explain

EXPLAIN 或者 DESC 命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中表如何连接和连接的顺序。
语法：
	# 直接在select语句之前加上关键字 explain / desc
	EXPLAIN SELECT 字段列表 FROM 表名 HWERE 条件;

EXPLAIN 各字段含义：

- id：select 查询的序列号，表示查询中执行 select 子句或者操作表的顺序（id相同，执行顺序从上到下；id不同，值越大越先执行）
- select_type：表示 SELECT 的类型，常见取值有 SIMPLE（简单表，即不适用表连接或者子查询）、PRIMARY（主查询，即外层的查询）、UNION（UNION中的第二个或者后面的查询语句）、SUBQUERY（SELECT/WHERE之后包含了子查询）等
- type：表示连接类型，性能由好到差的连接类型为 NULL、system、const、eq_ref、ref、range、index、all
- possible_key：可能应用在这张表上的索引，一个或多个
- Key：实际使用的索引，如果为 NULL，则没有使用索引
- Key_len：表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好
- rows：MySQL认为必须要执行的行数，在InnoDB引擎的表中，是一个估计值，可能并不总是准确的
- filtered：表示返回结果的行数占需读取行数的百分比，filtered的值越大越好

## 索引

索引是帮助 MySQL **高效获取数据**的**数据结构（有序）**。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查询算法，这种数据结构就是索引。

优缺点：

优点：

- 提高数据检索效率，降低数据库的IO成本
- 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗

缺点：

- 索引列也是要占用空间的
- 索引大大提高了查询效率，但降低了更新的速度，比如 INSERT、UPDATE、DELETE

### 索引结构

| 索引结构  | 描述  |
| ------------ | ------------ |
| B+Tree  | 最常见的索引类型，大部分引擎都支持B+树索引  |
| Hash  | 底层数据结构是用哈希表实现，只有精确匹配索引列的查询才有效，不支持范围查询  |
| R-Tree(空间索引)  | 空间索引是 MyISAM 引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少  |
| Full-Text(全文索引)  | 是一种通过建立倒排索引，快速匹配文档的方式，类似于 Lucene, Solr, ES  |

| 索引  | InnoDB  | MyISAM  | Memory  |
| ------------ | ------------ | ------------ | ------------ |
| B+Tree索引  | 支持  | 支持  | 支持  |
| Hash索引  | 不支持  | 不支持  | 支持  |
| R-Tree索引  | 不支持  | 支持  | 不支持  |
| Full-text  | 5.6版本后支持  | 支持  | 不支持  |

#### B-Tree

![二叉树](https://dhc.pythonanywhere.com/media/editor/二叉树_20220316153214227108.png "二叉树")

二叉树的缺点可以用红黑树来解决：
![红黑树](https://dhc.pythonanywhere.com/media/editor/红黑树_20220316163142686602.png "红黑树")
红黑树也存在大数据量情况下，层级较深，检索速度慢的问题。

为了解决上述问题，可以使用 B-Tree 结构。
B-Tree (多路平衡查找树) 以一棵最大度数（max-degree，指一个节点的子节点个数）为5（5阶）的 b-tree 为例（每个节点最多存储4个key，5个指针）

![B-Tree结构](https://dhc.pythonanywhere.com/media/editor/B-Tree结构_20220316163813441163.png "B-Tree结构")

> B-Tree 的数据插入过程动画参照：https://www.bilibili.com/video/BV1Kr4y1i7ru?p=68
演示地址：https://www.cs.usfca.edu/~galles/visualization/BTree.html

#### B+Tree

结构图：

![B+Tree结构图](https://dhc.pythonanywhere.com/media/editor/B+Tree结构图_20220316170700591277.png "B+Tree结构图")

> 演示地址：https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html

与 B-Tree 的区别：

- 所有的数据都会出现在叶子节点
- 叶子节点形成一个单向链表

MySQL 索引数据结构对经典的 B+Tree 进行了优化。在原 B+Tree 的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的 B+Tree，提高区间访问的性能。

![MySQL B+Tree 结构图](https://dhc.pythonanywhere.com/media/editor/结构图_20220316171730865611.png "MySQL B+Tree 结构图")

#### Hash

哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。
如果两个（或多个）键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决。

![Hash索引原理图](https://dhc.pythonanywhere.com/media/editor/Hash索引原理图_20220317143226150679.png "Hash索引原理图")

特点：

- Hash索引只能用于对等比较（=、in），不支持范围查询（betwwn、>、<、...）
- 无法利用索引完成排序操作
- 查询效率高，通常只需要一次检索就可以了，效率通常要高于 B+Tree 索引

存储引擎支持：

- Memory
- InnoDB: 具有自适应hash功能，hash索引是存储引擎根据 B+Tree 索引在指定条件下自动构建的

#### 面试题

1. 为什么 InnoDB 存储引擎选择使用 B+Tree 索引结构？

- 相对于二叉树，层级更少，搜索效率高
- 对于 B-Tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，指针也跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低
- 相对于 Hash 索引，B+Tree 支持范围匹配及排序操作

### 索引分类

| 分类  | 含义  | 特点  | 关键字  |
| ------------ | ------------ | ------------ | ------------ |
| 主键索引  | 针对于表中主键创建的索引  | 默认自动创建，只能有一个  | PRIMARY  |
| 唯一索引  | 避免同一个表中某数据列中的值重复  | 可以有多个  | UNIQUE  |
| 常规索引  | 快速定位特定数据  | 可以有多个  |   |
| 全文索引  | 全文索引查找的是文本中的关键词，而不是比较索引中的值  | 可以有多个  | FULLTEXT  |

在 InnoDB 存储引擎中，根据索引的存储形式，又可以分为以下两种：

| 分类  | 含义  | 特点  |
| ------------ | ------------ | ------------ |
| 聚集索引(Clustered Index)  | 将数据存储与索引放一块，索引结构的叶子节点保存了行数据  | 必须有，而且只有一个  |
| 二级索引(Secondary Index)  | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键  | 可以存在多个  |

演示图：

![大致原理](https://dhc.pythonanywhere.com/media/editor/原理图_20220318194454880073.png "大致原理")
![演示图](https://dhc.pythonanywhere.com/media/editor/演示图_20220319215403721066.png "演示图")

聚集索引选取规则：

- 如果存在主键，主键索引就是聚集索引
- 如果不存在主键，将使用第一个唯一(UNIQUE)索引作为聚集索引
- 如果表没有主键或没有合适的唯一索引，则 InnoDB 会自动生成一个 rowid 作为隐藏的聚集索引

#### 思考题

1\. 以下 SQL 语句，哪个执行效率高？为什么？

```mysql
select * from user where id = 10;
select * from user where name = 'Arm';
-- 备注：id为主键，name字段创建的有索引
```

答：第一条语句，因为第二条需要回表查询，相当于两个步骤。

2\. InnoDB 主键索引的 B+Tree 高度为多少？

答：假设一行数据大小为1k，一页中可以存储16行这样的数据。InnoDB 的指针占用6个字节的空间，主键假设为bigint，占用字节数为8.
可得公式：`n * 8 + (n + 1) * 6 = 16 * 1024`，其中 8 表示 bigint 占用的字节数，n 表示当前节点存储的key的数量，(n + 1) 表示指针数量（比key多一个）。算出n约为1170。

如果树的高度为2，那么他能存储的数据量大概为：`1171 * 16 = 18736`；
如果树的高度为3，那么他能存储的数据量大概为：`1171 * 1171 * 16 = 21939856`。

另外，如果有成千上万的数据，那么就要考虑分表，涉及运维篇知识。

### 语法

创建索引：
`CREATE [ UNIQUE | FULLTEXT ] INDEX index_name ON table_name (index_col_name, ...);`
如果不加 CREATE 后面不加索引类型参数，则创建的是常规索引

查看索引：
`SHOW INDEX FROM table_name;`

删除索引：
`DROP INDEX index_name ON table_name;`

案例：

```mysql
-- name字段为姓名字段，该字段的值可能会重复，为该字段创建索引
create index idx_user_name on tb_user(name);
-- phone手机号字段的值非空，且唯一，为该字段创建唯一索引
create unique index idx_user_phone on tb_user (phone);
-- 为profession, age, status创建联合索引
create index idx_user_pro_age_stat on tb_user(profession, age, status);
-- 为email建立合适的索引来提升查询效率
create index idx_user_email on tb_user(email);

-- 删除索引
drop index idx_user_email on tb_user;
```

### 使用规则

#### 最左前缀法则

如果索引关联了多列（联合索引），要遵守最左前缀法则，最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列。
如果跳跃某一列，索引将部分失效（后面的字段索引失效）。

联合索引中，出现范围查询（<, >），范围查询右侧的列索引失效。可以用>=或者<=来规避索引失效问题。

#### 索引失效情况

1. 在索引列上进行运算操作，索引将失效。如：`explain select * from tb_user where substring(phone, 10, 2) = '15';`
2. 字符串类型字段使用时，不加引号，索引将失效。如：`explain select * from tb_user where phone = 17799990015;`，此处phone的值没有加引号
3. 模糊查询中，如果仅仅是尾部模糊匹配，索引不会是失效；如果是头部模糊匹配，索引失效。如：`explain select * from tb_user where profession like '%工程';`，前后都有 % 也会失效。
4. 用 or 分割开的条件，如果 or 其中一个条件的列没有索引，那么涉及的索引都不会被用到。
5. 如果 MySQL 评估使用索引比全表更慢，则不使用索引。

#### SQL 提示

是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的。

例如，使用索引：
`explain select * from tb_user use index(idx_user_pro) where profession="软件工程";`
不使用哪个索引：
`explain select * from tb_user ignore index(idx_user_pro) where profession="软件工程";`
必须使用哪个索引：
`explain select * from tb_user force index(idx_user_pro) where profession="软件工程";`

use 是建议，实际使用哪个索引 MySQL 还会自己权衡运行速度去更改，force就是无论如何都强制使用该索引。

#### 覆盖索引&回表查询

尽量使用覆盖索引（查询使用了索引，并且需要返回的列，在该索引中已经全部能找到），减少 select *。

explain 中 extra 字段含义：
`using index condition`：查找使用了索引，但是需要回表查询数据
`using where; using index;`：查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询

如果在聚集索引中直接能找到对应的行，则直接返回行数据，只需要一次查询，哪怕是select \*；如果在辅助索引中找聚集索引，如`select id, name from xxx where name='xxx';`，也只需要通过辅助索引(name)查找到对应的id，返回name和name索引对应的id即可，只需要一次查询；如果是通过辅助索引查找其他字段，则需要回表查询，如`select id, name, gender from xxx where name='xxx';`

所以尽量不要用`select *`，容易出现回表查询，降低效率，除非有联合索引包含了所有字段

面试题：一张表，有四个字段（id, username, password, status），由于数据量大，需要对以下SQL语句进行优化，该如何进行才是最优方案：
`select id, username, password from tb_user where username='itcast';`

解：给username和password字段建立联合索引，则不需要回表查询，直接覆盖索引

#### 前缀索引

当字段类型为字符串（varchar, text等）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO，影响查询效率，此时可以只降字符串的一部分前缀，建立索引，这样可以大大节约索引空间，从而提高索引效率。

语法：`create index idx_xxxx on table_name(columnn(n));`
前缀长度：可以根据索引的选择性来决定，而选择性是指不重复的索引值（基数）和数据表的记录总数的比值，索引选择性越高则查询效率越高，唯一索引的选择性是1，这是最好的索引选择性，性能也是最好的。
求选择性公式：
```mysql
select count(distinct email) / count(*) from tb_user;
select count(distinct substring(email, 1, 5)) / count(*) from tb_user;
```

show index 里面的sub_part可以看到接取的长度

#### 单列索引&联合索引

单列索引：即一个索引只包含单个列
联合索引：即一个索引包含了多个列
在业务场景中，如果存在多个查询条件，考虑针对于查询字段建立索引时，建议建立联合索引，而非单列索引。

单列索引情况：
`explain select id, phone, name from tb_user where phone = '17799990010' and name = '韩信';`
这句只会用到phone索引字段

##### 注意事项

- 多条件联合查询时，MySQL优化器会评估哪个字段的索引效率更高，会选择该索引完成本次查询

### 设计原则

1. 针对于数据量较大，且查询比较频繁的表建立索引
2. 针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高
4. 如果是字符串类型的字段，字段长度较长，可以针对于字段的特点，建立前缀索引
5. 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率
6. 要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价就越大，会影响增删改的效率
7. 如果索引列不能存储NULL值，请在创建表时使用NOT NULL约束它。当优化器知道每列是否包含NULL值时，它可以更好地确定哪个索引最有效地用于查询

## SQL 优化

### 插入数据

普通插入：

1. 采用批量插入（一次插入的数据不建议超过1000条）
2. 手动提交事务
3. 主键顺序插入

大批量插入：
如果一次性需要插入大批量数据，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令插入。

```mysql
# 客户端连接服务端时，加上参数 --local-infile（这一行在bash/cmd界面输入）
mysql --local-infile -u root -p
# 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
set global local_infile = 1;
select @@local_infile;
# 执行load指令将准备好的数据，加载到表结构中
load data local infile '/root/sql1.log' into table 'tb_user' fields terminated by ',' lines terminated by '\n';
```

### 主键优化

数据组织方式：在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表（Index organized table, IOT）

页分裂：页可以为空，也可以填充一般，也可以填充100%，每个页包含了2-N行数据（如果一行数据过大，会行溢出），根据主键排列。
页合并：当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged）为删除并且它的空间变得允许被其他记录声明使用。当页中删除的记录到达 MERGE_THRESHOLD（默认为页的50%），InnoDB会开始寻找最靠近的页（前后）看看是否可以将这两个页合并以优化空间使用。

MERGE_THRESHOLD：合并页的阈值，可以自己设置，在创建表或创建索引时指定

> 文字说明不够清晰明了，具体可以看视频里的PPT演示过程：https://www.bilibili.com/video/BV1Kr4y1i7ru?p=90

主键设计原则：

- 满足业务需求的情况下，尽量降低主键的长度
- 插入数据时，尽量选择顺序插入，选择使用 AUTO_INCREMENT 自增主键
- 尽量不要使用 UUID 做主键或者是其他的自然主键，如身份证号
- 业务操作时，避免对主键的修改

### order by优化

1. Using filesort：通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区 sort buffer 中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序
2. Using index：通过有序索引顺序扫描直接返回有序数据，这种情况即为 using index，不需要额外排序，操作效率高

如果order by字段全部使用升序排序或者降序排序，则都会走索引，但是如果一个字段升序排序，另一个字段降序排序，则不会走索引，explain的extra信息显示的是`Using index, Using filesort`，如果要优化掉Using filesort，则需要另外再创建一个索引，如：`create index idx_user_age_phone_ad on tb_user(age asc, phone desc);`，此时使用`select id, age, phone from tb_user order by age asc, phone desc;`会全部走索引

总结：

- 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则
- 尽量使用覆盖索引
- 多字段排序，一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）
- 如果不可避免出现filesort，大数据量排序时，可以适当增大排序缓冲区大小 sort_buffer_size（默认256k）

### group by优化

- 在分组操作时，可以通过索引来提高效率
- 分组操作时，索引的使用也是满足最左前缀法则的

如索引为`idx_user_pro_age_stat`，则句式可以是`select ... where profession order by age`，这样也符合最左前缀法则

### limit优化

常见的问题如`limit 2000000, 10`，此时需要 MySQL 排序前2000000条记录，但仅仅返回2000000 - 2000010的记录，其他记录丢弃，查询排序的代价非常大。
优化方案：一般分页查询时，通过创建覆盖索引能够比较好地提高性能，可以通过覆盖索引加子查询形式进行优化

例如：

```mysql
-- 此语句耗时很长
select * from tb_sku limit 9000000, 10;
-- 通过覆盖索引加快速度，直接通过主键索引进行排序及查询
select id from tb_sku order by id limit 9000000, 10;
-- 下面的语句是错误的，因为 MySQL 不支持 in 里面使用 limit
-- select * from tb_sku where id in (select id from tb_sku order by id limit 9000000, 10);
-- 通过连表查询即可实现第一句的效果，并且能达到第二句的速度
select * from tb_sku as s, (select id from tb_sku order by id limit 9000000, 10) as a where s.id = a.id;
```

### count优化

MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 count(\*) 的时候会直接返回这个数，效率很高（前提是不适用where）；
InnoDB 在执行 count(\*) 时，需要把数据一行一行地从引擎里面读出来，然后累计计数。
优化方案：自己计数，如创建key-value表存储在内存或硬盘，或者是用redis

count的几种用法：

- 如果count函数的参数（count里面写的那个字段）不是NULL（字段值不为NULL），累计值就加一，最后返回累计值
- 用法：count(\*)、count(主键)、count(字段)、count(1)
- count(主键)跟count(\*)一样，因为主键不能为空；count(字段)只计算字段值不为NULL的行；count(1)引擎会为每行添加一个1，然后就count这个1，返回结果也跟count(\*)一样；count(null)返回0

各种用法的性能：

- count(主键)：InnoDB引擎会遍历整张表，把每行的主键id值都取出来，返回给服务层，服务层拿到主键后，直接按行进行累加（主键不可能为空）
- count(字段)：没有not null约束的话，InnoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，服务层判断是否为null，不为null，计数累加；有not null约束的话，InnoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加
- count(1)：InnoDB 引擎遍历整张表，但不取值。服务层对于返回的每一层，放一个数字 1 进去，直接按行进行累加
- count(\*)：InnoDB 引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加

按效率排序：count(字段) < count(主键) < count(1) < count(\*)，所以尽量使用 count(\*)

### update优化（避免行锁升级为表锁）

InnoDB 的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁。

如以下两条语句：
`update student set no = '123' where id = 1;`，这句由于id有主键索引，所以只会锁这一行；
`update student set no = '123' where name = 'test';`，这句由于name没有索引，所以会把整张表都锁住进行数据更新，解决方法是给name字段添加索引

## 视图（View）

视图（View）是一种虚拟存在的表，视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。

简单来说：**视图就是一条 SELECT 语句的结果集，被封装起来方便重复使用。**

### 视图的作用

- **简化查询**：将复杂的多表查询封装成视图，直接查询视图即可
- **安全控制**：通过视图只展示部分字段，隐藏敏感数据
- **数据独立性**：屏蔽表结构变化对上层应用的影响

### 创建视图

语法：
```sql
CREATE [OR REPLACE] VIEW 视图名称 [(列名列表)] AS SELECT语句 [WITH [CASCADED | LOCAL] CHECK OPTION];
```

**详细示例：**

1. **创建简单视图**
```sql
-- 创建一个只显示员工姓名和部门的视图
CREATE VIEW v_employee_dept AS
SELECT e.name, e.age, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;

-- 查询视图（和查询普通表一样）
SELECT * FROM v_employee_dept;
```

2. **带条件的视图**
```sql
-- 创建只显示技术部员工的视图
CREATE VIEW v_tech_employees AS
SELECT id, name, age, salary
FROM employees
WHERE department_id = 1;

-- 查询
SELECT * FROM v_tech_employees;
```

3. **使用 OR REPLACE（视图存在则替换）**
```sql
CREATE OR REPLACE VIEW v_employee_dept AS
SELECT e.id, e.name, e.age, d.dept_name, d.location
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```

4. **带 CHECK OPTION 的视图**
```sql
-- WITH CHECK OPTION 保证通过视图插入/修改的数据必须满足视图的 WHERE 条件
CREATE VIEW v_young_employees AS
SELECT id, name, age
FROM employees
WHERE age < 30
WITH CHECK OPTION;

-- 这条插入会失败，因为 age=35 不满足 age < 30 的条件
INSERT INTO v_young_employees (name, age) VALUES ('测试', 35);
-- ERROR 1369 (HY000): CHECK OPTION failed

-- 这条插入会成功
INSERT INTO v_young_employees (name, age) VALUES ('小王', 25);
```

### 查询视图

```sql
-- 查看所有视图（在 information_schema 中）
SELECT TABLE_NAME FROM information_schema.VIEWS 
WHERE TABLE_SCHEMA = 'mycompany';

-- 查看视图定义
SHOW CREATE VIEW v_employee_dept;

-- 查看视图结构
DESC v_employee_dept;
```

### 修改视图

```sql
-- 方法1：使用 CREATE OR REPLACE
CREATE OR REPLACE VIEW v_employee_dept AS
SELECT e.name, e.age, e.salary, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;

-- 方法2：使用 ALTER VIEW
ALTER VIEW v_employee_dept AS
SELECT e.name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```

### 删除视图

```sql
DROP VIEW [IF EXISTS] 视图名称 [, 视图名称]...;

-- 示例
DROP VIEW v_tech_employees;
DROP VIEW IF EXISTS v_young_employees, v_employee_dept;
```

### 更新视图数据

视图本质上是虚拟表，在一定条件下可以通过视图更新底层表数据。

```sql
-- 简单视图可以直接更新
UPDATE v_young_employees SET age = 26 WHERE name = '小王';

-- 插入数据（需要视图包含所有 NOT NULL 字段）
INSERT INTO v_young_employees (name, age) VALUES ('小李', 24);

-- 删除数据
DELETE FROM v_young_employees WHERE name = '小王';
```

**不可更新的视图：**
- 包含聚合函数（SUM、COUNT、AVG 等）
- 包含 DISTINCT、GROUP BY、HAVING
- 包含 UNION 或 UNION ALL
- 包含子查询
- 包含连接查询（部分情况下）

### Navicat 中操作视图

1. **创建视图**：右键点击数据库下的"视图" → "新建视图" → 编写 SELECT 语句 → 保存
2. **查看视图**：双击视图名查看数据
3. **修改视图**：右键视图 → "设计视图"
4. **删除视图**：右键视图 → "删除视图"

**执行示例：**
```sql
mysql> CREATE VIEW v_emp_count AS
    -> SELECT d.dept_name, COUNT(e.id) AS emp_count
    -> FROM departments d
    -> LEFT JOIN employees e ON d.id = e.department_id
    -> GROUP BY d.dept_name;
Query OK, 0 rows affected (0.02 sec)

mysql> SELECT * FROM v_emp_count;
+----------+-----------+
| dept_name| emp_count |
+----------+-----------+
| 技术部   |         2 |
| 市场部   |         1 |
| 人事部   |         0 |
+----------+-----------+
3 rows in set (0.00 sec)
```

## 变量与流程控制

### 变量

MySQL 中的变量分为三类：**系统变量**、**用户变量**、**局部变量**。

#### 系统变量

由 MySQL 服务器提供，分为全局变量（GLOBAL）和会话变量（SESSION）。

```sql
-- 查看所有系统变量
SHOW GLOBAL VARIABLES;
SHOW SESSION VARIABLES;

-- 查看指定系统变量
SHOW VARIABLES LIKE 'character%';
SELECT @@global.character_set_server;
SELECT @@session.character_set_client;
SELECT @@character_set_client;  -- 不指定则优先会话变量

-- 设置系统变量
SET GLOBAL max_connections = 1000;
SET SESSION sort_buffer_size = 1024 * 1024;
SET @@global.max_connections = 1000;
```

#### 用户变量

由用户自定义，以 `@` 开头，当前连接有效。

```sql
-- 定义并赋值
SET @name = '张三';
SET @age := 25;

-- 通过 SELECT 赋值
SELECT COUNT(*) INTO @emp_count FROM employees;

-- 使用
SELECT @name, @age;
SELECT * FROM employees WHERE age > @age;
```

#### 局部变量

用 `DECLARE` 声明，只能在存储过程/函数的 BEGIN...END 块中使用。

```sql
DECLARE total INT DEFAULT 0;
DECLARE name VARCHAR(50);
DECLARE salary DECIMAL(10,2);

-- 赋值
SET total = 100;
SELECT COUNT(*) INTO total FROM employees;
```

### 流程控制语句

流程控制语句用于存储过程、函数、触发器中。

#### IF 语句

```sql
IF 条件1 THEN
    语句1;
ELSEIF 条件2 THEN
    语句2;
ELSE
    语句3;
END IF;
```

**示例：**
```sql
DELIMITER //
CREATE PROCEDURE p_age_level(IN emp_age INT)
BEGIN
    DECLARE level VARCHAR(10);
    IF emp_age < 25 THEN
        SET level = '青年';
    ELSEIF emp_age < 40 THEN
        SET level = '中年';
    ELSE
        SET level = '老年';
    END IF;
    SELECT level;
END //
DELIMITER ;

-- 调用
CALL p_age_level(28);  -- 中年
CALL p_age_level(18);  -- 青年
```

#### CASE 语句

```sql
-- 语法1：搜索型 CASE
CASE
    WHEN 条件1 THEN 语句1;
    WHEN 条件2 THEN 语句2;
    ELSE 语句3;
END CASE;

-- 语法2：简单型 CASE
CASE 表达式
    WHEN 值1 THEN 语句1;
    WHEN 值2 THEN 语句2;
    ELSE 语句3;
END CASE;
```

**示例：**
```sql
DELIMITER //
CREATE PROCEDURE p_grade_level(IN score INT)
BEGIN
    DECLARE grade VARCHAR(10);
    CASE
        WHEN score >= 90 THEN SET grade = '优秀';
        WHEN score >= 80 THEN SET grade = '良好';
        WHEN score >= 60 THEN SET grade = '及格';
        ELSE SET grade = '不及格';
    END CASE;
    SELECT grade;
END //
DELIMITER ;

CALL p_grade_level(85);  -- 良好
```

#### WHILE 循环

```sql
WHILE 条件 DO
    循环体;
END WHILE;
```

**示例：计算1到100的和**
```sql
DELIMITER //
CREATE PROCEDURE p_sum(IN n INT)
BEGIN
    DECLARE total INT DEFAULT 0;
    DECLARE i INT DEFAULT 1;
    WHILE i <= n DO
        SET total = total + i;
        SET i = i + 1;
    END WHILE;
    SELECT total;
END //
DELIMITER ;

CALL p_sum(100);  -- 5050
```

#### LOOP 循环

```sql
loop_label: LOOP
    循环体;
    IF 条件 THEN
        LEAVE loop_label;  -- 退出循环
    END IF;
END LOOP;
```

**示例：**
```sql
DELIMITER //
CREATE PROCEDURE p_loop_demo()
BEGIN
    DECLARE i INT DEFAULT 0;
    loop1: LOOP
        SET i = i + 1;
        IF i >= 5 THEN
            LEAVE loop1;
        END IF;
    END LOOP;
    SELECT i;  -- 5
END //
DELIMITER ;
```

#### REPEAT 循环

```sql
REPEAT
    循环体;
UNTIL 条件 END REPEAT;
```

**示例：**
```sql
DELIMITER //
CREATE PROCEDURE p_repeat_demo()
BEGIN
    DECLARE i INT DEFAULT 0;
    REPEAT
        SET i = i + 1;
    UNTIL i >= 5 END REPEAT;
    SELECT i;  -- 5
END //
DELIMITER ;
```

#### LEAVE 和 ITERATE

- `LEAVE`：跳出整个循环（类似 break）
- `ITERATE`：跳过本次循环，进入下一次（类似 continue）

```sql
DELIMITER //
CREATE PROCEDURE p_iterate_demo()
BEGIN
    DECLARE i INT DEFAULT 0;
    DECLARE sum_even INT DEFAULT 0;
    loop1: LOOP
        SET i = i + 1;
        IF i > 10 THEN
            LEAVE loop1;
        END IF;
        IF i % 2 = 1 THEN
            ITERATE loop1;  -- 奇数跳过
        END IF;
        SET sum_even = sum_even + i;
    END LOOP;
    SELECT sum_even;  -- 2+4+6+8+10 = 30
END //
DELIMITER ;
```

### DELIMITER 说明

在存储过程/函数中，SQL 语句以 `;` 结尾，但存储过程体内有多个 `;`。为了让 MySQL 不把每个 `;` 当作结束符，需要先用 `DELIMITER` 临时修改结束符，定义完后再改回来。

```sql
DELIMITER //   -- 将结束符改为 //
CREATE PROCEDURE ...
BEGIN
    ...
    ...;       -- 存储过程内的 ; 不会结束整个语句
END //
DELIMITER ;    -- 改回 ;
```

## 存储过程与存储函数

存储过程和存储函数是事先经过编译并存储在数据库中的一段 SQL 语句的集合。

### 存储过程 vs 存储函数

| 特性 | 存储过程 | 存储函数 |
|------|---------|---------|
| 返回值 | 可有多个输出参数 | 必须有一个返回值 |
| 调用方式 | CALL 调用 | 在 SELECT 中调用 |
| 适用场景 | 复杂业务逻辑 | 计算并返回一个值 |

### 创建存储过程

语法：
```sql
CREATE PROCEDURE 存储过程名([IN|OUT|INOUT] 参数名 数据类型)
BEGIN
    SQL语句;
END;
```

**参数类型：**
- `IN`：输入参数（默认），调用时传入，在过程内只读
- `OUT`：输出参数，过程内赋值，调用后可获取
- `INOUT`：既是输入也是输出

**详细示例：**

1. **无参数的存储过程**
```sql
DELIMITER //
CREATE PROCEDURE p_get_all_employees()
BEGIN
    SELECT * FROM employees;
END //
DELIMITER ;

-- 调用
CALL p_get_all_employees();
```

2. **IN 输入参数**
```sql
DELIMITER //
CREATE PROCEDURE p_get_emp_by_dept(IN dept_id INT)
BEGIN
    SELECT * FROM employees WHERE department_id = dept_id;
END //
DELIMITER ;

-- 调用
CALL p_get_emp_by_dept(1);
```

3. **OUT 输出参数**
```sql
DELIMITER //
CREATE PROCEDURE p_get_emp_count(OUT total INT)
BEGIN
    SELECT COUNT(*) INTO total FROM employees;
END //
DELIMITER ;

-- 调用（需要用用户变量接收）
CALL p_get_emp_count(@count);
SELECT @count;
```

4. **INOUT 参数**
```sql
DELIMITER //
CREATE PROCEDURE p_double(INOUT num INT)
BEGIN
    SET num = num * 2;
END //
DELIMITER ;

-- 调用
SET @n = 10;
CALL p_double(@n);
SELECT @n;  -- 20
```

5. **综合案例：转账**
```sql
DELIMITER //
CREATE PROCEDURE p_transfer(IN from_id INT, IN to_id INT, IN amount DECIMAL(10,2))
BEGIN
    DECLARE from_balance DECIMAL(10,2);
    
    -- 检查转出账户余额
    SELECT balance INTO from_balance FROM account WHERE id = from_id;
    
    IF from_balance < amount THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = '余额不足';
    END IF;
    
    -- 开始转账
    UPDATE account SET balance = balance - amount WHERE id = from_id;
    UPDATE account SET balance = balance + amount WHERE id = to_id;
    
    SELECT '转账成功' AS result;
END //
DELIMITER ;

-- 调用
CALL p_transfer(1, 2, 500);
```

### 查看存储过程

```sql
-- 查看所有存储过程
SHOW PROCEDURE STATUS WHERE Db = 'mycompany';

-- 查看存储过程定义
SHOW CREATE PROCEDURE p_get_emp_count;
```

### 删除存储过程

```sql
DROP PROCEDURE [IF EXISTS] 存储过程名;

DROP PROCEDURE p_get_emp_count;
```

### 创建存储函数

语法：
```sql
CREATE FUNCTION 函数名([参数名 数据类型, ...])
RETURNS 返回值类型
[DETERMINISTIC]
BEGIN
    SQL语句;
    RETURN 值;
END;
```

**详细示例：**

1. **简单函数：返回员工总数**
```sql
DELIMITER //
CREATE FUNCTION f_emp_count()
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE total INT;
    SELECT COUNT(*) INTO total FROM employees;
    RETURN total;
END //
DELIMITER ;

-- 调用（在 SELECT 中使用）
SELECT f_emp_count();
```

2. **带参数的函数：计算年薪**
```sql
DELIMITER //
CREATE FUNCTION f_annual_salary(monthly_salary DECIMAL(10,2))
RETURNS DECIMAL(12,2)
DETERMINISTIC
BEGIN
    RETURN monthly_salary * 12;
END //
DELIMITER ;

-- 调用
SELECT name, salary, f_annual_salary(salary) AS annual_salary FROM employees;
```

3. **根据员工ID获取部门名称**
```sql
DELIMITER //
CREATE FUNCTION f_get_dept_name(emp_id INT)
RETURNS VARCHAR(50)
DETERMINISTIC
BEGIN
    DECLARE dept_name VARCHAR(50);
    SELECT d.dept_name INTO dept_name
    FROM employees e
    LEFT JOIN departments d ON e.department_id = d.id
    WHERE e.id = emp_id;
    RETURN dept_name;
END //
DELIMITER ;

-- 调用
SELECT name, f_get_dept_name(id) AS dept FROM employees;
```

### 查看和删除存储函数

```sql
-- 查看
SHOW FUNCTION STATUS WHERE Db = 'mycompany';
SHOW CREATE FUNCTION f_emp_count;

-- 删除
DROP FUNCTION f_emp_count;
```

### Navicat 中操作存储过程/函数

1. **创建**：右键点击"函数" → "新建函数" → 选择"过程"或"函数" → 编写代码 → 保存
2. **运行**：双击函数名，或在查询中用 `CALL` 调用
3. **删除**：右键函数 → "删除函数"

## 触发器（Trigger）

触发器是与表有关的数据库对象，在满足定义条件时触发，并执行触发器中定义的 SQL 语句集合。

**触发时机：**
- `BEFORE`：在操作之前触发
- `AFTER`：在操作之后触发

**触发事件：**
- `INSERT`
- `UPDATE`
- `DELETE`

### 创建触发器

语法：
```sql
CREATE TRIGGER 触发器名
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON 表名
FOR EACH ROW
BEGIN
    SQL语句;
END;
```

**NEW 和 OLD：**
- `NEW`：新数据（INSERT 后、UPDATE 后的值）
- `OLD`：旧数据（DELETE 前、UPDATE 前的值）
- INSERT：只有 NEW
- DELETE：只有 OLD
- UPDATE：既有 NEW 也有 OLD

**详细示例：**

1. **插入后记录日志**
```sql
-- 创建日志表
CREATE TABLE employee_log (
    id INT PRIMARY KEY AUTO_INCREMENT,
    operation VARCHAR(10) COMMENT '操作类型',
    emp_id INT COMMENT '员工ID',
    emp_name VARCHAR(50) COMMENT '员工姓名',
    operate_time DATETIME COMMENT '操作时间'
);

DELIMITER //
CREATE TRIGGER tr_emp_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (operation, emp_id, emp_name, operate_time)
    VALUES ('INSERT', NEW.id, NEW.name, NOW());
END //
DELIMITER ;

-- 测试：插入员工会自动记录日志
INSERT INTO employees (name, age) VALUES ('测试员工', 30);
SELECT * FROM employee_log;
```

2. **删除前备份数据**
```sql
-- 创建备份表
CREATE TABLE employees_backup LIKE employees;

DELIMITER //
CREATE TRIGGER tr_emp_delete
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employees_backup 
    SELECT * FROM employees WHERE id = OLD.id;
END //
DELIMITER ;

-- 测试：删除员工会自动备份
DELETE FROM employees WHERE id = 1;
SELECT * FROM employees_backup;
```

3. **更新时记录修改前后的值**
```sql
DELIMITER //
CREATE TRIGGER tr_emp_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    IF OLD.name != NEW.name THEN
        INSERT INTO employee_log (operation, emp_id, emp_name, operate_time)
        VALUES ('UPDATE', NEW.id, CONCAT(OLD.name, '->', NEW.name), NOW());
    END IF;
END //
DELIMITER ;

-- 测试
UPDATE employees SET name = '张三改' WHERE id = 2;
SELECT * FROM employee_log;
```

4. **校验数据（BEFORE INSERT）**
```sql
DELIMITER //
CREATE TRIGGER tr_emp_check_age
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF NEW.age < 18 OR NEW.age > 65 THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = '年龄必须在18到65之间';
    END IF;
END //
DELIMITER ;

-- 测试
INSERT INTO employees (name, age) VALUES ('童工', 15);
-- ERROR 1644 (45000): 年龄必须在18到65之间
```

### 查看触发器

```sql
-- 查看所有触发器
SHOW TRIGGERS;

-- 查看指定表的触发器
SHOW TRIGGERS LIKE 'employees';

-- 查看触发器定义
SHOW CREATE TRIGGER tr_emp_insert;
```

### 删除触发器

```sql
DROP TRIGGER [IF EXISTS] 触发器名;

DROP TRIGGER tr_emp_insert;
```

### 触发器注意事项

- 触发器不能调用存储过程
- 触发器中不能使用 START TRANSACTION、COMMIT、ROLLBACK
- 触发器执行失败会导致触发它的 SQL 语句失败
- 不要在触发器中写太复杂的逻辑，影响性能

## 事件（Event）

事件调度器（Event Scheduler）是 MySQL 5.1 引入的，用于定时执行 SQL 语句或存储过程，类似于 Linux 的 crontab。

### 开启事件调度器

```sql
-- 查看事件调度器状态
SHOW VARIABLES LIKE 'event_scheduler';

-- 开启事件调度器（当前会话）
SET GLOBAL event_scheduler = ON;

-- 永久开启，在 my.ini 中添加
-- [mysqld]
-- event_scheduler = ON
```

### 创建事件

语法：
```sql
CREATE EVENT [IF NOT EXISTS] 事件名
ON SCHEDULE schedule
[ON COMPLETION [NOT] PRESERVE]
[ENABLE | DISABLE]
DO
    SQL语句;
```

**schedule 语法：**
```sql
-- 定时执行一次
AT timestamp [+ INTERVAL interval]

-- 周期性执行
EVERY interval [STARTS timestamp] [ENDS timestamp]
```

**详细示例：**

1. **定时执行一次（3天后清空日志表）**
```sql
CREATE EVENT e_clear_log
ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 3 DAY
DO
    TRUNCATE TABLE employee_log;
```

2. **周期性执行（每天凌晨2点统计数据）**
```sql
CREATE EVENT e_daily_stats
ON SCHEDULE EVERY 1 DAY
STARTS '2024-01-01 02:00:00'
DO
    INSERT INTO daily_stats (stat_date, emp_count, avg_salary)
    SELECT CURDATE(), COUNT(*), AVG(salary) FROM employees;
```

3. **每分钟执行一次（测试用）**
```sql
CREATE EVENT e_every_minute
ON SCHEDULE EVERY 1 MINUTE
DO
    INSERT INTO test_log (msg) VALUES (CONCAT('执行时间：', NOW()));
```

4. **每小时执行一次并设置结束时间**
```sql
CREATE EVENT e_hourly_clean
ON SCHEDULE EVERY 1 HOUR
STARTS CURRENT_TIMESTAMP
ENDS CURRENT_TIMESTAMP + INTERVAL 1 MONTH
DO
    DELETE FROM session_log WHERE create_time < DATE_SUB(NOW(), INTERVAL 2 HOUR);
```

### 查看事件

```sql
-- 查看所有事件
SHOW EVENTS;

-- 查看事件定义
SHOW CREATE EVENT e_daily_stats;
```

### 修改事件

```sql
-- 禁用事件
ALTER EVENT e_daily_stats DISABLE;

-- 启用事件
ALTER EVENT e_daily_stats ENABLE;

-- 修改执行计划
ALTER EVENT e_daily_stats
ON SCHEDULE EVERY 2 HOUR;
```

### 删除事件

```sql
DROP EVENT [IF EXISTS] 事件名;

DROP EVENT e_daily_stats;
```

## 游标（Cursor）

游标是用来存储查询结果集的数据类型，在存储过程和函数中可以使用游标对结果集进行逐行处理。

**游标使用步骤：** 声明 → 打开 → 读取 → 关闭

### 游标的使用

语法：
```sql
-- 1. 声明游标
DECLARE 游标名 CURSOR FOR 查询语句;

-- 2. 打开游标
OPEN 游标名;

-- 3. 读取游标数据
FETCH 游标名 INTO 变量1, 变量2, ...;

-- 4. 关闭游标
CLOSE 游标名;
```

**详细示例：计算所有员工的薪资总和并逐个输出**

```sql
DELIMITER //
CREATE PROCEDURE p_calc_total_salary()
BEGIN
    DECLARE emp_name VARCHAR(50);
    DECLARE emp_salary DECIMAL(10,2);
    DECLARE total DECIMAL(12,2) DEFAULT 0;
    DECLARE done INT DEFAULT 0;
    
    -- 声明游标
    DECLARE cur CURSOR FOR 
        SELECT name, salary FROM employees;
    
    -- 声明处理程序：当没有数据时设置 done=1
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;
    
    -- 打开游标
    OPEN cur;
    
    -- 循环读取
    read_loop: LOOP
        FETCH cur INTO emp_name, emp_salary;
        IF done = 1 THEN
            LEAVE read_loop;
        END IF;
        SET total = total + emp_salary;
        SELECT CONCAT(emp_name, '的薪资是：', emp_salary) AS info;
    END LOOP;
    
    -- 关闭游标
    CLOSE cur;
    
    SELECT CONCAT('薪资总和：', total) AS result;
END //
DELIMITER ;

-- 调用
CALL p_calc_total_salary();
```

### 综合案例：给所有员工涨薪

```sql
DELIMITER //
CREATE PROCEDURE p_raise_salary()
BEGIN
    DECLARE emp_id INT;
    DECLARE emp_salary DECIMAL(10,2);
    DECLARE done INT DEFAULT 0;
    
    DECLARE cur CURSOR FOR 
        SELECT id, salary FROM employees;
    
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;
    
    OPEN cur;
    
    raise_loop: LOOP
        FETCH cur INTO emp_id, emp_salary;
        IF done = 1 THEN
            LEAVE raise_loop;
        END IF;
        -- 薪资低于5000的涨20%，其他涨10%
        IF emp_salary < 5000 THEN
            UPDATE employees SET salary = salary * 1.2 WHERE id = emp_id;
        ELSE
            UPDATE employees SET salary = salary * 1.1 WHERE id = emp_id;
        END IF;
    END LOOP;
    
    CLOSE cur;
END //
DELIMITER ;

-- 调用
CALL p_raise_salary();
```

> **注意**：游标是逐行处理，数据量大时性能较差，能用集合操作（UPDATE/SELECT）就不要用游标。

## 数据库设计与三范式

### 数据库设计概述

数据库设计是将业务需求转化为数据库结构的过程，好的设计能减少数据冗余、保证数据一致性、提高查询效率。

**设计步骤：**
1. **需求分析**：了解业务需要存储哪些数据
2. **概念设计**：画 E-R 图（实体-关系图）
3. **逻辑设计**：将 E-R 图转化为表结构
4. **物理设计**：选择存储引擎、设置索引、优化

### 实体与关系

**实体（Entity）**：现实世界中可区分的事物，如员工、部门、商品

**关系（Relationship）**：
- **一对一（1:1）**：一个人对应一个身份证
- **一对多（1:N）**：一个部门有多个员工
- **多对多（M:N）**：一个学生选多门课，一门课多个学生选

**E-R 图符号：**
- 矩形：实体
- 椭圆：属性
- 菱形：关系

### 三大范式

范式是设计关系数据库时需要遵循的规范，级别越高冗余越少。

#### 第一范式（1NF）

**原子性**：每一列都是不可分割的原子数据项。

**反例（不满足1NF）：**
| 员工ID | 姓名 | 联系方式 |
|--------|------|---------|
| 1 | 张三 | 手机:138...,邮箱:zs@xx.com |

**正例（满足1NF）：**
| 员工ID | 姓名 | 手机号 | 邮箱 |
|--------|------|--------|------|
| 1 | 张三 | 138... | zs@xx.com |

#### 第二范式（2NF）

**唯一性**：在满足1NF的基础上，非主键字段必须完全依赖于整个主键（消除部分依赖）。

主要针对**联合主键**的情况。

**反例（不满足2NF）：**
| 订单ID | 商品ID | 商品名称 | 数量 | 单价 |
|--------|--------|---------|------|------|
| 1001 | 10 | 手机 | 2 | 3999 |

联合主键是 (订单ID, 商品ID)，但"商品名称"只依赖"商品ID"，不依赖"订单ID"，属于部分依赖。

**正例（满足2NF，拆表）：**

订单明细表：
| 订单ID | 商品ID | 数量 |
|--------|--------|------|
| 1001 | 10 | 2 |

商品表：
| 商品ID | 商品名称 | 单价 |
|--------|---------|------|
| 10 | 手机 | 3999 |

#### 第三范式（3NF）

**非传递依赖**：在满足2NF的基础上，非主键字段不能传递依赖于主键（即非主键字段之间不能有依赖关系）。

**反例（不满足3NF）：**
| 员工ID | 姓名 | 部门ID | 部门名称 |
|--------|------|--------|---------|
| 1 | 张三 | 1 | 技术部 |

"部门名称"依赖于"部门ID"，而"部门ID"依赖于主键"员工ID"，这就是传递依赖。

**正例（满足3NF，拆表）：**

员工表：
| 员工ID | 姓名 | 部门ID |
|--------|------|--------|
| 1 | 张三 | 1 |

部门表：
| 部门ID | 部门名称 |
|--------|---------|
| 1 | 技术部 |

### 范式总结

| 范式 | 核心要求 | 解决的问题 |
|------|---------|-----------|
| 1NF | 列不可再分 | 消除重复组 |
| 2NF | 完全依赖主键 | 消除部分依赖 |
| 3NF | 不传递依赖主键 | 消除传递依赖 |

> 实际开发中，一般满足 3NF 即可。但有时为了查询性能，会适当违反范式（反范式化），增加冗余字段减少联表查询。

### 数据库设计案例：学生选课系统

```sql
-- 学生表
CREATE TABLE students (
    student_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '学号',
    name VARCHAR(50) NOT NULL COMMENT '姓名',
    gender CHAR(1) COMMENT '性别',
    birthday DATE COMMENT '生日',
    class_id INT COMMENT '班级ID'
) COMMENT '学生表';

-- 课程表
CREATE TABLE courses (
    course_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '课程号',
    course_name VARCHAR(100) NOT NULL COMMENT '课程名',
    credit INT COMMENT '学分',
    teacher_id INT COMMENT '授课教师ID'
) COMMENT '课程表';

-- 选课表（中间表，解决多对多关系）
CREATE TABLE student_courses (
    id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT NOT NULL COMMENT '学号',
    course_id INT NOT NULL COMMENT '课程号',
    score DECIMAL(5,2) COMMENT '成绩',
    enrollment_date DATE COMMENT '选课日期',
    CONSTRAINT fk_sc_student FOREIGN KEY (student_id) REFERENCES students(student_id),
    CONSTRAINT fk_sc_course FOREIGN KEY (course_id) REFERENCES courses(course_id)
) COMMENT '选课表';
```

## 窗口函数（MySQL 8.0+）

窗口函数是 MySQL 8.0 新增的功能，允许在查询中对一组相关行（窗口）进行计算，而不会像 GROUP BY 那样合并行。

语法：
```sql
窗口函数名() OVER (
    [PARTITION BY 分区字段]
    [ORDER BY 排序字段]
    [行范围]
)
```

### 序号函数

#### ROW_NUMBER()

为每一行分配一个唯一序号（1, 2, 3, ...），相同值序号不同。

```sql
-- 按部门分组，按薪资降序编号
SELECT 
    name, 
    department_id, 
    salary,
    ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rn
FROM employees;
```

#### RANK()

相同值序号相同，下一个不同值的序号会跳过（1, 1, 3, ...）。

```sql
SELECT 
    name, 
    department_id, 
    salary,
    RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rk
FROM employees;
```

#### DENSE_RANK()

相同值序号相同，下一个不同值的序号不会跳过（1, 1, 2, ...）。

```sql
SELECT 
    name, 
    department_id, 
    salary,
    DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS dr
FROM employees;
```

### 聚合窗口函数

在窗口上执行聚合，每行都显示聚合结果。

```sql
SELECT 
    name, 
    department_id, 
    salary,
    SUM(salary) OVER (PARTITION BY department_id) AS dept_total,
    AVG(salary) OVER (PARTITION BY department_id) AS dept_avg,
    COUNT(*) OVER (PARTITION BY department_id) AS dept_count
FROM employees;
```

### 分布函数

#### CUME_DIST()

计算某行在窗口中的累计分布值（0~1）。

```sql
SELECT 
    name, 
    salary,
    CUME_DIST() OVER (ORDER BY salary) AS cume_dist
FROM employees;
```

#### PERCENT_RANK()

计算某行的百分比排名（0~1）。

```sql
SELECT 
    name, 
    salary,
    PERCENT_RANK() OVER (ORDER BY salary) AS pr
FROM employees;
```

### 前后函数

#### LAG() 和 LEAD()

- `LAG(col, n, default)`：取前 n 行的值
- `LEAD(col, n, default)`：取后 n 行的值

```sql
-- 查询每个员工和上一个入职员工的薪资差
SELECT 
    name, 
    hire_date, 
    salary,
    LAG(salary, 1, 0) OVER (ORDER BY hire_date) AS prev_salary,
    salary - LAG(salary, 1, 0) OVER (ORDER BY hire_date) AS diff
FROM employees;
```

#### FIRST_VALUE() 和 LAST_VALUE()

取窗口第一行/最后一行的值。

```sql
SELECT 
    name, 
    department_id, 
    salary,
    FIRST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY salary DESC) AS dept_max_salary
FROM employees;
```

### 实际应用：取每个部门薪资前3的员工

```sql
SELECT * FROM (
    SELECT 
        name, 
        department_id, 
        salary,
        ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rn
    FROM employees
) t
WHERE rn <= 3;
```

## 公用表表达式 CTE（MySQL 8.0+）

CTE（Common Table Expression）是一个命名的临时结果集，可以在 SELECT、INSERT、UPDATE、DELETE 中使用。

### 普通 CTE

语法：
```sql
WITH cte_name AS (
    SELECT语句
)
SELECT * FROM cte_name;
```

**示例：**
```sql
-- 查询薪资高于平均薪资的员工
WITH avg_salary AS (
    SELECT AVG(salary) AS avg_sal FROM employees
)
SELECT e.name, e.salary
FROM employees e, avg_salary a
WHERE e.salary > a.avg_sal;
```

### 多个 CTE

```sql
WITH 
dept_avg AS (
    SELECT department_id, AVG(salary) AS avg_sal 
    FROM employees 
    GROUP BY department_id
),
dept_info AS (
    SELECT id, dept_name FROM departments
)
SELECT d.dept_name, da.avg_sal
FROM dept_avg da
JOIN dept_info d ON da.department_id = d.id;
```

### 递归 CTE

递归 CTE 可以引用自身，用于处理树形结构数据。

语法：
```sql
WITH RECURSIVE cte_name AS (
    -- 初始查询（锚点成员）
    SELECT ...
    UNION ALL
    -- 递归查询（递归成员）
    SELECT ... FROM cte_name WHERE ...
)
SELECT * FROM cte_name;
```

**示例：查询组织架构（员工-领导层级）**

```sql
-- 数据准备：员工表有 manager_id 字段表示上级
WITH RECURSIVE emp_hierarchy AS (
    -- 锚点：查最高领导（没有上级的）
    SELECT id, name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- 递归：查下属
    SELECT e.id, e.name, e.manager_id, eh.level + 1 AS level
    FROM employees e
    INNER JOIN emp_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM emp_hierarchy ORDER BY level, id;
```

**示例：生成数字序列 1 到 10**

```sql
WITH RECURSIVE seq(n) AS (
    SELECT 1
    UNION ALL
    SELECT n + 1 FROM seq WHERE n < 10
)
SELECT n FROM seq;
```

### CTE vs 子查询 vs 临时表

| 特性 | CTE | 子查询 | 临时表 |
|------|-----|--------|--------|
| 可读性 | 高 | 低 | 中 |
| 可复用 | 同语句内可多次引用 | 不能复用 | 整个会话可复用 |
| 递归 | 支持 | 不支持 | 不支持 |
| 适用场景 | 结构清晰、递归查询 | 简单嵌套 | 复杂计算、多次使用 |

## JSON 类型操作详解

MySQL 5.7 引入 JSON 类型，8.0 进一步增强。JSON 类型可以存储结构化的动态数据。

### 创建表使用 JSON 类型

```sql
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    attributes JSON COMMENT '商品属性',
    tags JSON COMMENT '标签列表'
);
```

### 插入 JSON 数据

```sql
-- 插入对象格式
INSERT INTO products (name, attributes, tags) VALUES
('手机', '{"color": "黑色", "storage": "128GB", "price": 3999}', '["电子", "通讯"]');

-- 插入数组格式
INSERT INTO products (name, attributes, tags) VALUES
('笔记本', '{"color": "银色", "storage": "512GB", "price": 6999}', '["电子", "办公"]');
```

### 查询 JSON 数据

#### 使用 `->` 操作符

```sql
-- 查询商品的颜色
SELECT name, attributes->'$.color' AS color FROM products;

-- 查询价格大于5000的商品
SELECT * FROM products WHERE CAST(attributes->'$.price' AS UNSIGNED) > 5000;
```

#### 使用 `->>` 操作符（去除引号）

```sql
-- -> 返回带引号的字符串，->> 返回不带引号的
SELECT attributes->'$.color' AS color_with_quotes,
       attributes->>'$.color' AS color_no_quotes
FROM products;
```

#### 使用 JSON_EXTRACT() 函数

```sql
SELECT 
    name,
    JSON_EXTRACT(attributes, '$.color') AS color,
    JSON_EXTRACT(attributes, '$.storage') AS storage
FROM products;
```

#### 查询数组元素

```sql
-- 查询第一个标签
SELECT name, tags->'$[0]' AS first_tag FROM products;

-- 查询所有包含"电子"标签的商品
SELECT * FROM products WHERE JSON_CONTAINS(tags, '"电子"');
```

### 修改 JSON 数据

#### JSON_SET()：设置值（不存在则添加，存在则修改）

```sql
UPDATE products 
SET attributes = JSON_SET(attributes, '$.price', 4299)
WHERE id = 1;
```

#### JSON_INSERT()：插入值（只在不存在时插入）

```sql
UPDATE products 
SET attributes = JSON_INSERT(attributes, '$.weight', '180g')
WHERE id = 1;
```

#### JSON_REPLACE()：替换值（只替换已存在的）

```sql
UPDATE products 
SET attributes = JSON_REPLACE(attributes, '$.color', '白色')
WHERE id = 1;
```

#### JSON_REMOVE()：删除键

```sql
UPDATE products 
SET attributes = JSON_REMOVE(attributes, '$.weight')
WHERE id = 1;
```

### JSON 常用函数

| 函数 | 说明 |
|------|------|
| JSON_TYPE() | 返回 JSON 类型 |
| JSON_VALID() | 验证是否为有效 JSON |
| JSON_LENGTH() | 返回 JSON 元素数量 |
| JSON_KEYS() | 返回 JSON 对象的所有键 |
| JSON_CONTAINS() | 检查是否包含指定值 |
| JSON_CONTAINS_PATH() | 检查是否包含指定路径 |
| JSON_SEARCH() | 搜索指定值并返回路径 |
| JSON_ARRAY() | 创建 JSON 数组 |
| JSON_OBJECT() | 创建 JSON 对象 |
| JSON_MERGE() / JSON_MERGE_PRESERVE() | 合并 JSON |

**示例：**
```sql
-- 返回键列表
SELECT name, JSON_KEYS(attributes) AS keys_list FROM products;

-- 验证 JSON 有效性
SELECT JSON_VALID(attributes) FROM products;

-- 返回元素数量
SELECT name, JSON_LENGTH(tags) AS tag_count FROM products;

-- 返回数据类型
SELECT JSON_TYPE(attributes), JSON_TYPE(tags) FROM products;
```

## 锁机制

锁是计算机协调多个进程或线程并发访问某一资源的机制。在数据库中，除传统的计算资源（CPU、RAM、I/O）的争用以外，数据也是一种供多用户共享的资源。

### 锁的分类

**按操作粒度分：**
- **全局锁**：锁住整个数据库实例
- **表级锁**：锁住整张表
- **行级锁**：锁住某一行（InnoDB）

**按操作类型分：**
- **读锁（共享锁 S）**：多个事务可以同时加读锁，互相不阻塞
- **写锁（排他锁 X）**：一个事务加写锁后，其他事务不能加任何锁

### 全局锁

锁住整个数据库实例，使其处于只读状态。

```sql
-- 加全局锁
FLUSH TABLES WITH READ LOCK;

-- 释放全局锁
UNLOCK TABLES;
```

**使用场景**：全库备份时，保证数据一致性。

### 表级锁

锁住整张表，开销小、加锁快，但并发度低。MyISAM 只支持表级锁。

#### 表锁

```sql
-- 加表锁（读锁或写锁）
LOCK TABLES employees READ;      -- 读锁
LOCK TABLES employees WRITE;     -- 写锁

-- 释放表锁
UNLOCK TABLES;
```

**读锁**：其他会话可以读，但不能写
**写锁**：其他会话既不能读也不能写

#### 元数据锁（MDL）

MDL 是 MySQL 5.5 引入的，访问表时自动加，不需要显式操作。
- 执行 DML（SELECT、INSERT、UPDATE、DELETE）时自动加 MDL 读锁
- 执行 DDL（ALTER TABLE 等）时自动加 MDL 写锁

### 行级锁（InnoDB）

InnoDB 支持行级锁，粒度细、并发高，但开销大。

#### 加锁方式

```sql
-- 共享锁（S锁）：其他事务可以读，不能写
SELECT * FROM employees WHERE id = 1 LOCK IN SHARE MODE;

-- 排他锁（X锁）：其他事务既不能读也不能写
SELECT * FROM employees WHERE id = 1 FOR UPDATE;

-- UPDATE、DELETE 自动加排他锁
UPDATE employees SET age = 30 WHERE id = 1;
```

#### 行锁的注意事项

1. **行锁是基于索引的**：如果查询没有用到索引，会升级为表锁
2. **间隙锁（Gap Lock）**：锁定索引记录之间的间隙，防止幻读
3. **临键锁（Next-Key Lock）**：行锁 + 间隙锁，InnoDB 默认锁算法

**示例：行锁升级为表锁**

```sql
-- name 字段没有索引
-- 事务A执行：
UPDATE employees SET salary = salary * 1.1 WHERE name = '张三';

-- 事务B执行（会被阻塞，因为A的行锁升级为表锁）：
UPDATE employees SET salary = salary * 1.1 WHERE name = '李四';
```

**解决方法**：给 name 字段加索引。

### 死锁

两个或多个事务互相等待对方释放锁，导致都无法继续执行。

**示例：**

```
事务A：
BEGIN;
UPDATE employees SET salary = 5000 WHERE id = 1;  -- 锁住id=1
-- 此时事务B锁住了id=2
UPDATE employees SET salary = 6000 WHERE id = 2;  -- 等待事务B释放
COMMIT;

事务B：
BEGIN;
UPDATE employees SET salary = 7000 WHERE id = 2;  -- 锁住id=2
-- 此时事务A锁住了id=1
UPDATE employees SET salary = 8000 WHERE id = 1;  -- 等待事务A释放
COMMIT;
-- 形成死锁！
```

**死锁处理：**
- MySQL 自动检测死锁，回滚代价较小的事务
- 可以通过 `SHOW ENGINE INNODB STATUS` 查看最近的死锁信息

**避免死锁的方法：**
1. 尽量使用相同的顺序访问数据
2. 缩短事务，减少锁持有时间
3. 降低隔离级别
4. 合理使用索引，避免全表扫描

# 数据类型

## 整型

| 类型名称      | 取值范围                                  | 大小    |
| ------------- | ----------------------------------------- | ------- |
| TINYINT       | -128〜127                                 | 1个字节 |
| SMALLINT      | -32768〜32767                             | 2个宇节 |
| MEDIUMINT     | -8388608〜8388607                         | 3个字节 |
| INT (INTEGHR) | -2147483648〜2147483647                   | 4个字节 |
| BIGINT        | -9223372036854775808〜9223372036854775807 | 8个字节 |

无符号在数据类型后加 unsigned 关键字。

## 浮点型

| 类型名称            | 说明               | 存储需求   |
| ------------------- | ------------------ | ---------- |
| FLOAT               | 单精度浮点数       | 4 个字节   |
| DOUBLE              | 双精度浮点数       | 8 个字节   |
| DECIMAL (M, D)，DEC | 压缩的“严格”定点数 | M+2 个字节 |

## 日期和时间

| 类型名称  | 日期格式            | 日期范围                                          | 存储需求 |
| --------- | ------------------- | ------------------------------------------------- | -------- |
| YEAR      | YYYY                | 1901 ~ 2155                                       | 1 个字节 |
| TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                            | 3 个字节 |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-3                            | 3 个字节 |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59         | 8 个字节 |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:01 UTC ~ 2040-01-19 03:14:07 UTC | 4 个字节 |

## 字符串

| 类型名称   | 说明                                         | 存储需求                                                   |
| ---------- | -------------------------------------------- | ---------------------------------------------------------- |
| CHAR(M)    | 固定长度非二进制字符串                       | M 字节，1<=M<=255                                          |
| VARCHAR(M) | 变长非二进制字符串                           | L+1字节，在此，L< = M和 1<=M<=255                          |
| TINYTEXT   | 非常小的非二进制字符串                       | L+1字节，在此，L<2^8                                       |
| TEXT       | 小的非二进制字符串                           | L+2字节，在此，L<2^16                                      |
| MEDIUMTEXT | 中等大小的非二进制字符串                     | L+3字节，在此，L<2^24                                      |
| LONGTEXT   | 大的非二进制字符串                           | L+4字节，在此，L<2^32                                      |
| ENUM       | 枚举类型，只能有一个枚举字符串值             | 1或2个字节，取决于枚举值的数目 (最大值为65535)             |
| SET        | 一个设置，字符串对象可以有零个或 多个SET成员 | 1、2、3、4或8个字节，取决于集合 成员的数量（最多64个成员） |

## 二进制类型

| 类型名称       | 说明                 | 存储需求               |
| -------------- | -------------------- | ---------------------- |
| BIT(M)         | 位字段类型           | 大约 (M+7)/8 字节      |
| BINARY(M)      | 固定长度二进制字符串 | M 字节                 |
| VARBINARY (M)  | 可变长度二进制字符串 | M+1 字节               |
| TINYBLOB (M)   | 非常小的BLOB         | L+1 字节，在此，L<2^8  |
| BLOB (M)       | 小 BLOB              | L+2 字节，在此，L<2^16 |
| MEDIUMBLOB (M) | 中等大小的BLOB       | L+3 字节，在此，L<2^24 |
| LONGBLOB (M)   | 非常大的BLOB         | L+4 字节，在此，L<2^32 |

# 权限一览表

> 具体权限的作用详见[官方文档](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html "官方文档")

GRANT 和 REVOKE 允许的静态权限

| Privilege                                                    | Grant Table Column           | Context                               |
| :----------------------------------------------------------- | :--------------------------- | :------------------------------------ |
| [`ALL [PRIVILEGES]`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_all) | Synonym for “all privileges” | Server administration                 |
| [`ALTER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_alter) | `Alter_priv`                 | Tables                                |
| [`ALTER ROUTINE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_alter-routine) | `Alter_routine_priv`         | Stored routines                       |
| [`CREATE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create) | `Create_priv`                | Databases, tables, or indexes         |
| [`CREATE ROLE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-role) | `Create_role_priv`           | Server administration                 |
| [`CREATE ROUTINE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-routine) | `Create_routine_priv`        | Stored routines                       |
| [`CREATE TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-tablespace) | `Create_tablespace_priv`     | Server administration                 |
| [`CREATE TEMPORARY TABLES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-temporary-tables) | `Create_tmp_table_priv`      | Tables                                |
| [`CREATE USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-user) | `Create_user_priv`           | Server administration                 |
| [`CREATE VIEW`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-view) | `Create_view_priv`           | Views                                 |
| [`DELETE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_delete) | `Delete_priv`                | Tables                                |
| [`DROP`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_drop) | `Drop_priv`                  | Databases, tables, or views           |
| [`DROP ROLE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_drop-role) | `Drop_role_priv`             | Server administration                 |
| [`EVENT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_event) | `Event_priv`                 | Databases                             |
| [`EXECUTE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_execute) | `Execute_priv`               | Stored routines                       |
| [`FILE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_file) | `File_priv`                  | File access on server host            |
| [`GRANT OPTION`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_grant-option) | `Grant_priv`                 | Databases, tables, or stored routines |
| [`INDEX`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_index) | `Index_priv`                 | Tables                                |
| [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_insert) | `Insert_priv`                | Tables or columns                     |
| [`LOCK TABLES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_lock-tables) | `Lock_tables_priv`           | Databases                             |
| [`PROCESS`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_process) | `Process_priv`               | Server administration                 |
| [`PROXY`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_proxy) | See `proxies_priv` table     | Server administration                 |
| [`REFERENCES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_references) | `References_priv`            | Databases or tables                   |
| [`RELOAD`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_reload) | `Reload_priv`                | Server administration                 |
| [`REPLICATION CLIENT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_replication-client) | `Repl_client_priv`           | Server administration                 |
| [`REPLICATION SLAVE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_replication-slave) | `Repl_slave_priv`            | Server administration                 |
| [`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_select) | `Select_priv`                | Tables or columns                     |
| [`SHOW DATABASES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_show-databases) | `Show_db_priv`               | Server administration                 |
| [`SHOW VIEW`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_show-view) | `Show_view_priv`             | Views                                 |
| [`SHUTDOWN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_shutdown) | `Shutdown_priv`              | Server administration                 |
| [`SUPER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_super) | `Super_priv`                 | Server administration                 |
| [`TRIGGER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_trigger) | `Trigger_priv`               | Tables                                |
| [`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_update) | `Update_priv`                | Tables or columns                     |
| [`USAGE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_usage) | Synonym for “no privileges”  | Server administration                 |

GRANT 和 REVOKE 允许的动态权限

| Privilege                                                    | Context                                           |
| :----------------------------------------------------------- | :------------------------------------------------ |
| [`APPLICATION_PASSWORD_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_application-password-admin) | Dual password administration                      |
| [`AUDIT_ABORT_EXEMPT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_audit-abort-exempt) | Allow queries blocked by audit log filter         |
| [`AUDIT_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_audit-admin) | Audit log administration                          |
| [`AUTHENTICATION_POLICY_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_authentication-policy-admin) | Authentication administration                     |
| [`BACKUP_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_backup-admin) | Backup administration                             |
| [`BINLOG_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_binlog-admin) | Backup and Replication administration             |
| [`BINLOG_ENCRYPTION_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_binlog-encryption-admin) | Backup and Replication administration             |
| [`CLONE_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_clone-admin) | Clone administration                              |
| [`CONNECTION_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_connection-admin) | Server administration                             |
| [`ENCRYPTION_KEY_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_encryption-key-admin) | Server administration                             |
| [`FIREWALL_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_firewall-admin) | Firewall administration                           |
| [`FIREWALL_EXEMPT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_firewall-exempt) | Firewall administration                           |
| [`FIREWALL_USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_firewall-user) | Firewall administration                           |
| [`FLUSH_OPTIMIZER_COSTS`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_flush-optimizer-costs) | Server administration                             |
| [`FLUSH_STATUS`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_flush-status) | Server administration                             |
| [`FLUSH_TABLES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_flush-tables) | Server administration                             |
| [`FLUSH_USER_RESOURCES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_flush-user-resources) | Server administration                             |
| [`GROUP_REPLICATION_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_group-replication-admin) | Replication administration                        |
| [`GROUP_REPLICATION_STREAM`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_group-replication-stream) | Replication administration                        |
| [`INNODB_REDO_LOG_ARCHIVE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_innodb-redo-log-archive) | Redo log archiving administration                 |
| [`NDB_STORED_USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_ndb-stored-user) | NDB Cluster                                       |
| [`PASSWORDLESS_USER_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_passwordless-user-admin) | Authentication administration                     |
| [`PERSIST_RO_VARIABLES_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_persist-ro-variables-admin) | Server administration                             |
| [`REPLICATION_APPLIER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_replication-applier) | `PRIVILEGE_CHECKS_USER` for a replication channel |
| [`REPLICATION_SLAVE_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_replication-slave-admin) | Replication administration                        |
| [`RESOURCE_GROUP_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_resource-group-admin) | Resource group administration                     |
| [`RESOURCE_GROUP_USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_resource-group-user) | Resource group administration                     |
| [`ROLE_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_role-admin) | Server administration                             |
| [`SESSION_VARIABLES_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_session-variables-admin) | Server administration                             |
| [`SET_USER_ID`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_set-user-id) | Server administration                             |
| [`SHOW_ROUTINE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_show-routine) | Server administration                             |
| [`SYSTEM_USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_system-user) | Server administration                             |
| [`SYSTEM_VARIABLES_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_system-variables-admin) | Server administration                             |
| [`TABLE_ENCRYPTION_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_table-encryption-admin) | Server administration                             |
| [`VERSION_TOKEN_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_version-token-admin) | Server administration                             |
| [`XA_RECOVER_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_xa-recover-admin) | Server administration                             |

# 图形化界面工具 Navicat 使用详解

> 本教程以 **Navicat for MySQL 16** 为例，所有操作在 MySQL 8.0 环境下验证通过。

## 一、Navicat 简介与安装

### 1.1 Navicat 是什么

Navicat 是一套快速、可靠的数据库管理工具，专为简化数据库管理和降低系统管理成本而设计。它支持 MySQL、MariaDB、Oracle、SQL Server、PostgreSQL 等多种数据库，是 Java 开发人员日常开发中最常用的图形化数据库工具之一。

### 1.2 下载与安装

**步骤 1：下载 Navicat**

- 官方下载地址：https://www.navicat.com/en/download/navicat-for-mysql
- 选择 Windows 版本，下载安装包（约 80MB）
- 官方提供 14 天免费试用版

**步骤 2：安装 Navicat**

1. 双击下载的安装包 `navicat162_mysql_cs_x64.exe`
2. 选择安装语言（简体中文）
3. 点击"下一步"，接受许可协议
4. 选择安装路径（建议默认 `C:\Program Files\PremiumSoft\Navicat for MySQL 16`）
5. 选择开始菜单文件夹
6. 勾选"创建桌面快捷方式"
7. 点击"安装"，等待安装完成
8. 点击"完成"启动 Navicat

### 1.3 首次启动

启动后会弹出激活窗口，可以选择：
- **试用**：点击"试用"按钮，可免费使用 14 天
- **激活**：输入购买的许可证密钥

---

## 二、连接 MySQL 数据库

### 2.1 创建连接

**步骤 1：打开连接窗口**

启动 Navicat 后，点击左上角的 **"连接"** 按钮，选择 **"MySQL..."**，弹出连接配置窗口。

**步骤 2：填写连接信息**

| 配置项 | 说明 | 示例值 |
|--------|------|--------|
| 连接名 | 自定义连接名称，便于识别 | `本地MySQL` |
| 主机 | MySQL 服务器地址 | `localhost` 或 `127.0.0.1` |
| 端口 | MySQL 端口号，默认 3306 | `3306` |
| 用户名 | 数据库用户名 | `root` |
| 密码 | 数据库密码 | 安装时设置的密码 |

**步骤 3：测试连接**

1. 点击左下角的 **"测试连接"** 按钮
2. 如果配置正确，弹出"连接成功"提示框
3. 点击"确定"保存连接

**步骤 4：打开连接**

- 在左侧连接列表中，双击刚创建的连接名（或右键 → 打开连接）
- 连接成功后，左侧会显示该服务器下的所有数据库

> **注意**：如果测试连接失败，请检查：
> 1. MySQL 服务是否已启动（`net start MySQL80`）
> 2. 主机地址和端口是否正确
> 3. 用户名和密码是否正确
> 4. 防火墙是否放行 3306 端口

### 2.2 连接远程数据库

如果连接远程服务器，将"主机"改为远程服务器的 IP 地址即可，但需要确保：
1. MySQL 允许远程连接（用户的 host 为 `%` 或指定 IP）
2. 服务器防火墙开放 3306 端口
3. MySQL 配置文件中 `bind-address` 不是 `127.0.0.1`

---

## 三、数据库操作

### 3.1 创建数据库

**方法一：右键菜单方式（推荐）**

1. 在左侧连接列表中，右键点击连接名（如`本地MySQL`）
2. 选择 **"新建数据库..."**
3. 填写数据库信息：
   - **数据库名**：输入数据库名称，如 `mycompany`
   - **字符集**：选择 `utf8mb4`（支持完整 Unicode，包括 emoji）
   - **排序规则**：选择 `utf8mb4_general_ci` 或 `utf8mb4_unicode_ci`
4. 点击"确定"创建

**方法二：SQL 语句方式**

1. 点击工具栏的 **"查询"** → **"新建查询"**
2. 输入 SQL 语句：
```sql
CREATE DATABASE IF NOT EXISTS mycompany 
DEFAULT CHARACTER SET utf8mb4 
DEFAULT COLLATE utf8mb4_general_ci;
```
3. 点击 **"运行"** 按钮（或按 `F5`）执行

### 3.2 查看数据库

- 左侧连接列表中展开连接，即可看到所有数据库
- 每个数据库名前面的图标表示其状态

### 3.3 修改数据库

1. 右键点击数据库名 → **"编辑数据库..."**
2. 可以修改字符集和排序规则
3. 点击"确定"保存

### 3.4 删除数据库

1. 右键点击数据库名 → **"删除数据库"**
2. 在确认对话框中输入 `DELETE`
3. 点击"确定"

> ⚠️ **危险操作**：删除数据库会永久丢失所有数据，请谨慎操作！

### 3.5 打开/关闭数据库

- **打开数据库**：双击数据库名，展开后可看到"表"、"视图"、"函数"等节点
- **关闭数据库**：右键数据库名 → "关闭数据库"

---

## 四、数据表操作

### 4.1 创建数据表

**步骤 1：打开表设计器**

1. 展开数据库 → 右键点击 **"表"** → **"新建表..."**
2. 进入表设计界面

**步骤 2：设计字段**

在表设计器中，逐行添加字段：

| 列名 | 类型 | 长度 | 不是null | 键 | 默认值 | 注释 |
|------|------|------|----------|-----|--------|------|
| id | int | - | ✅ | ☑主键 | - | 员工ID |
| name | varchar | 50 | ✅ | - | - | 员工姓名 |
| age | int | - | ❌ | - | NULL | 年龄 |
| gender | char | 1 | ❌ | - | '男' | 性别 |
| hire_date | date | - | ❌ | - | NULL | 入职日期 |

**设置主键和自增：**
- 勾选 id 行的"键"列，设置为主键
- 在下方的"自动递增"列勾选 ✅，设置为自增

**步骤 3：设置表属性**

点击表设计器上方的 **"..."** 按钮（或右键 → "表属性"），设置：
- **表名**：`employees`
- **注释**：`员工信息表`
- **引擎**：`InnoDB`（默认）
- **字符集**：`utf8mb4`

**步骤 4：保存表**

1. 点击工具栏的 **"保存"** 按钮（或按 `Ctrl+S`）
2. 输入表名 `employees`
3. 点击"确定"

**方法二：使用 SQL 语句创建表**

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT '员工ID',
    name VARCHAR(50) NOT NULL COMMENT '员工姓名',
    age INT COMMENT '年龄',
    gender CHAR(1) DEFAULT '男' COMMENT '性别',
    hire_date DATE COMMENT '入职日期'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='员工信息表';
```

### 4.2 查看表结构

**方法一：设计视图**
- 右键点击表名 → **"设计表"**，可以查看和修改表结构

**方法二：SQL 语句**
```sql
DESC employees;
SHOW CREATE TABLE employees;
```

### 4.3 修改表结构

1. 右键点击表名 → **"设计表"**
2. 在表设计器中：
   - **添加字段**：点击最后一行下方的空白行，输入新字段信息
   - **修改字段**：直接修改对应行的内容
   - **删除字段**：选中要删除的行，点击工具栏的"删除字段"按钮（红色减号）
3. 点击"保存"（`Ctrl+S`）

### 4.4 删除表

1. 右键点击表名 → **"删除表"**
2. 确认删除

### 4.5 清空表数据

1. 右键点击表名 → **"清空表"**
2. 这会删除表中所有数据，但保留表结构（等同于 `TRUNCATE TABLE`）

### 4.6 复制表

1. 右键点击表名 → **"复制表"**
2. 选择目标数据库
3. 选择复制方式：
   - **结构和数据**：复制表结构和所有数据
   - **仅结构**：只复制表结构
4. 点击"确定"

---

## 五、数据操作（增删改查）

### 5.1 查看表数据

1. 双击表名，或右键点击表名 → **"打开表"**
2. 进入数据浏览界面，可查看所有记录
3. 底部显示当前记录数

### 5.2 插入数据

**方法一：图形界面方式**

1. 打开表后，点击底部的 **"+"** 按钮（或按 `Ctrl+N`）
2. 在空白行中输入数据
3. 点击 **"✓"** 按钮（或按 `Ctrl+S`）保存

**方法二：SQL 语句方式**

1. 点击 **"查询"** → **"新建查询"**
2. 输入 INSERT 语句：
```sql
INSERT INTO employees (name, age, gender, hire_date) 
VALUES ('张三', 25, '男', '2023-01-15');
```
3. 点击"运行"（`F5`）

### 5.3 修改数据

**方法一：图形界面方式**

1. 打开表，直接在单元格中修改数据
2. 点击"保存"（`Ctrl+S`）

**方法二：SQL 语句方式**

```sql
UPDATE employees SET age = 26 WHERE id = 1;
```

### 5.4 删除数据

**方法一：图形界面方式**

1. 打开表，选中要删除的行
2. 点击底部的 **"-"** 按钮删除
3. 点击"保存"

**方法二：SQL 语句方式**

```sql
DELETE FROM employees WHERE id = 1;
```

### 5.5 筛选和排序数据

在数据浏览界面底部：
- **筛选**：点击"筛选"按钮，输入筛选条件（如 `age > 25`）
- **排序**：点击列标题，可按该列升序/降序排序
- **查找**：按 `Ctrl+F` 打开查找对话框

---

## 六、SQL 查询编辑器

### 6.1 打开查询编辑器

- 点击工具栏的 **"查询"** → **"新建查询"**（或按 `Ctrl+Q`）
- 查询编辑器支持语法高亮、自动补全、格式化等功能

### 6.2 编写和执行 SQL

1. 在编辑器中输入 SQL 语句
2. 执行方式：
   - **运行当前语句**：将光标放在语句上，按 `Ctrl+Shift+R`
   - **运行所有语句**：点击"运行"按钮或按 `F5`
   - **运行选中语句**：选中部分语句，按 `F5`

### 6.3 查看查询结果

- 查询结果显示在下方的结果面板中
- 多个 SELECT 语句会显示多个结果标签页
- 结果可以导出为 Excel、CSV、JSON 等格式

### 6.4 SQL 自动补全

- 输入表名或字段名的前几个字母，按 `Esc` 或等待自动弹出提示
- 支持智能提示，包括数据库对象、关键字、函数等

### 6.5 SQL 格式化

- 选中 SQL 语句，右键 → **"美化 SQL"**（或按 `Ctrl+B`）
- 自动格式化 SQL，提高可读性

---

## 七、导入导出数据

### 7.1 导出数据库

**导出整个数据库为 SQL 文件：**

1. 右键点击数据库名 → **"转储 SQL 文件"** → **"结构和数据..."**
2. 选择保存路径和文件名
3. 选择导出选项：
   - **导出表结构**：勾选
   - **导出数据**：勾选
   - **使用完整插入语句**：勾选（兼容性更好）
4. 点击"开始"导出

**导出单张表：**

1. 右键点击表名 → **"导出向导"**
2. 选择导出格式（SQL、Excel、CSV、JSON、XML 等）
3. 选择要导出的表
4. 配置导出选项
5. 点击"开始"

### 7.2 导入数据库

**从 SQL 文件导入：**

1. 右键点击目标数据库 → **"运行 SQL 文件..."**
2. 选择 SQL 文件
3. 配置导入选项：
   - **字符集**：选择 `utf8mb4`
   - **遇到错误继续**：按需勾选
4. 点击"开始"
5. 导入完成后点击"关闭"

**通过导入向导导入：**

1. 右键点击数据库或表 → **"导入向导"**
2. 选择导入文件格式和文件
3. 配置字段映射
4. 点击"开始"

---

## 八、用户与权限管理

### 8.1 查看用户

1. 展开 `mysql` 数据库 → 点击 `user` 表
2. 可以查看所有用户信息

### 8.2 创建用户

在查询编辑器中执行：

```sql
-- 创建本地用户
CREATE USER 'developer'@'localhost' IDENTIFIED BY 'Dev@123456';

-- 创建允许远程访问的用户
CREATE USER 'developer'@'%' IDENTIFIED BY 'Dev@123456';

-- 授予权限
GRANT ALL PRIVILEGES ON mycompany.* TO 'developer'@'localhost';

-- 刷新权限
FLUSH PRIVILEGES;
```

### 8.3 使用新用户连接

1. 点击"连接" → "MySQL..."
2. 填写新用户的用户名和密码
3. 测试连接成功后保存

---

## 九、ER 图（实体关系图）

### 9.1 查看 ER 图

1. 右键点击数据库 → **"逆向表到模型..."**
2. Navicat 会自动生成该数据库的 ER 图
3. 可以直观看到表与表之间的关系（外键关联）

### 9.2 新建模型

1. 点击 **"文件"** → **"新建"** → **"模型"**
2. 在模型设计器中：
   - 从左侧拖拽"表"到画布
   - 编辑字段
   - 通过外键连线建立表关系
3. 点击 **"同步到数据库"** 将模型应用到数据库

---

## 十、数据传输与同步

### 10.1 数据传输

将一个数据库的表结构和数据复制到另一个数据库：

1. 点击 **"工具"** → **"数据传输"**
2. 选择源数据库和目标数据库
3. 选择要传输的表
4. 点击"开始"

### 10.2 数据同步

比较并同步两个数据库的结构差异：

1. 点击 **"工具"** → **"结构同步"**
2. 选择源和目标
3. 点击"比对"查看差异
4. 选择要同步的对象
5. 点击"运行查询"执行同步

---

## 十一、备份与恢复

### 11.1 备份数据库

1. 右键点击数据库 → **"转储 SQL 文件"** → **"结构和数据..."**
2. 选择保存位置
3. 点击"开始"

### 11.2 计划备份（定时备份）

1. 点击 **"文件"** → **"新建"** → **"备份"**
2. 选择要备份的数据库
3. 点击 **"计划"** → **"新建批处理作业"**
4. 设置执行时间（如每天凌晨 2 点）
5. 保存计划

---

## 十二、常用快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+Q` | 新建查询 |
| `Ctrl+N` | 新建记录 |
| `Ctrl+S` | 保存 |
| `F5` | 运行 SQL |
| `Ctrl+Shift+R` | 运行当前语句 |
| `Ctrl+B` | 美化 SQL |
| `Ctrl+F` | 查找 |
| `Ctrl+H` | 替换 |
| `Ctrl+R` | 刷新 |
| `Esc` | 停止运行 |

---

## 十三、常见问题

### 13.1 连接失败：10061 错误

**原因**：MySQL 服务未启动

**解决**：打开命令提示符，执行 `net start MySQL80`

### 13.2 连接失败：1045 错误

**原因**：用户名或密码错误

**解决**：检查用户名和密码，或重置 root 密码

### 13.3 中文乱码

**解决**：
1. 数据库字符集设置为 `utf8mb4`
2. 连接时在"高级"选项中设置编码为 `utf8mb4`
3. 导入 SQL 文件时选择 `utf8mb4` 编码

### 13.4 表数据无法编辑

**原因**：表没有主键

**解决**：给表添加主键，Navicat 需要主键来定位行数据进行编辑

# 安装

## 其他图形化工具

除了 Navicat，还有以下常用工具：

- **MySQL Workbench**（免费，官方推荐）: https://dev.mysql.com/downloads/workbench/
- **HeidiSQL**（免费轻量）: https://www.heidisql.com/
- **phpMyAdmin**（免费，Web 端）: https://www.phpmyadmin.net/
- **DBeaver**（免费开源，支持多数据库）: https://dbeaver.io/
- **DataGrip**（JetBrains 出品，收费）: https://www.jetbrains.com/datagrip/

# 小技巧

1. 在SQL语句之后加上`\G`会将结果的表格形式转换成行文本形式
2. 查看Mysql数据库占用空间：
```mysql
SELECT table_schema "Database Name"
     , SUM(data_length + index_length) / (1024 * 1024) "Database Size in MB"
FROM information_schema.TABLES
GROUP BY table_schema;
```

# 后续内容

后续内容因为跟当前学习、工作计划有冲突，所以后续课程的学习计划会无限期推后。
目前的工作重点放在重做一个学习笔记网站，当然这是边做边学的，开发过程中遇到的难点和知识点我也会记录下来供大家学习。
**在此感谢B站同样爱学习的同学 @守心-人 提供的后续课程笔记，大家有条件一定要去给个star，你的每一个star和点赞都是我们前进的动力**
**[https://github.com/Buildings-Lei/mysql_note/blob/main/README.md](https://github.com/Buildings-Lei/mysql_note/blob/main/README.md)**