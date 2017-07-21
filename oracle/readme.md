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
