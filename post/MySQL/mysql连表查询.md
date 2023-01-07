---
title:  多表查询
---

## 多表查询

![](https://img-blog.csdnimg.cn/cd54817c56d84c7d908e7f2629904144.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbWFzdGVyIGNhdA==,size_20,color_FFFFFF,t_70,g_se,x_16)



笛卡尔积(或交叉连接)的理解。

笛卡尔乘积是一个数学运算。假设我有两个集合X和Y,那么X和Y的笛卡尔积就是×和Y的所有可能组合,也就是第一个对象来自于x,第二个对象来自于Y的所有可能。组合的个数即为两个集合中元素个数的乘积数。 

SQL99：`CROSS  JOIN` 表示交叉连接。它的作用就是可以把任意表进行连接，即使这两张表不相关。



多表查询需要有连接条件：

```mysql
SELECT employees_id,department_name,department_id 
FROM employess,department
# 两个表的连接条件
WHERE 表名.列名 = 表名.列名 # 同等的条件
```

如果查询语句中出现了多个表中都存在的字段，则必须指明此字段所在的表。

```mysql
SELECT employees_id,department_name,表名.department_id 
FROM employess,department
WHERE 表名.列名 = 表名.列名 # 同等的条件
```



多表查询，建议：

1. 从sql优化的角度，建议多表查询时，每个字段前都指明其所在的表。
2. 可以给表起别名，在SELECT和WHERE中使用表的别名。（一旦在SELECT和WHERE中使用，则必须使用表的别名，而不能再使用表的原名）

结论：如果有n个表实现有多表查询，则需要至少n-1个连接条件。（否则会出现笛卡尔积的错误）



## 多表查询的分类：

角度1：等值连接 vs 非等值连接

角度2：自连接 vs 非自连接

角度3：内连接 vs 外连接



内连接：合并具有同一列的两个以上的表的行，结果集中不包含一个表与另一个表不匹配的行。

外连接：合并具有同一列的两个以上的表的行，结果集中除了包含一个表与另一个表匹配的行之外，还查询到了左表 或 右表中不匹配的行。

外连接的分类：左外连接、右外连接、满外连接。

左外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回左表中不满足条件的行,这种连接称为左外连接。

右外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回右表中不满足条件的行, 这种连接称为右外连接。



<img src="https://files.catbox.moe/m60map.png" style="zoom:50%;" />



SQL92语法实现内连接：

```mysql
SELECT e.employees_id,b.department_name,e.department_id 
FROM employess e,department b
WHERE 表名.列名 = 表名.列名; # 同等的条件
```

SQL92语法实现外连接：使用 + ------- MYSQL不支持SQL92语法中外连接的写法！

（Oracle支持SQL92语法实现外连接）

```mysql
SELECT e.employees_id,b.department_name,e.department_id 
FROM employess e,department b
WHERE 表名.列名 = 表名.列名(+); # 同等的条件
```



SQL99语法中使用JOIN ...ON的方式实现多表的查询。这种方式也能解决外连接的问题。MYSQL是支持此种方式的。

SQL99语法实现内连接：

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 INNER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.表名 
JOIN 表名 别名
ON 别名.列名 = 别名.表名

```

SQL99语法实现外连接：

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 OUTER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.列名;
```

SQL99语法实现左连接：

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 LEFT OUTER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.列名;
```

SQL99语法实现右连接：

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 RIGHT OUTER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.列名;
```



满外连接：mysql不支持FULL OUTER JOIN语法（Oracle支持SQL92 FULL OUTER JOIN语法）

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 FULL OUTER（可省略） JOIN 表名 别名
ON 别名.列名 = 别名.列名;
```



UNION的使用

```mysql
SELECT 别名.列名,.....FROM 表名 别名
UNION [ALL]
SELECT 别名.列名,.....FROM 表名 别名
```

UNION操作符：UNION操作符返回两个查询的结果集的并集，去除重复记录。（去重时导致，效率过低）

UNION ALL操作符：UNION ALL操作符返回两个查询的结果集的并集。对于两个结果集的重复部分不去重。

注意:执行UNION ALL语句时所需要的资源TUNION语句少。如果明确知道合并数据后的结果数据不存在重复数据,或者不需要去除重复的数据,则尽量使用UNION ALL语句,以提高数据查询的效率。



## 7种JOIN的实现：

中图，内连接：

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 JOIN 表名 别名
ON 别名.列名 = 别名.表名 ;
```

左上图，左外连接：

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 LEFT JOIN 表名 别名
ON 别名.列名 = 别名.表名 ;
```

右上图，右外连接：

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 RIGHT JOIN 表名 别名
ON 别名.列名 = 别名.表名 ;
```

左中图：

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 LEFT JOIN 表名 别名
ON 别名.列名 = 别名.表名
WHERE （左表） 别名.列名 IS NULL;
```

右中图：

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 RIGHT JOIN 表名 别名
ON 别名.列名 = 别名.表名
WHERE （右表）别名.列名 IS NULL;
```



左下图：满外连接

方式1：左上图 UNION ALL 右中图

```mysql
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

```mysql
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

```mysql
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

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 NATURAL JOIN 表名 别名;
```



SQL99语法的新特性2：自然连接

USING：适用于多表查询，有相同列名的查询（不适用与自连接）

```mysql
SELECT 别名.列名,别名.列名
FROM 表名 别名 JOIN 表名 别名
USING(列名);
```



拓展：可以先JOIN连接但不建议（具体看需求）

```mysql
SELECT 列名 FROM A表名
INNER JOIN B表名 INNER JOIN C表名
ON 别名.列名 = 别名.列名
AND 别名.列名 = 别名.列名；
```