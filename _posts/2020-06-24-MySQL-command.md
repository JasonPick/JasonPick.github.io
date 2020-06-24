---
layout: post
title: MYSQL语句复习
subtitle: 复习整理
cover-img: /assets/img/path.jpg
tags: [mysql]
categories: ['MySQL']
---

这是一篇对于MYSQL语句的复习博客，在[这里练习sql语句](https://www.sql-ex.ru/learn_exercises.php#answer_ref)

## MYSQL语句


1. **BETWEEN AND**


检索列prod_name, prod_price，同时prod_price在5至10之间


```SELECT prod_name, prod_price FROM Products WHERE prod_price BETWEEN 5 AND 10;```


2. **IS NULL**


检索列prod_name, prod_price，同时prod_price为空值


```SELECT prod_name, prod_price FROM Products WHERE prod_price IS NULL;```


3.字符匹配


检索列prod_id, prod_name，其中prod_name以词Fish起头、包含"bean bag"、以F开头y结尾


```SELECT  prod_id, prod_name FROM Products WHERE prod_name LIKE 'Fish%';```

```SELECT  prod_id, prod_name FROM Products WHERE prod_name LIKE '%bean bag%';```

```SELECT  prod_id, prod_name FROM Products WHERE prod_name LIKE 'F%y';```



4. **Concat**


Vendors表包含列vend_name和vend_country，用括号将vend_country括起来，组合两列返回（使用别名vend_title）


```SELECT Concat(vend_name, '(', vend_country, ')') AS vend_title FROM Vendors ORDER BY vend_name;```


5. **UPPER**


在Vendors中，将vend_name显示为小写和大写（vend_name_upcase）


```SELECT vend_name, UPPER(vend_name) AS vend_upcase FROM Vendors ORDER BY vend_name;```


6.**JION**


在Vendors和Products中，选取列vend_name, prod_name, prod_price（vend_id相同，等值联结）（+使用内联结）


第一种方式使用值相等的隐式链接


```SELECT vend_name, prod_name, prod_price FROM Vendors, Products WHERE Vendors.vend_id = Products.vend_id;```


第二种方式使用显示链接


```SELECT vend_name, prod_name, prod_price FROM Vendors INNER JOIN Products ON Vendors.vend_id = Products.vend_id;```


7. **INSERT INTO**


在Customers中，插入一行数据，cust_id=10006, cust_name=yano(+并删除)


```INSERT INTO Customers (cust_id, cust_name) VALUES(10006, 'yano');```


 ```DELETE from Customers where cust_name = 'yano';```
 
 
 8. **CREATE TABLE**
 
 
 将表Customers的内容全部复制到CustCopy表中


 ```CREATE TABLE CustCopy AS  SELECT * FROM Customers;```
 
 
 9. **UPDATE**
 
 
 在Customers中，更新cust_id为1000000005的用户，将cust_email改为'yano_nankai'


```UPDATE Customers SET cust_email = 'yano_nankai' WHERE cust_id = '1000000005';```


综合训练


创建表yano，id为整数非空，name为10个字符非空（默认为hello），并更新表结构（增加和删除phone列，20个字符非空），将id增加约束为主键，删除表yano


```
  CREATE TABLE YANO( id INTEGER NOT NULL, name CHAR(10) NOT NULL DEFAULT 'HELLO');
  ALTER TABLE YANO ADD phone CHAR(20);
  ALTER TABLE YANO DROP COLUMN phone;
```


10. **SHOW INDEX**


查看Orders的索引


```SHOW INDEX FROM Orders;```
