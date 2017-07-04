## oracle note

### 1.oracle 安装

1. 表信息查询
```python
# 导入sys
import sys
```

2. 查询
```
SELECT \* from user\_tables;  
查看名称包含log字符的表  
select object\_name,object\_id from user\_objects where instr\(object\_name,'LOG'\)&gt;0;
```

# oracle note

>### oracle 安装配置
#### 1.启动
	lsnrctl start /lsnrctl stop
#### 2.创建用户
```sql
create user jwdn identified by password;
grant connect,resource to jwdn;
alter user jwdn default tablespace users;
```
#### 3.创建/删除表
```
create table test(id number,text VARCHAR2(100)) / drop table test
create TABLE SG AS(SELECT a.*,b.经度,b."纬度" from TBL_ACDNT_RECORD a, s1 b WHERE a.ACDNT_ID = b.ACDNT_ID)
```
>### 查询语句
#### 1.从表中随机抽取N条数据显示
```
select * from (select * from tablename order by sys_guid()) where rownum < N;
```


