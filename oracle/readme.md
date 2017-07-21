[toc]
### oracle 安装配置
##### 1.启动
```
lsnrctl start /lsnrctl stop
```
##### 2.创建用户
```sql
创建普通用户并授权brain
create user jwdn identified by password;
grant connect,resource to jwdn;
alter user jwdn default tablespace users;
```
##### 3.创建/删除表
```sql
CREATE TABLE "JWDN"."TEST"(
	id number, 
	"CODE" VARCHAR2(10) DEFAULT 00000000 NOT NULL ENABLE
) / drop table test
create T SG AS(
	SELECT A.*,B.X,B.Y from T1 A,T2 B
	WHERE A.ID = B.ID)
```
##### 4.查询表的大小
有两种含义的表大小:  
一种是分配给一个表的物理空间数量，而不管空间是否被使用。可以这样查询获得字节数：

	select segment_name, bytes from user_segments where segment_type = 'TABLE';
或者

	Select Segment_Name,Sum(bytes)/1024/1024 From User_Extents Group By Segment_Name;
另一种表实际使用的空间。这样查询：

	analyze table AAA compute statistics; 
	select num_rows * avg_row_len from user_tables where table_name = 'AAA';
### 查询语句
##### 从表中随机抽取N条数据显示
```sql
select * from (select * from tablename order by sys_guid()) where rownum < N;
```
### ORACLE正则匹配

	正则表达式由标准的元字符（metacharacters）所构成： '$' 匹配输入字符串的结尾位置。
	如果设置了 RegExp 对象的 Multiline 属性，则 $ 也匹配 '\n' 或 '\r'。 
	'?' 匹配前面的子表达式零次或一次。'*' 匹配前面的子表达式零次或多次。'|' 指明两项之间的一个选择。
	例子：
	'^([a-z]+|[0-9]+)$'表示所有小写字母或数字组合成的 '( )' 标记一个子表达式的开始和结束位置。
	'{m,n}' 一个精确地出现次数范围，m=<出现次数<=n，'{m}'表示出现m次，'{m,}'表示至少出现m次。
	对所获取的匹配的引用。 
	[[:alpha:]] 任何字母。
	[[:digit:]] 任何数字。
	[[:alnum:]] 任何字母和数字。
	[[:space:]] 任何白字符。
	[[:upper:]] 任何大写字母。
	[[:lower:]] 任何小写字母。
	[[:punct:]] 任何标点符号。
	[[:xdigit:]] 任何16进制的数字，相当于[0-9a-fA-F]。
	\转义符 *, +, ?, {n}, {n,}, {n,m} 限定符^, $, anymetacharacter 位置和顺序。

##### 1.regexp_substr函数格式如下：

	function REGEXP_SUBSTR(String, pattern, position, occurrence, modifier)
	__srcstr     ：需要进行正则处理的字符串
	__pattern    ：进行匹配的正则表达式
	__position   ：起始位置，从第几个字符开始正则表达式匹配（默认为1）
	__occurrence ：标识第几个匹配组，默认为1
	__modifier   ：模式（'i'不区分大小写进行检索；'c'区分大小写进行检索。默认为'c'。

##### 2.regexp_replace函数格式如下：

	REGEXP_REPLACE(source_char, pattern, replace_string[,position[,occurrence[,match_parameter]]]])
	source_char  : 字符表达式,这通常是一个字符列，可以是任何数据类型CHAR,VARCHAR2,NCHAR,NVARCHAR2,CLOB或NCLOB
	pattern	     : 进行匹配的正则表达式。
	replace_str  : 可选，匹配的模式将替换pattern。如果省略replace_str参数，将删除所有匹配的模式，并返回结果字符串。
	position     : 可选，在字符串中的开始位置搜索。如果省略，则默认为1。
	occurrence   : 可选，是一个非负整数默认为1，指示替换操作的发生：如果指定0，那么所有出现将被替换字符串。
		如果指定了正整数n，那么将替换第n次出现。
	match_parameter : 模式（'i'不区分大小写进行检索；'c'区分大小写进行检索。默认为'c'）
	例子：
	删除前后标点符号
	SELECT REGEXP_REPLACE(',张三，李四:', '[[:punct:]]') FROM DUAL

### oracle函数
##### 1.trim 函数（trim,ltrim,rtrim）
	SELECT /*TRIM(';a;b;c;'),*/ ltrim(';a;b;c;',';'), rtrim(';a;b;c;',';') FROM dual;
	注意：无法使用TRIM(‘;a;b;c;’, ‘;’)的格式
	TRIM() 有它自己的格式
	SELECT TRIM(';' FROM ';a;b;c;'),
	       TRIM(leading ';' FROM ';a;b;c;'),
	       TRIM(trailing ';' FROM ';a;b;c;'),
	       TRIM(both ';' FROM ';a;b;c;')
	FROM dual;
##### 2.trunc()函数的用法
	日期
	1.select trunc(sysdate) from dual --2013-01-06 今天的日期为2013-01-06
	2.select trunc(sysdate, 'mm') from dual --2013-01-01 返回当月第一天.
	3.select trunc(sysdate,'yy') from dual --2013-01-01 返回当年第一天
	4.select trunc(sysdate,'dd') from dual --2013-01-06 返回当前年月日
	5.select trunc(sysdate,'yyyy') from dual --2013-01-01 返回当年第一天
	6.select trunc(sysdate,'d') from dual --2013-01-06 (星期天)返回当前星期的第一天
	7.select trunc(sysdate, 'hh') from dual --2013-01-06 17:00:00 当前时间为17:35 
	8.select trunc(sysdate, 'mi') from dual --2013-01-06 17:35:00 TRUNC()函数没有秒的精
	数字
	TRUNC（number,num_digits） 
	Number ：需要截尾取整的数字。 
	Num_digits ：用于指定取整精度的数字。Num_digits 的默认值为 0。
	TRUNC()函数截取时不进行四舍五入
	9.select trunc(123.458) from dual --123
	10.select trunc(123.458,0) from dual --123
	11.select trunc(123.458,1) from dual --123.4
	12.select trunc(123.458,-1) from dual --120
	13.select trunc(123.458,-4) from dual --0
	14.select trunc(123.458,4) from dual --123.458
	15.select trunc(123) from dual --123
	16.select trunc(123,1) from dual --123
	17.select trunc(123,-1) from dual --120