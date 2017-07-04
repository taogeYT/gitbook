## oracle note
### 1.oracle 安装
1.表信息查询
    查看用户下所有的表 
    SELECT * from user_tables;
    查看名称包含log字符的表
    select object_name,object_id from user_objects where instr(object_name,'LOG')>0; 
