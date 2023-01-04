---
title: MYSQL语句
cover: https://files.catbox.moe/l8sn6i.png
categories:
  - MYSQL

tags:
  - mysql
  - 语句
---

MYSQL知识点学习

学习网站：

https://dev.mysql.com/doc/refman/8.0/en/

![image-20220311093059904](C:\Users\moyu_\AppData\Roaming\Typora\typora-user-images\image-20220311093059904.png)

![image-20220311093514689](C:\Users\moyu_\AppData\Roaming\Typora\typora-user-images\image-20220311093514689.png)

![image-20220311093719662](C:\Users\moyu_\AppData\Roaming\Typora\typora-user-images\image-20220311093719662.png)

![image-20220311093834662](C:\Users\moyu_\AppData\Roaming\Typora\typora-user-images\image-20220311093834662.png)

表，记录，字段

E-R (entity-relationship,实体-联系)模型中有三个主要概念是:实体集、属性、联系集。

一个实体集(class)对应于数据库中的一个表(table),一个实体(instance)则对应于数据库表中的一行(row) ,也称为一条记录(record).一个属性(attribute)对应于数据库表中的一列(column),也称为一个字段(field).

关系型数据管理系统（RDBMS）之间的对比非关系型数据管理系统（RDBMS）(了解)：

1. 键值型数据库: Redis.
2. 文档型数据库: MongoDB
3. 搜索引Y数据库: ES, Solr
4. 列式数据库: HBase
5. 图形数据库: InfoGrid

ORM 思想：对象关系的映射（object Relational Mapping）

数据库中的一个表<--->java或python中的一个类

表中的一条数据<--->类中的一个对象（或实体）

表中的一个列<--->类中的一个字段、属性（fieid）

表的关联关系

四种：

一对一的关联，

一对多的关联（主表（从表）），

多对多的关联（主表（（联接表）从表）），

自我引用。



# SQL语言的分类

DDL：数据定义语言。

CREATE(创建) \  ALTER(修改) \ DROP(删除结构) \ RENAME(重命名) \ TRUNCATE(清空)

DML：数据操作语言。

INSEET(添加) \ DELETE(删除记录) \ UPDATE(修改) \ SELECT(*查询，场景，过滤)

DCL：操作控制语言。

COMMIT(提交修改永久性（事物相关）) 

ROLLBACK(回滚/撤回（事物相关）) 

SAVEPOINT(设置保存点) 

GRANT(赋予权限) \ REVOKE(取消权限)

命令结束符合: (; \G\g)

SQL 大小写规范：

1. MYSQL在Windows环境下是大小写不敏感的
2. MYSQL在Linux环境下是大小写敏感的
3. 数据库名、表名、表的别名、变量名是严格区分大小写的。
4. 关键字、函数名、列名(或字段名I、列的别名(字段的别名)是忽略大小写的。
5. 推荐采用统一的书写规范:。数据库名、表名、表别名、字段名、字段别名等都小写。
6. SQL关键字、函数名、绑定变量等都大写

可以使用如下格式的注释结构单行注释:

1. #注释文字(MySQL特有的方式)
2. 单行注释: I-注释文字(--后面必须包含一个空格。)
3. 多行注释: /*注释文字 */

导入现有的数据表、表数据：

1. source  文件的全路径名
2. 基于具体的图形化界面的工具可以导入数据

## msyql命令：

### 登录命令

```
mysql -u root -P 端口号 -h localhost -p // 登录指定服务器
```

### 查看版本

```
msyql -V
mysql --version
select version(); // 登录后查看当前版本信息
```

开启-服务器

```
net start mysql80
```

停止-服务器

```
net stop mysql80
```

查看数据库信息-DB：database,看做是数据库文件  

```
show databases;
```

创建数据库

```
create database 库名;
```

选中数据库

```
use 库名;
```

选中数据库查看表

```
show tables;          
```

创建表

```
 create table emplogees(需创建的参数);
```

添加数据

```
insert into 表名 values(1001,'Tom');
```

查看表数据

```
select * from employee;
```

添加数据库：（1.基于具体的图形化界面的工具可以导入数据）

```
source 文件的全路径名称
```

删除库

```
drop database 库名; 
```



## SQL语句，简单SELECT搜索语句：

查询多列

```
SELECT 列名 from 表名；
```

查询所有列

```
SELECT 列名 from 表名；

SELECT * from 表名；
```

查询结果去重：以下两种都可以进行去重查询，区别是： 用distinct去重，只能查询到去重的属性那一列，无法查询其他字段 用group by分组查询，可以根据需求查询对应的其他字段。

关键字去重：distinct（放置列表前进行使用,用于返回唯一不同的值）

分组去重：group by（进行分组查询)

```
SELECT DISTINCT 列名 from 表名；

SELECT 列名 from 表名 GROUP BY 列名；
```

查询结果限制返回行数：

```
SELECT 列名 FROM 表名 LIMIT 2

SELECT 列名 FROM 表名 LIMIT 0,2

SELECT 列名 FROM 表名 LIMIT 2 OFFSET 0  // 跳过0条，从第一条数据开始取，取两条数据 
```

也可结合 limit offset： 一起使用时，limit表示要取的数量，offset表示跳过的数量

将查询后的列重新命名：

```
SELECT 旧列名 as 新列名 FROM 表名 LIMIT 结果限制返回行数;

列的别名：
as:全称：alias（别名），可以省略。或空格
列的别名可以使用一对""引起来，不要使用''。

SELECT 旧列名 新列名 FROM 表名 LIMIT 结果限制返回行数;
```

空值参与运算，空值参与运算：结果一定也为空。

空值参与运算的解决方案：在空值前面加上IFNULL括起来，如果是null就拿0去替换

```
SELECT employee_id, "月工资"，salary * (1 + IFNULL(commission_pct,0)) * 12 ''"年工资"，commission_pct  FROM employees；
```

<=>:安全等于。可以查询到数据为null的。

函数查询为null：ISNULL(表名)

伪表(DUAL)：

```
SELECT 数值 FROM DUAL;
```

着重号``(在表名或库名与关键字重名时必须加上)

常数，一般用于不存在表中，但需要固定出现的场景中可使用，根据数据的行数都相应匹配一条

```
select employee_id,last_name,'常量',123（常量） FROM employees;
```

显示表结构

```
DESCRIBE 表名;

DESC 表名;
```

最小 LEAST(值) \ 最大 GREATEST(值)

in （set值），not in （set值，set）

查询介于之间语句：BETWEEN

```
例子：SELECT 列名 FROM 表名 WHERE createtime BETWEEN 数值 AND 数值
```

查询除某个数据信息：

```
!=，not in(值) （判断取值不是这个的值）
```

用WHERE过滤空值

判断取值为空的语句格式为：

```
列名 ``IS` `NULL
```

判断取值不为空的语句格式为：

```
列名 ``IS` `NOT` `NULL
```

高级操作符：OR（或）

模糊查询语句：like  '匹配内容'

考点 like 相似

```
like '%北京%'列名包括北京的字样
like '北京%' 列名北京开头
like '%北京' 列名北京结尾

```

## 正则表达式：

1. _ ：下划线 代表匹配任意一个字符；（代表一个不确定的字符）
2. % ：百分号 代表匹配0个或多个字符；
3. []: 中括号 代表匹配其中的任意一个字符；
4. [^]: ^尖冒号 代表 非，取反的意思；不匹配中的任意一个字符。

转义字符：\ ，在需要转义的符合前输入 \。

转义关键字：ESCAPE，把需要转义的字符，输入在后方。   

## REGEXP运算符

1. '^'匹配以该字符后面的字符开头的字符串。
2.  '$'匹配以该字符前面的字符结尾的字符串。
3. '.'匹配任何一个单字符。
4.  “[...]"匹配在方括号内的任何字符。例如, "[abc]"匹配"a"或"b"或"c"。为了命名字符的范围,使用一个'-'。"[a-z]"匹配任何字母,而“[0-9]"匹配任何数字。
5. ' * '匹配零个或多个在它前面的字符。例如, “ x* "匹配任何数量的'x'字符, " [0-9]* "匹配任何数量的数字,而"*"匹配任何数量的任何字符。

## 位运算符

（位运算符是在二进制上进行计算的运算符。位运算符会先将操作数变成二进制数,然后进行位运算,最后将计算结果从二进制变回十进制数。）

&（与（位AND）） |（或（位OR）） ^（异或（位XOR）） ~（按位取反） >>（按位右移位） <<（按位左移位）



## 排序与分页

排序：未使用排序操作，默认情况下查询返回的数据是按照添加数据的顺序显示的。

升序语法格式：（在ORDER BY 后未写关键字升降，默认为升序）

```
ORDER BY 列名 ASC;  # ascend
```

降序语法格式：

```
ORDER BY  列名  DESC; # descend
```

**注意：列的别名只能在ORDER BY中使用，不能在WHERE中使用。**

**强调格式：WHERE需要声明在FROM后，ORDER BY之前。**

二级排序，语法格式：以此类推

```
ORDER BY 列名 DESC,列名 ASC;
```

分页：mysql使用limit实习数据的分页显示

分页语法格式：（必须放置语句最后）

```
LIMIT 每页显示pageSize条记录，此时显示第pageNO页；
公式：LIMIT (pageNO-1) * pageSize,pageSize;
```

MYSQL8.0新特性：LIMIT...OFFSET...语法格式：

```
SELECT 列名 FROM 表名 LIMIT 页数 OFFSET 条数;
```

字节函数：

```
LENGTH(列名);
```

函数时间戳转换为日期类型进行显示：

```
FROM_UNIXTIME（参数，format（是可选参数的，进行转化格式））
```

（1）不含有format参数，默认返回%Y-%m-%d %H:%i:%s的格式：

```
SELECT FROM_UNIXTIME(参数) FROM 表名;
```

```
返回格式如下： 2020-03-23 15:28:46
```

  （2）含有format参数：

```
SELECT FROM_UNIXTIME(参数,'%Y-%m-%d'（选择要转化的格式）) FROM 表名;
```

```
返回格式如下：2020-03-23
```

format（可选参数）：

```
%M 月名字(January……December)
%W 星期名字(Sunday……Saturday)
%D 有英语前缀的月份的日期(1st, 2nd, 3rd, 等等。）
%Y 年, 数字, 4 位
%y 年, 数字, 2 位
%a 缩写的星期名字(Sun……Sat)
%d 月份中的天数, 数字(00……31)
%e 月份中的天数, 数字(0……31)
%m 月, 数字(01……12)
%c 月, 数字(1……12)
%b 缩写的月份名字(Jan……Dec)
%j 一年中的天数(001……366)
%H 小时(00……23)
%k 小时(0……23)
%h 小时(01……12)
%I 小时(01……12)
%l 小时(1……12)
%i 分钟, 数字(00……59)
%r 时间,12 小时(hh:mm:ss [AP]M)
%T 时间,24 小时(hh:mm:ss)
%S 秒(00……59)
%s 秒(00……59)
%p AM或PM
%w 一个星期中的天数(0=Sunday ……6=Saturday ）
%U 星期(0……52), 这里星期天是星期的第一天
%u 星期(0……52), 这里星期一是星期的第一天
%% 一个文字“%”。
```

函数IFNULL使用说明：

MySQL IFNULL 函数是MySQL控制流函数之一，它接受两个参数，如果不是NULL，则返回第一个参数。否则，IFNULL函数返回第二个参数。（两个参数可以是文字或表达式）；

IFNULL函数语法：

```
IFNULL(参数1，参数2); # 如果参数1不为NULL，则IFNULL函数返回参数1;否则返回参数2的结果。
```



## 多表查询：

笛卡尔积(或交叉连接)的理解。

笛卡尔乘积是一个数学运算。假设我有两个集合X和Y,那么X和Y的笛卡尔积就是×和Y的所有可能组合,也就是第一个对象来自于x,第二个对象来自于Y的所有可能。组合的个数即为两个集合中元素个数的乘积数。 

SQL99：`CROSS  JOIN` 表示交叉连接。它的作用就是可以把任意表进行连接，即使这两张表不相关。

多表查询需要有连接条件：

```
SELECT employees_id,department_name,department_id 
FROM employess,department
# 两个表的连接条件
WHERE 表名.列名 = 表名.列名 # 同等的条件
```

如果查询语句中出现了多个表中都存在的字段，则必须指明此字段所在的表。

```
SELECT employees_id,department_name,表名.department_id 
FROM employess,department
WHERE 表名.列名 = 表名.列名 # 同等的条件
```

多表查询，建议：

1. 从sql优化的角度，建议多表查询时，每个字段前都指明其所在的表。
2. 可以给表起别名，在SELECT和WHERE中使用表的别名。（一旦在SELECT和WHERE中使用，则必须使用表的别名，而不能再使用表的原名）

结论：如果有n个表实现有多表查询，则需要至少n-1个连接条件。（否则会出现笛卡尔积的错误）

### 多表查询的分类：

角度1：等值连接 vs 非等值连接

角度2：自连接 vs 非自连接

角度3：内连接 vs 外连接

内连接：合并具有同一列的两个以上的表的行，结果集中不包含一个表与另一个表不匹配的行。

外连接：合并具有同一列的两个以上的表的行，结果集中除了包含一个表与另一个表匹配的行之外，

​               还查询到了左表 或 右表中不匹配的行。

外连接的分类：左外连接、右外连接、满外连接。

左外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回左表中不满足条件的行,

​                  这种连接称为左外连接。

右外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回右表中不满足条件的行,

​                  这种连接称为右外连接。

SQL92语法实现内连接：

```
SELECT e.employees_id,b.department_name,e.department_id 
FROM employess e,department b
WHERE 表名.列名 = 表名.列名; # 同等的条件
```

SQL92语法实现外连接：使用 + ------- MYSQL不支持SQL92语法中外连接的写法！

（Oracle支持SQL92语法实现外连接）

```
SELECT e.employees_id,b.department_name,e.department_id 
FROM employess e,department b
WHERE 表名.列名 = 表名.列名(+); # 同等的条件
```

SQL99语法中使用JOIN ...ON的方式实现多表的查询。这种方式也能解决外连接的问题。MYSQL是支持此种方式的。

SQL99语法实现内连接：

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 INNER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.表名 
JOIN 表名 别名
ON 别名.列名 = 别名.表名

```

SQL99语法实现外连接：

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 OUTER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.列名;
```

SQL99语法实现左连接：

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 LEFT OUTER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.列名;
```

SQL99语法实现右连接：

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 RIGHT OUTER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.列名;
```

满外连接：mysql不支持FULL OUTER JOIN语法（Oracle支持SQL92 FULL OUTER JOIN语法）

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 FULL OUTER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.列名;
```

![image-20220314153310242](C:\Users\moyu_\AppData\Roaming\Typora\typora-user-images\image-20220314153310242.png)

UNION操作符：UNION操作符返回两个查询的结果集的并集，去除重复记录。

​                         （去重时导致，效率过低）

```

```

UNION ALL操作符：UNION ALL操作符返回两个查询的结果集的并集。对于两个结果集的重复部分，

​                                  不去重。

```

```

注意:执行UNION ALL语句时所需要的资源TUNION语句少。如果明确知道合并数据后的结果数据不存在重复数据,或者不需要去除重复的数据,则尽量使用UNION ALL语句,以提高数据查询的效率。

7种JOIN的实现：

中图，内连接：

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 JOIN 表名 别名
ON 别名.列名 = 别名.表名 ;
```

左上图，左外连接：

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 LEFT JOIN 表名 别名
ON 别名.列名 = 别名.表名 ;
```

右上图，右外连接：

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 RIGHT JOIN 表名 别名
ON 别名.列名 = 别名.表名 ;
```

左中图：

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 LEFT JOIN 表名 别名
ON 别名.列名 = 别名.表名
WHERE （左表） 别名.列名 IS NULL;
```

右中图：

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 RIGHT JOIN 表名 别名
ON 别名.列名 = 别名.表名
WHERE （右表）别名.列名 IS NULL;
```

左下图：满外连接

方式1：左上图 UNION ALL 右中图

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 LEFT JOIN 表名 别名
ON 别名.列名 = 别名.表名
WHERE （左表） 别名.列名 IS NULL;
UNION ALL
SELECT 别名.列名,别名.列名
FROM 表名 别名 RIGHT JOIN 表名 别名
ON 别名.列名 = 别名.表名
WHERE （右表）别名.列名 IS NULL;
```

方式2：左中图 UNION ALL 右上图

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 LEFT JOIN 表名 别名
ON 别名.列名 = 别名.表名
WHERE （左表） 别名.列名 IS NULL
UNION ALL
SELECT 别名.列名,别名.列名
FROM 表名 别名 RIGHT JOIN 表名 别名
ON 别名.列名 = 别名.表名
```

右下图：左中图 UNION ALL 右中图

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 LEFT JOIN 表名 别名
ON 别名.列名 = 别名.表名
WHERE （左表） 别名.列名 IS NULL
UNION ALL
SELECT 别名.列名,别名.列名
FROM 表名 别名 RIGHT JOIN 表名 别名
ON 别名.列名 = 别名.表名
WHERE （左表） 别名.列名 IS NULL;
```

SQL99语法的新特性1：自然连接

NATURAL JOIN：它会帮你自动查询两张连接表中`所有相同的字段`，然后进行`等值连接`。

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 NATURAL JOIN 表名 别名;
```

SQL99语法的新特性2：自然连接

USING：适用于多表查询，有相同列名的查询（不适用与自连接）

```
SELECT 别名.列名,别名.列名
FROM 表名 别名 JOIN 表名 别名
USING(列名);
```

拓展：可以先JOIN连接但不建议（具体看需求）

```
SELECT 列名 FROM A表名
INNER JOIN B表名 INNER JOIN C表名
ON 别名.列名 = 别名.列名
AND 别名.列名 = 别名.列名；
```



## 函数的理解：

从函数定义的绝度出发，我们可以将函数分成内置函数和自定义函数。

MySQL提供的内置函数从实现的功能角度可以分为数值函数、字符串函数、日期和时间函数、流程控制函数、加密与解密函数、获取MySQL信息函数、聚合函数等。这里,我将这些丰富的内置函数再分为两类:单行函数、聚合函数(或分组函数)

两种SQL函数：（单行函数，多行函数）

单行函数：

1. 操作数据对象，接受参数返回一个结果。
2. 只对一行进行变换，每行返回一个结果。
3. 可以嵌套，参数可以是一列或一个值。

1.数值函数

1.1基本的操作：

```
# 返回绝对值,取相反
ABS(-132); 
ABS(32);
# 返回x的符合。正数返回1,负数返回-1,0返回o
SIGN(x); 
SIGN(23); 
SIGN(-23); 
# 返回圆周率的值(常量)
PI(); 
# 返回大于或等于某个值的最小整数(取值天花板，取上)
CELL(33.2);
CEILING(44.3); 
# 返回小于或等于某个值的最大整数(取值天花板，取下)
FLOOR(33.2); 
# 返回列表中的最小值
LEAST(e1,e2,e3);
# 返回列表中的最大值
GREATEST(e1,e2,e3);
# 返回x除以Y后的余数(取余数)
MOD(12,5); 

```

1.2取随机数：（也称伪随机数，指定因子返回数则固定）

```;
# 返回0~1的随机值
RAND();
# 返回0~1的随机值,其中x的值用作种子值,相同的x值会产生相同的随机数
RAND(x);
RAND(10（因子）);
RAND(10);
```

1.3四舍五入，截断操作：

```
# 返回一个对x的值进行四舍五入后,最接近于x的整数(默认截断，舍取小数)
ROUND(123.556);
# 返回一个对x的值进行四舍五入后最接近x的值,并保留到小数点后面Y位
  (保留0位小数，从小数点后进行截断进行四舍五入)
ROUND(123.456,0);
ROUND(123.556,1);
ROUND(123.456,2);
# 保留-1位小数，从小数点前进行截断进行四舍五入
ROUND(123.556,-1);
ROUND(123.456,-2);
ROUND(125.556,-3);
# 返回数字x截断为y位小数的结果(截断，必须舍掉)
TRUNCATE(125.556,1);
# 返回x的平方根。当x的值为负数时,返回NULL
SQRT(x);
# 单行函数可以嵌套
SELECT TRUNCATE(ROUND(125.553,0),0)
FROM DUAL;
```

1.4角度与弧度的互换

```
# 弧度(将角度转化为弧度,其中,参数x为角度值)
RADIANS(X);
# 角度(将弧度转化为角度,其中,参数x为弧度值)
DEGREES(X);
例子：
SELECT RADIANS(30),RADIANS(45),RADIANS(60),RADIANS(90),
DEGREES(2*PI()),DEGREES(RADIANS(60));
```

1.5三角函数

```
# 返回x的正弦值,其中,参数x为弧度值
SIN(x);
# 返回x的反正弦值,即获取正弦为x的值。如果x的值不在-1到1之间,则返回NULL
ASIN(x);
# 返回x的余弦值,其中,参数x为弧度值
COS(x);
# 返回x的反余弦值,即获取余弦为x的值。如果x的值不在-1到1之间,则返回NULL
ACOS(x);
# 返回x的正切值,其中,参数x为弧度值
TAN(x);
# 返回x的反正切值,即返回正切值为x的值
ATAN(x);
# 返回两个参数的反正切值
ATAN2(m,n);
# 返回x的余切值,其中,x为弧度值
COT(x);
```

1.6指数和对数

```
# 返回x的y次方
POW(x,y),POWER(x,y);
# 返回e的x次方,其中e是一个常数, 2.718281828459045
EXP(x);
# 返回以e为底的x的对数,当X<=0时,返回的结果为NULL
LN(x),LOG(x);
# 返回以10为底的x的对数,当X<=0时,返回的结果为NULL
LOG10(x);
# 返回以2为底的x的对数,当X<=0时,返回NULL
LOG2(x);
```

1.7进制间的转换

```
# 返回x的二进制编码
BIN(x);
# 返回x的十六进制编码
HEX(x);
# 返回x的八进制编码
OCT(x);
# 返回f1进制数变成f2进制数
CONV(x,f1,f2);
```

2.字符串函数

###### # ASCII() 返回字符串S中的第一个字符的ASCII码值

```
  ASCII(S);
# LENGTH()返回字符串s的字节数，和字符集有关。
  LENGTH(s);
# CHAR_LENGTH()返回字符串s的字符数。
  CHAR_LENGTH(s);
```

###### # 连接s1,s2,......,sn为一个字符串（可以申明多个字符串进行拼接）

```
例子：SELECT CONCAT(别名.列名, ' 固定连接字符串 ', 别名.列名) "列名展示" 
      FROM 表名 别名 JOIN 表名 别名
      WHERE 别名.列名 = 别名.列名;
```

###### # 同CONCAT(s1,s2,...)函数，但是每个字符串之间要加上x

```
CONCAT_WS(指定字符串对每个参数进行连接,s1,s2,......,sn);
```

###### # 将字符串str从第idx位置开始，len个字符长的子串替换为字符串replacestr（字符串的索引是从1开始的）

```
INSERT(str, idx(索引位置), len,replacestr(需要替换的字符串))
```

###### # 用字符串b替换字符串str中所有出现的字符串a

```
REPLACE(str, a(需要要替换的字符串), b(替换上的字符串))
```

###### # 将字符串s的所有字母转成大写字母

```
UPPER(s) 或 UCASE(s)
```

###### # 将字符串s的所有字母转成小写字母

```
LOWER(s) 或LCASE(s)
```

###### # 返回字符串str最左边的n个字符

```
LEFT(str,n(从最左边，根据数值取出字符))
```

###### # 返回字符串str最右边的n个字符

```
RIGHT(str,n(从最右边，根据数值取出字符))
```

###### # 用字符串pad对str最左边进行填充，直到str的长度为len个字符

```
LPAD: 实现右对齐效果
LPAD(str, len(需要占满多少位置), pad(不够使用值进行补齐))
```

###### # 用字符串pad对str最右边进行填充，直到str的长度为len个字符

```
RPAD: 实现左对齐效果
RPAD(str ,len, pad)
```

###### # 去掉字符串s开始与结尾的空格

```
TRIM(s)
```

###### # 去掉字符串s左侧的空格

```
LTRIM(s)
```

###### # 去掉字符串s右侧的空格

```
RTRIM(s)
```

###### # 去掉字符串s开始与结尾的s1

```
TRIM(s1 FROM s)
例子：TRIM('字符串'(首尾出现进行去除) FROM '字符串'(首尾出现相同的字符串则进行去除));
```

###### # 去掉字符串s开始处的s1

```
TRIM(LEADING s1FROM s)
```

###### # 去掉字符串s结尾处的s1

```
TRIM(TRAILING s1FROM s)
```

###### # 返回str重复n次的结果

```
REPEAT(str, n(数值，重复的次数))
```

###### # 返回n个空格

```
SPACE(n)
```

###### # 比较字符串s1,s2的ASCII码值的大小

```
STRCMP(s1,s2)
```

###### # 返回从字符串s的index位置其len个字符，作用与SUBSTRING(s,n,len)、MID(s,n,len)相同

```
SUBSTR(s,index(),len(取的长度))
```

###### # 返回字符串substr在字符串str中首次出现的位置，作用于POSITION(substrIN str)、INSTR(str,substr)相同。

######    未找到，返回0

```
LOCATE(substr,str)
```

###### # 返回指定位置的字符串，如果m=1，则返回s1，如果m=2，则返回s2，如果m=n，则返回sn\

```
ELT(m,s1,s2,…,sn)
```

###### # 返回字符串s在字符串列表中第一次出现的位置

```
FIELD(s,s1,s2,…,sn)
```

###### # 返回字符串s1在字符串s2中出现的位置。其中，字符串s2是一个以逗号分隔的字符串

```
FIND_IN_SET(s1,s2)
```

###### # 返回s反转后的字符串

```
REVERSE(s)
```

###### # 比较两个字符串，如果value1与value2相等，则返回NULL，否则返回value1

```
NULLIF(value1,value2)
```

注意：MySQL中，字符串的位置是从1开始的。



