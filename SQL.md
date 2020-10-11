# ***Oracle***

## 1.Oracle大小写问题:

感觉上Oracle是不区分大小写的，但实际Oracle是对大小写敏感的，只是查询时Oracle自动将我们的小写转换为了大写

## 2.Oracle批量插入:

INSERT ALL 

INTO TABLE_1(id,name,sex) VALUES(1,'HELLO',0)

INTO TABLE_2(id,dept,student_no) VALUES(2,'2018',‘stu001’)

SELECT 1 FROM DUAL

## 3.日期格式与范围:

#### 格式

***yyyy-mm-dd hh24:mi:ss***

#### 大小比较

***请保证传入的参照日期个格式与数据库一致***

例:

*如果数据库内时间格式为**2020-08-26 21:00:00**,请保证传入的参照日期的格式也是**yyyy-mm-dd hh24:mi:ss***







# ***MySql***

1.JSON_EXSTRACT(COLUMN,'$.key') 去除来的是带引号的内容

2.日期格式:%Y-%m-%d %H%i%s

3.字符串转日期:

STR_TO_DATA(date,formater)

