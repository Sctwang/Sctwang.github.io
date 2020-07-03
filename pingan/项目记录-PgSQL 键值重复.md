## 项目记录 - PgSQL 键值重复

> 问题描述： postgresql duplicate key violates unique constraint



在对数据库进行插入操作时，会提示数据库已存在这样的数据，当数据库为空时，也会提示这样的错误。

- 主键与序列不同步

原因：这可能意味着您正在使用的表中的主键序列以某种方式变得不同步，这可能是由于批量导入过程（或沿这些方向进行的操作）所致。从设计上将其称为“错误”，但从转储文件还原后，您似乎必须手动重置主键索引。无论如何，要查看您的值是否不同步，请运行以下两个命令：

```
SELECT MAX(the_primary_key) FROM the_table;   
SELECT nextval('the_primary_key_sequence');
```

删除序列：

drop sequence  <序列名字>；

例如：

drop sequence table_sequence;



创建序列：

create sequence <序列名字> start with 1;

例如：

create sequence table_sequence start with 1;





### 参考

- [Stack Overflow](https://stackoverflow.com/questions/4448340/postgresql-duplicate-key-violates-unique-constraint)

- [CSDN](https://blog.csdn.net/publishwy/article/details/10498973)