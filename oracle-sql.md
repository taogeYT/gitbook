# oracle note
>### oracle 安装配置
#### 1.启动
```
lsnrctl start /lsnrctl stop
```
#### 2.创建用户
```sql
create user jwdn identified by password;
grant connect,resource to jwdn;
alter user jwdn default tablespace users;
```
#### 3.创建/删除表
```
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


