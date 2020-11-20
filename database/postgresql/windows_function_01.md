# 记录一次开发中遇到的问题

刚刚开始使用postgresql, 今天在开发的时候，遇到一个使用window function的问题.

window function 跟group by 很类似，但是不像group by 每一个分组只有一个值，window function可以让每一行都可以根据window计算出一个值。



先来看问题

假设有以下一个表 `demo_01`

```sql
	create table demo_01(
		trade_date date,
		value int
	);
	insert into demo_01(trade_date, value) 
	values('2010-01-01', 1),
	('2010-01-01', 2),
	('2010-01-02', 3),
	('2010-01-02', 4),
	('2010-01-06', 5);
```

|trade_date|value|
|--|--|
|2010-01-01|1|
|2010-01-01|2|
|2010-01-02|3|
|2010-01-02|4|
|2010-01-06|5|



```sql
	select 
		trade_date,
		value,
		sum(value) over() as sum1,
		sum(value) over(order by trade_date) as sum2,
		sum(value) over(order by trade_date rows between 1 preceding and current row) as sum3,
		sum(value) over(order by trade_date groups between 1 preceding and current row) as sum4,
		sum(value) over(order by trade_date range between '1 day' preceding and current row) as sum5
	from 
		demo_01
	order by trade_date
```

以上SQL的输出结果为

> trade_date | value | sum1 | sum2 | sum3 | sum4 | sum5
------------+-------+------+------+------+------+------
 2010-01-01 |     1 |   15 |    3 |    1 |    3 |    3
 2010-01-01 |     2 |   15 |    3 |    3 |    3 |    3
 2010-01-02 |     3 |   15 |   10 |    5 |   10 |   10
 2010-01-02 |     4 |   15 |   10 |    7 |   10 |   10
 2010-01-06 |     5 |   15 |   15 |    9 |   12 |    5
(5 rows)



* sum1 对应的窗口是表中所有的行,所以每一行对应的值都是15
* sum2 窗口帧中，每一行order by 字段的值相同的行集合是一样的，所以2020-01-01对应的两行的的sum2的值是相同的
* sum3 每行的窗口帧中包含个当前行和前面的一样，所以每一行的sum3都是当前行和上一样的value的和
* sum4 每行的窗口帧中包含跟当前行中order by 字段值相同的平行组以及前一个平行组, 所以2010-01-02中sum4的值对应的是trade_date为`2020-01-01`和`2020-01-02`中所有行中value的和
* sum5每行的窗口帧中包含跟当前行中order by 字段值相同的平行组以及当前order by 字段-1后的值对应的平行组，而不是按照行的顺序的前一个平行组，所以trade_date为`2020-01-06`对应sum5的值为`5`,因为它的窗口帧包含的是trade_date为`2010-01-06`的平行组和trade_date为`2010-01-05`的平行组中value的和，而不是`trade_date为`2010-01-06'的平行组和trade_date为`2010-01-02`的平行组中value的和