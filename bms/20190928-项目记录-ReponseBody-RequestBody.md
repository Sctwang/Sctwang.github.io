## 0928

### 注解

> 此处应注意还有另外一个注释 @PathVariable

- @RequestParam
- @RequestBody
  - 该注解用于读取 Request 请求的 body 部分数据，使用系统默认配置的 HttpMessageConverter 进行解析，然后把相应的数据绑定到要返回的对象上；
  - 再把 HttpMessageConverter 返回的对象数据绑定到 controller 中方法的参数上。
- @ReponseBody
  - 该注解用于将 Controller 的方法返回的对象，通过适当的 HttpMessageConverter 转换为指定格式后，写入到 Response 对象的 body 数据区。

参考：

- [Spring MVC 之 @RequestBody, @ResponseBody 详解链接](https://blog.csdn.net/kobejayandy/article/details/12690555)



### 逻辑

- ```java
  @ApiOperation("根据角色id和permissionId,删除角色的api权限")
  ```

- 实现：只需要删除角色和权限中间表存在的关系就行。中间表有 3 个字段 ID， ROLE_ID（角色 ID）， PERMISSION_ID（权限 ID）

