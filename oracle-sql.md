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
2.REGEXP_SUBSTR函数格式如下：

	function REGEXP_SUBSTR(String, pattern, position, occurrence, modifier)
	__srcstr     ：需要进行正则处理的字符串
	__pattern    ：进行匹配的正则表达式
	__position   ：起始位置，从第几个字符开始正则表达式匹配（默认为1）
	__occurrence ：标识第几个匹配组，默认为1
	__modifier   ：模式（'i'不区分大小写进行检索；'c'区分大小写进行检索。默认为'c'。）
