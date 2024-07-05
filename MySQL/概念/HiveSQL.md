# HiveSQL常用函数

## 时间函数

* current_date()：获取当前格式化日期
```mysql
hive> select current_date();
OK
2021-07-19
Time taken: 0.045 seconds, Fetched: 1 row(s)
```

* current_timestamp()：获取当前格式化时间
```mysql
hive> select current_timestamp();
OK
2021-07-19 14:15:56.822
Time taken: 0.052 seconds, Fetched: 1 row(s)
```

* unix_timestamp()：获取当前unix时间戳
```mysql
hive> select unix_timestamp();
OK
1626665638
Time taken: 11.868 seconds, Fetched: 1 row(s)
```

* from_unixtime()：把unix时间戳转化为格式化时间
```mysql
hive> select from_unixtime(1626665638,'yyyy-MM-dd HH:mm:ss');
OK
2021-07-19 11:33:58
Time taken: 0.111 seconds, Fetched: 1 row(s)
```
注：from_unixtime() 第一个参数为数值类型，第二个参数为所需时间格式

* 把当前时间戳转化为格式化时间：
```mysql
hive> select from_unixtime(unix_timestamp(),'yyyy-MM-dd HH:mm:ss');
OK
2021-07-19 11:47:09
Time taken: 0.089 seconds, Fetched: 1 row(s)
```

* 取当前格式化时间的前30天：
```mysql
hive> select from_unixtime(unix_timestamp('2021-07-19','yyyy-MM-dd')-30*86400,'yyyy-MM-dd');
OK
2021-06-19
Time taken: 0.673 seconds, Fetched: 1 row(s)
```

* year、month、day、hour、minute、second：年、月、日、时、分、秒
```mysql
spark-sql> select year('2021-08-11');
2021
Time taken: 2.693 seconds, Fetched 1 row(s)
```
```mysql
spark-sql> select month('2021-08-11');
8
Time taken: 3.502 seconds, Fetched 1 row(s)
```
```mysql
spark-sql> select day('2021-08-11');
11
Time taken: 0.73 seconds, Fetched 1 row(s)
```
```mysql
spark-sql> select hour('2021-08-11 10:34');
10
Time taken: 0.787 seconds, Fetched 1 row(s)
```
```mysql
spark-sql> select minute('2021-08-11 10:34');
34
Time taken: 0.079 seconds, Fetched 1 row(s)
```
```mysql
spark-sql> select second('2021-08-11 10:34:40');
40
Time taken: 0.666 seconds, Fetched 1 row(s)
```

* date_add()、date_sub()：取格式化时间的前一天（n天）
```mysql
hive> select date_sub('2021-07-19',1);
OK
2021-07-18
Time taken: 0.052 seconds, Fetched: 1 row(s)
```
```mysql
hive> select date_add('2021-07-19',-1);
OK
2021-07-18
Time taken: 0.051 seconds, Fetched: 1 row(s)
```

* date_add()、date_sub()：取格式化时间的后一天（n天）
```mysql
hive> select date_add('2021-07-19',1);
OK
2021-07-20
Time taken: 4.623 seconds, Fetched: 1 row(s)
```
```mysql
hive> select date_sub('2021-07-19',-1);
OK
2021-07-20
Time taken: 0.046 seconds, Fetched: 1 row(s)
```

* add_months()、add_years():当前时间的后n月（年）
```mysql
SELECT add_months(your_date_column, -3) AS previous_3_months_date  
FROM your_table;
```
```mysql
SELECT add_years(your_date_column, -1) AS last_year_same_day  
FROM your_table;
```

* to_date()：当前格式化时间（含时分秒）转化为年月日
```mysql
hive> select to_date('2021-07-19 13:34:12');
OK
2021-07-19
Time taken: 0.092 seconds, Fetched: 1 row(s)
```
注：to_date() 默认转化格式为yyyy-MM-dd

* date_format()：对日期进行格式化
```mysql
spark-sql> select date_format('2021-08-11 10:45:40','yyyy-MM-dd');
2021-08-11
Time taken: 3.999 seconds, Fetched 1 row(s)
```
注：比to_date()函数稍微灵活一点

* weekofyear()：日期转周（当前日期是一年的第几周）
```mysql
spark-sql> select weekofyear('2021-08-11');
32
Time taken: 0.098 seconds, Fetched 1 row(s)
```

* dayofyear()：日期转天（当前日期是一年的第几天）
```mysql
spark-sql> select dayofyear('2021-08-11');
223
Time taken: 0.649 seconds, Fetched 1 row(s)
```

* add_months()：当前时间的前一个月或者后一个月
```mysql
hive> select add_months('2021-07-19',1);
OK
2021-08-19
Time taken: 0.045 seconds, Fetched: 1 row(s)

hive> select add_months('2021-07-19',-1);
OK
2021-06-19
Time taken: 0.146 seconds, Fetched: 1 row(s)
```

* datediff()：获取两个时间的天数差值
```mysql
hive> select datediff('2021-07-20','2021-07-19');
OK
1
Time taken: 0.052 seconds, Fetched: 1 row(s)
```
注：默认只做天数的差值，较大的在前结果为正数，较大的在后结果为负数

* next_day()：获取指定时间的下一个星期几
```mysql
hive> select next_day('2021-07-20','su');
OK
2021-07-25
Time taken: 0.081 seconds, Fetched: 1 row(s)
```
```mysql
hive> select next_day('2021-07-20','SU');
OK
2021-07-25
Time taken: 2.113 seconds, Fetched: 1 row(s)

hive> select next_day('2021-07-20','SUN');
OK
2021-07-25
Time taken: 0.057 seconds, Fetched: 1 row(s)

hive> select next_day('2021-07-20','SUNDAY');
OK
2021-07-25
Time taken: 0.569 seconds, Fetched: 1 row(s)
```
注：next_day()第二个参数支持小写、大写、缩写（su/sun/sunday）

* trunc()：获取当月第一天
```mysql
hive> select trunc('2021-07-20','MM');
OK
2021-07-01
Time taken: 0.043 seconds, Fetched: 1 row(s)
```

* trunc()：获取当年第一天
```mysql
hive> select trunc('2021-06-20','YY');
OK
2021-01-01
Time taken: 0.042 seconds, Fetched: 1 row(s)
```

## 字符串函数

* 字符串长度计算函数：length

语法：length(string A)，

返回值：int

例子：
```mysql
spark-sql> select length('abc');
3
Time taken: 0.095 seconds, Fetched 1 row(s)
```

* 字符串反转函数：reverse

语法：reverse(string A)

返回值： string

说明：返回字符串A的反转结果

例子：
```mysql
spark-sql> select reverse('abc');
cba
Time taken: 0.147 seconds, Fetched 1 row(s)
```

* 字符串连接函数：concat

语法：concat(string A，string B…)

返回值：string

说明：返回输入字符串连接后的结果，支持任意多个输入字符串

例子：
```mysql
spark-sql> select concat('a','_','c');
a_c
Time taken: 0.068 seconds, Fetched 1 row(s)
```

* 带分隔符字符串连接函数：concat_ws

语法： concat_ws(string separator, string A, string B,string C…)

返回值：string

说明：返回输入字符串连接后的结果，separator表示各个字符串间的分隔符；

常和collect_list、collect_set一起使用，想了解详细信息，可以点击hive系统函数collect_list和collect_set的应用

例子：
```mysql
spark-sql> select concat_ws('_','a','b','c');
a_b_c
Time taken: 0.142 seconds, Fetched 1 row(s)
```

 * 字符串截取函数：substr、substring

语法：substr(string A,int start)、substring(string A, int start)

返回值：string

说明：返回字符串A从start位置到结尾的字符串

例子：
```mysql
spark-sql> select substr('abc',2);
bc
Time taken: 0.11 seconds, Fetched 1 row(s)

spark-sql> select substring('abc',-1);
c
Time taken: 0.03 seconds, Fetched 1 row(s)
```

* 字符串截取函数：substr、substring

语法： substr(string A, int start, int len),substring(string A, intstart, int len)

返回值：string

说明：返回字符串A从start位置开始，长度为len的字符串

例子：
```mysql
spark-sql> select substr('abcdef',1,3);
abc
Time taken: 0.11 seconds, Fetched 1 row(s)

spark-sql> select substring('abcdef',3,6);
cdef
Time taken: 0.054 seconds, Fetched 1 row(s)
```

* 字符串转大写函数：upper、ucase

语法：upper(string A)、ucase(string A)

返回值：string

说明：返回字符串A的大写格式

例子：
```mysql
spark-sql> select upper('abc');
ABC
Time taken: 0.03 seconds, Fetched 1 row(s)

spark-sql> select ucase('abc');
ABC
Time taken: 0.156 seconds, Fetched 1 row(s)
```

* 字符串转小写函数：lower、lcase

语法：lower(string A)、lcase(string A)

返回值：string

说明：返回字符串A的小写格式

例子：
```mysql
spark-sql> select lower('ABC');
abc
Time taken: 0.193 seconds, Fetched 1 row(s)

spark-sql> select lcase('ABC');
abc
Time taken: 0.032 seconds, Fetched 1 row(s)
```

* 去空格函数：trim

语法：trim(string A)

返回值：string

说明：去除字符串两边的空格

例子：
```mysql
spark-sql> select trim(' ab ');
ab
Time taken: 0.024 seconds, Fetched 1 row(s)
```

* 左边去空格函数：ltrim

语法：ltrim(string A)

返回值：string

说明：去除字符串左边的空格

例子：
```mysql
spark-sql> select ltrim(' ab');
ab
Time taken: 0.036 seconds, Fetched 1 row(s)
```

* 右边去空格函数：rtrim

语法：rtrim(string A)

返回值：string

说明：去除字符串右边的空格

例子：
```mysql
spark-sql> select rtrim('ab ');
ab
Time taken: 0.024 seconds, Fetched 1 row(s)
```

* 分割字符串函数: split

语法：split(string str, string pat)

返回值：array

说明：按照pat字符串分割str，会返回分割后的字符串数组

例子：
```mysql
spark-sql> select split('name,address',',');
["name","address"]
Time taken: 3.046 seconds, Fetched 1 row(s)

spark-sql> select split('name,address',',')[0];
name
Time taken: 0.041 seconds, Fetched 1 row(s)
```

* 空格字符串函数：space

语法：space(int n)

返回值：string

说明：返回长度为n的字符串

例子：
```mysql
spark-sql> select concat_ws(space(1),'a','b','c');
a b c
Time taken: 0.031 seconds, Fetched 1 row(s)
```

* 重复字符串函数：repeat

语法：repeat(string str, int n)

返回值：string

说明：返回重复n次后的str字符串

例子：
```mysql
spark-sql> select repeat('abc',3);
abcabcabc
Time taken: 0.034 seconds, Fetched 1 row(s)
```

* 左补足函数：lpad

语法：lpad(string str, int len, string pad)

返回值：string

说明：将str进行用pad进行左补足到len位

例子：
```mysql
spark-sql> select lpad('abc',10,'d');
dddddddabc
Time taken: 0.035 seconds, Fetched 1 row(s)
```

* 右补足函数：rpad

语法：rpad(string str, int len, string pad)

返回值：string

说明：将str进行用pad进行右补足到len位

例子：
```mysql
spark-sql> select rpad('abc',10,'d');
abcddddddd
Time taken: 0.027 seconds, Fetched 1 row(s)
```

* 正则表达式替换函数：regexp_replace（hive里面可以用replace和regexp_replace，spark-sql只能用regexp_replace）

语法：regexp_replace(string A, string B, string C)

返回值：string

说明：将字符串A中的符合正则表达式B的部分替换为C

例子：
```mysql
spark-sql> select regexp_replace('abc','a','d');
dbc
Time taken: 0.025 seconds, Fetched 1 row(s)

spark-sql> select regexp_replace('abcd','a|c','f');
fbfd
Time taken: 0.029 seconds, Fetched 1 row(s)
```

* URL解析函数：parse_url

语法：parse_url(string urlString, string partToExtract [stringkeyToExtract])

返回值：string

说明：返回URL中指定的部分。partToExtract的有效值为：HOST, PATH, QUERY, REF, PROTOCOL, AUTHORITY, FILE, and USERINFO.

例子：
```mysql
spark-sql> select parse_url('https://blog.csdn.net/weixin_40267121/article/details/119185340?spm=1001.2014.3001.5501','HOST');
blog.csdn.net
Time taken: 0.107 seconds, Fetched 1 row(s)

spark-sql> select parse_url('https://blog.csdn.net/weixin_40267121/article/details/119185340?spm=1001.2014.3001.5501','PATH');
/weixin_40267121/article/details/119185340
Time taken: 0.424 seconds, Fetched 1 row(s)

spark-sql> select parse_url('https://blog.csdn.net/weixin_40267121/article/details/119185340?spm=1001.2014.3001.5501','QUERY','spm');
1001.2014.3001.5501
Time taken: 0.256 seconds, Fetched 1 row(s)
```

* json解析函数：get_json_object

语法：get_json_object(string json_string, string path)

返回值：string

说明：解析json的字符串json_string,返回path指定的内容。如果输入的json字符串无效，那么返回NULL。

例子：
```mysql
spark-sql> select get_json_object('{"data":{"age":10}}','$.data.age');
10
Time taken: 0.028 seconds, Fetched 1 row(s)
```


# HiveSQL常用语句

## 数据加载清理与建表
```mysql
--创建外部表
CREATE EXTERNAL TABLE IF NOT EXISTS stocks (
  exchange        STRING,
  symbol          STRING,
  price_open      FLOAT,
  price_close     FLOAT,
  volume          INT)
COMMENT 'this is stocks table'  --表描述/注释信息
ROW FORMAT DELIMITED FIELDS TERMINATED BY '|'  --表数据分隔符，这里的分隔符只能是一个分隔符
LINES TERMINATED BY '\n'  --行分隔符'\n'，一般缺省，一般均用\n作为换行
stored as textfile  --存储格式
LOCATION '/data/stocks';

--多分隔符使用MuLtiDelimitSerDe
create external table t(id INT,name STRING)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.MultiDelimitSerDe'
WITH SERDEPROPERTIES("field.delim"="!^")
LOCATION '/user/hive/warehouse/t';

--Load加载时一般先创建临时表textfile存储格式
DROP TABLE IF EXISTS DB_NAME.TABLE_NAME;
CREATE TABLE IF NOT EXISTS DB_NAME.TABLE_NAME(
COLUMN1 STRING,
COLUMN2 VARCHAR(10),
PT_DT   STRING,
C_TEMP  STRING   --文件加载一般可给临时表最后多加一个temp字段，用于解决文本有行尾分隔符，或者有误操作多字段数据等情况
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.MultiDelimitSerDe'
WITH SERDEPROPERTIES("field.delim"="!^")  --指定分隔符
STORED AS TEXTFILE;  --指定存储格式

--复制表结构创建表
CREATE [EXTERNAL] TABLE IF NOT EXISTS mydb.employees_temp
LIKE mydb.employees
LOCATION '/path/to/data';

--将内部表转换为外部表
alter table log4 set tblproperties(
'EXTERNAL' = 'TRUE'
);
alter table log4 set tblproperties(
'EXTERNAL' = 'false'
);
alter table log4 set tblproperties(
'EXTERNAL' = 'FALSE'
);

--创建分区表
CREATE TABLE employees (
  name         STRING,
  salary       FLOAT,
)
comment '员工表（标注释）'
PARTITIONED BY (PT_DT STRING COMMENT '分区日期');

--标准使用创建分区表格式，其中tblproperties是表属性，设置压缩格式等
DROP TABLE IF EXISTS DB_NAME.TABLE_NAME;  --建表先删除再创建
CREATE TABLE IF NOT EXISTS DB_NAME.TABLE_NAME
(
H_ROWKEY STRING COMMENT 'rowkey',
ID STRING COMMENT 'id',
BRH_NO VARCHAR(5) COMMENT '机构号',
ST_DT VARCHAR(10) COMMENT '开始日期',
END_DT VARCHAR(10) COMMENT '结束日期',
MD5 VARCHAR(32) COMMENT 'md5值'
)
COMMENT '表描述/注释信息'
PARTITIONED BY(PT_DT STRING COMMENT '分区日期')
STORED AS PARQUET
TBLPROPERTIES('PARQUET.COMPRESSION'='SNAPPY');

--load加载一般加载到创建的textfile格式的temp表
--load数据加载，从本地
LOAD DATA LOCAL INPATH '/nas/temp/teble_name_file' overwrite into table db_name.table_name;
--load数据加载，从本地加载到分区表，overwrite可根据情况缺省，缺省时为追加加载
LOAD DATA LOCAL INPATH '/nas/temp/teble_name_file' overwrite into table db_name.table_name
PARTITION (pt_dt='2020-01-01');

--load数据加载，从hdfs
LOAD DATA INPATH '/user/user_name/teble_name_file' overwrite into table db_name.table_name;
--load数据加载，从hdfs加载到分区表，overwrite可根据情况缺省，缺省时为追加加载
LOAD DATA INPATH '/user/user_name/teble_name_file' overwrite into table db_name.table_name;
PARTITION (pt_dt='2020-01-01');

--将一个表的数据插入另一个表
insert into table table1 select * from table2;

--INSERT OVERWRITE插入
--会覆盖该表已有的数据
--针对非分区表，相当于truncate再insert
--针对分区表，相当于truncate partitions再insert，涉及的partition都会进行truncate
INSERT OVERWRITE TABLE employees
PARTITION (pt_dt='2020-01-01')
SELECT * FROM staged_employees se
WHERE pt_dt='2020-01-01';
INSERT OVERWRITE TABLE TABLE_NAME SELECT * FROM TABLE_NAME WHERE where_statement;

--INSERT INTO插入
--追加数据到该表，不会影响该表已有的数据
INSERT INTO TABLE TABLE_NAME partition(col_name='col_value') column1,column2,…… FROM OTHER_TABLE_NAME;

--INSERT INTO values
insert into table_name values(1,'value1');
insert into table_name values(2,'value2');

--分区表一般设置如下两个属性，开启动态分区
set hive.exec.dynamic.partition = true;
set hive.exec.dynamic.partition.mode = nonstrict;

--动态分区加载（动态分区加载必须开启动态分区）
INSERT OVERWRITE TABLE ${DB_NAME}.TABLE_NAME PARTITION(PT_DT)
SELECT
CONCAT(SUBSTR(COALESCE(TRIM(KEHUZH),''),1,2),
COALESCE(END_DT,'')),
END_DT AS PT_DT
FROM ${DB_NAME}.TABLE_NAME_H
WHERE END_DT>='{load_time}';

--数据归档
INSERT INTO TABLE ${DB_NAME}.TABLE_NAME PARTITION(PT_DT)
SELECT ST_DT,
END_DT,
CASE WHEN END_DT<='2018-12-31' THEN CONCAT(SUBSTR(END_DT,1,4),'-01-01')
ELSE CONCAT(SUBSTR(END_DT,1,7),'-01')
END AS PT_DT
FROM ${DB_NAME}.TABLE_NAME_TEMP
WHERE END_DT<='2020-06-30'
AND END_DT>=CONCAT(SUBSTR('${load_time}',1,4),'-01-01')
AND END_DT<=CONCAT(SUBSTR('${load_time}',1,4),'-12-31');

--导出数据
INSERT OVERWRITE LOCAL DIRECTORY '/tmp/ca_employees'
SELECT name, salary, address
FROM employees
WHERE se.state = 'CA';
--Hive可以直接将查询结果insert到hdfs目录或者本地目录
INSERT OVERWRITE DIRECTORY '/tmp/hdfs_out' SELECT a.* FROM invites a WHERE a.ds='<DATE>';
INSERT OVERWRITE LOCAL DIRECTORY '/tmp/local_out' SELECT a.* FROM pokes a;

--删除表
DROP TABLE IF EXISTS table_name;
--修改表名
ALTER TABLE table_name RENAME TO table_new_name;

--修改列
alter table table_name change column ip myip String;
alter table 表名 change column 字段名 新字段名 字段类型 [描述信息];
--修改列（使用after关键字，将修改后的字段放在某个字段后）
ALTER TABLE log_messages
CHANGE COLUMN hms hours_minutes_seconds INT
COMMENT 'The hours, minutes, and seconds part of the timestamp'
AFTER severity;
--使用first关键字，将修改的字段调整到第一个字段
alter table table_name change column ip myip int comment 'this is myip' first;

--增加列（使用add columns,后面跟括号，括号里是要加入的字段及字段描述，多个字段用逗号分开）
ALTER TABLE log_messages ADD COLUMNS (
app_name STRING COMMENT 'Application name',
session_id LONG COMMENT 'The current session id');

--删除 替换列（使用replace columns,后面跟括号，括号里是要删除的字段，多个字段间用逗号）
ALTER TABLE log_messages REPLACE COLUMNS (
hours_mins_secs INT COMMENT 'hour, minute, seconds from timestamp',
severity STRING COMMENT 'The message severity'
message STRING COMMENT 'The rest of the message');
alter table log4 replace columns(x int,y int);

--添加分区
ALTER TABLE table_name ADD PARTITION(partCol='value1') location '/user/hive/warehouse/table_name';
--删除分区
ALTER TABLE table_name DROP IF EXISTS PARTITION(PT_DT='2008-08-08');
--修改分区
ALTER TABLE table_name PARTITION(partCol='value1') set location 'new location';
```

## 表检索与表结构查询

```mysql
--数据库查询与切换
show databases like 'bdpd.*' 
use database
--表清单查看
show tables 
--模糊匹配查找表
show tables 'empl.*' --employees

--查看表分区
SHOW PARTITIONS employees;

--查看建表语句，查看分区字段
show create table table_name;

--查看表信息
DESCRIBE employees;
--查看表扩展信息，可以显示表所在hdfs路径，快速获取表数据条数numRows，分区个数numPartitions，存储压缩格式等
--describe extended table_name可以快速得到表的数据总量，在数据验证中非常实用
describe extended mydb.tablename;

--数据验证时，对于简单的不需要聚合的类似SELECT<COL> FROM <TABLE> LIMIT N语句，不需要起MapReduce job，直接通过Fetch task查取数据
set hive.fetch.task.conversion=more;
```

# HiveSQL行转列、列转行

## 行转列（数据透视）

### 概念说明：
行转列，也称为数据透视，是一种将行中的数据按照某个或多个字段进行分组，并将其他字段的不同值转换为列头的操作。这种操作通常用于数据汇总和分析，以便从不同的维度观察数据。

### 为什么要用到它：

* 数据分析：便于从不同维度比较和分析数据，比如按产品、地区、时间等维度查看销售额。
* 报表生成：生成交叉表或报表时，需要将某些行数据转换为列，以满足特定的展示需求。

### 使用原则：

*明确分组字段：确定哪些字段作为行头（分组依据）。
*确定转换字段：选择哪些字段的值需要转换为列头。
*使用聚合函数：由于可能有多个值需要转换到同一列下，通常需要使用SUM()、MAX()等聚合函数来合并这些值。

### 使用规则：

*确保转换后的列头在查询前是已知的，或者可以通过某种方式动态生成（尽管这通常更复杂）。
*注意处理可能的空值或缺失数据，确保转换后的数据准确无误。

### 例子：

| sale_id | product | region | sales_amount |  
|---------|---------|--------|--------------|  
| 1       | Apple   | East   | 100          |  
| 2       | Banana  | East   | 150          |  
| 3       | Apple   | West   | 80           |  
| 4       | Cherry  | West   | 120          |  
| 5       | Apple   | East   | 110          |  
| 6       | Banana  | East   | 160          |  
| 7       | Cherry  | West   | 130          |


```mysql
SELECT  
  region,  
  year,  
  SUM(CASE WHEN product_id = 'A' THEN sales_amount ELSE 0 END) AS product_A_sales,  
  SUM(CASE WHEN product_id = 'B' THEN sales_amount ELSE 0 END) AS product_B_sales,  
  SUM(CASE WHEN product_id = 'C' THEN sales_amount ELSE 0 END) AS product_C_sales  
FROM  
  sales  
GROUP BY  
  region,  
  year;
```

| region | year | A_sales | B_sales | C_sales |  
|--------|------|---------|---------|---------|  
| East   | 2021 | 100     | 150     | 0       |  
| West   | 2021 | 80      | 0       | 120     |  
| East   | 2022 | 110     | 160     | 0       |  
| West   | 2022 | 0       | 0       | 130     |


## 列转行（数据逆透视）

### 概念说明：
列转行，也称为数据逆透视，是一种将列中的数据转换为行的操作。这种操作通常用于将汇总或宽表数据转换为更细粒度的长表数据，以便进行进一步的分析或处理。

### 为什么要用到它：

* 数据规范化：将宽表（多列）转换为长表（多行），符合数据库设计的规范化原则。
* 便于处理：在某些情况下，长表数据更容易通过编程或查询语言进行处理和分析。

### 使用原则：

* 确定转换列：选择哪些列的数据需要转换为行。
* 生成新列：如果需要，生成新的列来标识转换后的行数据（如产品ID）。
* 处理重复数据：如果原始数据中有重复行，确保在转换过程中正确处理。

### 使用规则：

* 确保转换后的数据保持一致性和准确性。
* 考虑到性能问题，特别是在处理大量数据时，评估转换操作的效率。

### 例子：

| region | Apple_sales | Banana_sales | Cherry_sales |  
|--------|-------------|--------------|--------------|  
| East   | 210         | 310          | 0            |  
| West   | 80          | 0            | 250          |

```mysql
SELECT region, 'A' AS product_id, product_A_sales AS sales_amount  
FROM sales_summary  
UNION ALL  
SELECT region, 'B' AS product_id, product_B_sales AS sales_amount  
FROM sales_summary  
UNION ALL  
SELECT region, 'C' AS product_id, product_C_sales AS sales_amount  
FROM sales_summary;
```

| region | product_id | sales_amount |  
|--------|------------|--------------|  
| East   | A          | 210          |  
| East   | B          | 310          |  
| East   | C          | 0            |  
| West   | A          | 80           |  
| West   | B          | 0            |  
| West   | C          | 250          |
