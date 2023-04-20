# Mysql复习	

> 实用篇

##### 现只需要查看前2个用户明细设备ID数据

```mysql
常规做法：
	select device_id from user_profile where id = 1 or id = 2;
	
	
更快的做法：
	select device_id from user_profile limit 2

运行效率最高的做法：
	select device_id from user_profile limit 0, 2
```



##### 给列起别名

```mysql
select device_id as xxx from table limit 0, 2
```



##### like用法

```mysql
LIKE Operator	Description
WHERE CustomerName LIKE 'a%'	Finds any values that start with "a"

WHERE CustomerName LIKE '%a'	Finds any values that end with "a"

WHERE CustomerName LIKE '%or%'	Finds any values that have "or" in any position

WHERE CustomerName LIKE '_r%'	Finds any values that have "r" in the second position

WHERE CustomerName LIKE 'a_%'	Finds any values that start with "a" and are at least 2 characters in length

WHERE CustomerName LIKE 'a__%'	Finds any values that start with "a" and are at least 3 characters in length

WHERE ContactName LIKE 'a%o'	Finds any values that start with "a" and ends with "o"
```



##### between 用法

查询 之间(包含左右端)的数可以用between，少用and

```mysql
select 
    device_id,
    gender,
    age 
from 
    user_profile
where 
    age between 20 and 23
```



##### not in用法

```
not in (列名) 和 != 效果一样
```



##### 判断列的值是否为空时，不能使用运算符 != 

```
需要用 not null 或者 is not null
```



##### 同一列中多个不同值作为条件时，用 column in (xxx,xxy,xxz)



##### 保留x位小数： round(col, 位数)



##### Group by分组

```
那你听清楚我下面这个问题，分组SQL语句中，select和from和where和group by 和 having 这几部分的执行顺序是怎么样的？
我：先是from，再到 where ，再到select，再到group by ，最后having。

你能说说为什么是这样子的吗？
我：因为首先肯定是要确定数据从哪张表来，然后按where条件查询出结果，再然后才能进行group by分组（分组条件可以有多个，按字段顺序依次分组），最后是由having对分组后的结果集进行过滤
```

```
聚合函数结果作为筛选条件时，不能用where，而是用having语法，配合重命名即可；
```



##### 分别查看 和 结果不去重  不能使用or

> 需要使用union 两张表

```
不去重 用 union all
去重 用 union
```



##### If 和 Case 的用法

```
select 
    if(age >= 25, "25岁及以上", "25岁以下") as age_cut, 
    count(*) as number
from 
    user_profile
group by
    age_cut
```



```
select
    case
        when age >= 25 then "25岁及以上"
        else  "25岁以下" end  
    as age_cut,
    count(*) as number
from 
    user_profile
group by 
    age_cut

```



##### mysql中日期的使用

```
date
```



