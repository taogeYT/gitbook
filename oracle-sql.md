## oracle note

### 1.oracle 安装

1. 表信息查询
```python
# 导入sys
import sys
```

2.查询
```
SELECT \* from user\_tables;  
查看名称包含log字符的表  
select object\_name,object\_id from user\_objects where instr\(object\_name,'LOG'\)&gt;0;
```


