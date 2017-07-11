### MERGE新特性(UPDATE WHERE,DELETE WHERE,INSERT WHERE)

    通过MERGE语句，根据一张表或子查询的连接条件对另外一张表进行查询，连接条件匹配上的进行UPDATE，无法匹配的执行INSERT。
    这个语法仅需要一次全表扫描就完成了全部工作，执行效率要高于INSERT＋UPDATE。 
    语法为:
    *************************************************************
    MERGE [INTO [schema .] table [t_alias] 
    USING [schema .] { table | view | subquery } [t_alias] 
    ON ( condition ) 
    WHEN MATCHED THEN merge_update_clause 
    WHEN NOT MATCHED THEN merge_insert_clause
    **************************************************************
##### 应用实例，重复的数据合并

    SELECT * FROM W;
    MERGE INTO W X
    USING (SELECT * FROM W WHERE (ID,TT) IN (SELECT ID,MIN(TT) FROM W GROUP BY ID HAVING count(id)>1)) Y
    ON (X.ID=Y.ID)
    WHEN MATCHED THEN
    UPDATE SET
    X.AA=NVL(X.AA,Y.AA),
    X.BB=NVL(X.BB,Y.BB),
    X.CC=NVL(X.CC,Y.CC),
    X.TT=NVL(X.TT,Y.TT)
    WHERE (X.ID,X.TT) IN (select id,tt from (SELECT ID,TT,row_number()over(partition by ID order by TT)num FROM W) WHERE num<=2)
    DELETE WHERE X.TT=Y.TT;
    SELECT * FROM W;
