## 1008-项目记录-PERMISSION_MISMATCH



问题描述：

- Postman 测试的时候提示权限不匹配，如图：

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20191008153722.png"/></div>
- 对应的数据表和 Redis 只有部分缓存，如图：

数据表：

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20191008154123.png"/></div>


Redis:

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20191008154228.png"/></div>

- 平台文档解决方案：

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20191008154354.png"/></div>



- 问题：

当项目里没有刷新权限的接口的时候，怎么解决？

//TODO

- 按照平台文档解决的话需要再手写一个权限服务来刷新。
- 本项目采用的是启动服务自动注册，按照文档，pom 文件加上依赖，启动类加上注解后启动，iam_permission 会自动注册服务