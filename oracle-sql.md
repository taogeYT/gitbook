# oracle note
>### oracle 安装配置
#### 1.启动
```
lsnrctl start /lsnrctl stop
```
#### 2.创建用户
- 创建普通用户并授权brain
```sql
create user jwdn identified by password;
grant connect,resource to jwdn;
alter user jwdn default tablespace users;
```
#### 3.创建/删除表
```sql
create table test(id number,text VARCHAR2(100)) / drop table test
create T SG AS(
	SELECT A.*,B.X,B.Y from T1 A,T2 B
	WHERE A.ID = B.ID)
```
>### 查询语句
1.从表中随机抽取N条数据显示
```sql
select * from (select * from tablename order by sys_guid()) where rownum < N;
```
>### ORACLE正则匹配
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
	**REGEXP_SUBSTR函数格式如下：**
	function REGEXP_SUBSTR(String, pattern, position, occurrence, modifier)
	__srcstr     ：需要进行正则处理的字符串
	__pattern    ：进行匹配的正则表达式
	__position   ：起始位置，从第几个字符开始正则表达式匹配（默认为1）
	__occurrence ：标识第几个匹配组，默认为1
	__modifier   ：模式（'i'不区分大小写进行检索；'c'区分大小写进行检索。默认为'c'。
	**REGEXP_REPLACE函数格式如下：**
	REGEXP_REPLACE(source_char, pattern [, replace_string [, position [, occurrence [, match_parameter ] ] ] ] )
	source_char  : 搜索值的字符表达式。这通常是一个字符列，可以是任何数据类型CHAR，VARCHAR2，NCHAR，NVARCHAR2，CLOB或NCLOB。
	pattern	     : 进行匹配的正则表达式
	replace_str  : 可选。匹配的模式将替换pattern。如果省略replace_str参数，将删除所有匹配的模式，并返回结果字符串。
	
	
	2.删除前后标点符号
```
SELECT REGEXP_REPLACE(',张三，李四:', '[[:punct:]]') FROM DUAL
```
	
	
	
	
