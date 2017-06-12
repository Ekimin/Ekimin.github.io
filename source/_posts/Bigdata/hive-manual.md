---
title: Hive 常用操作
tags:
  - Hive
  - 大数据
categories: 大数据
date: 2017-06-12 17:38:25
updated:
comments:
permalink:
---


整理了 Hive 的常用操作，方便以后查询。

<!--more-->


# 1、Hive脚本执行

```bash
hive -f /path/xxx.hql
```

在Hive shell中则使用source

```bash
source /path/xxx.hql
```

#  2、创建Table

```bash
CREATE TABLE employees(
name STRING,
salary FLOAT,
subordinates ARRAY<STRING>,
deductions MAP<STRING,FLOAT>,
address STRUCT<street:STRING,city:STRING,state:STRING,zip:INT>
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\001'
COLLECTION items TERMINATED BY '\002'
MAP KEYS TERMINATED BY '\003'
LINES TERMINATED BY '\n'
STORED AS TEXTFILE;
```

row format delimited 这组关键字必须写在其他子句前；

\001是^A的八进制，fields terminated by ‘\001’，表明Hive使用^A字符作为列分隔符；

\002是^B的八进制，collection items terminated by ‘\002’，表明Hive使用^B字符作为集合元素间的分隔符；

\003是^C的八进制，map keys terminated bu ‘\003’，表明Hive使用^C字符作为map的键和值之间的分隔符；

Hive目前行与行之间的分隔符只支持字符’\n’；

Hive缺省情况下默认为textfile文件格式，所以最后一句一般不写。


hive元数据的格式:

John Doe^A100000.0^Amary Smith^Btodd jones^Afederal Taxes^C.2^BstateTaxes^C.05^Binsurance^C.1^Amichigan Ave^Bchicago^BIL^B60600

转为json格式：

```json
{
	name:John Doe,
	salary:100000.0,
	subordinates:[Mary Smith,Todd Jones],
	deductions:{
		Federal Taxes: .2,
		State Taxes: .05,
		Insurance: .1
	},
	address:{
	street:Michigan Ave,
	city:Chicago,
	state:IL
	zip:60600
	}
}
```

# 3、Hive数据注意点

Hive不支持行级插入操作、更新操作和删除操作。Hive也不支持事务。

# 4、IF NOT EXISTS/IF EXISTS

前者在创建表或者数据库时，避免新建的表或者数据库已经存在而抛出错误。
后者在删除表或者数据库时，避免表或者数据库不存在而抛出错误。

# 5、LOCATION

可以使用LOCATION 来修改存储位置。

```bash
CREATE DATABASE TEST LOCATION '/XX/XX/XX';
```

# 6、复制表模式

```bash
CREATE TABLE IF NOT EXISTS mydb.employees2 LIKE mydb.employees;
```

在指定的数据库mydb中复制一张employees2与employees一样，但是employees2中没有数据。

# 7、修改表

```bash
表重命名：ALTER TABLE log_message RENAME TO logmsgs;
增加分区表的某个分区：ALTER TABLE log_messages ADD IF NOT EXISTS PARTITION(year = 2011,month = 12,day = 2);
删除分区表的某个分区：ALTER TABLE log_messages DROP IF EXISTS PARTITION(year = 2011,month = 12,day = 2);
增加列：ALTER TABLE log_messages ADD CLOUMNS(app_name STRING);
```

# 8、通过查询语句向表中插入数据

```bash
INSERT OVERWRITE TABLE employees SELECT * FROM staged_employees;
```

如果用户指定了OVERWRITER关键字，那么目标文件夹中之前存在的数据将会被先删除掉。

# 9、类型转换

cast操作符，Hive不支持在数值末尾加上字母，必须使用cast操作符。

```bash
SELECT name,salary FROM employees WHERE cast(salary AS FLOAT) <10000.0;
```