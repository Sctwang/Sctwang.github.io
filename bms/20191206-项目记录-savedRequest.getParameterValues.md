## 20191206 - 项目记录 - savedRequest.getParameterValues

- 背景描述：重定向 `return "redirect:/oauth/authorize?response_type=xxx&client_id=xxx&redirect-uri=url&user=" + uid;`其中 uid 为工号，在 /login 方法中需要获取 uid 的值，来判断此用户是否存在。

- 问题描述：当 url 的值为 `http://localhost:4200/#/permission-view/permission-view-list/0`的时候，无法获取到 uid

- 解决：打断点可以看到，重定向地址为：`/oauth/authorize?response_type=xxx&client_id=xxx&redirect-uri=url`，其中并无 user 字段。当修改 url 的为：`http://localhost:4200`，就可以获取到 uid 了。

- 原因：

  ~~~java
  SavedRequest savedRequest = (SavedRequest) request.getSession().getAttribute("SPRING_SECURITY_SAVED_REQUEST");
  
  uid = savedRequest.getParameterValues("user") != null ? savedRequest.getParameterValues("user")[0].toString() : null;
  ~~~

具体的还需要进一步看源码，看这两句代码是怎么获取的 uid。