## PostgreSQL



#### 字段长度

- http://www.postgres.cn/docs/11/functions-string.html
- https://blog.csdn.net/superit401/article/details/78237780
- 对于 varchar 的类型，长度指的是字符数
  - 查询的函数：`char_length(*string*)` or `character_length(*string*)`
  - 例如： `select char_length('test');`//输出为：4





#### 查询字段数量

- 查找数据库的某一张表有多少字段：
  - `select count(*) from information_schema.COLUMNS where table_schema='public' and "tbale_name"='table_name'`

