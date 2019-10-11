#### @PathVariable 和 @RequestParam 的区别

`@PathVariable`  获取的是请求路径中参数的值；接收请求路径中占位符的值

`@RequestParam` 获取的是请求参数，一般是 url 问号后面的参数值 



##### PathVariable 示例：

~~~java
@RestController
@RequestMapping("/test")
public class TestController {

    @RequestMapping("/username1")
    @ResponseBody
    public String test1(@PathVariable String username) {
        String str = "PathVariable:" + username;
        System.out.println(str);
        return str;
    }
}
~~~

访问 http://localhost:8001/test/username1

浏览器页面报错：

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190922165722.png"/></div>

控制台输出提示信息：`Resolved [org.springframework.web.bind.MissingPathVariableException: Missing URI template variable 'username' for method parameter of type String]`



访问 http://localhost:8001/test/username1?username=wyz

输出信息和访问 http://localhost:8001/test/username1 相同，原因是，PathVariable 的功能是**接收请求路径中占位符的值**



访问 http://localhost:8001/test/username1/wyz

浏览器页面输出：PathVariable:wyz

控制台输出：PathVariable:wyz



##### RequestParam 示例：

~~~java
@RestController
@RequestMapping("/test")
public class TestController {

    @RequestMapping("/username")
    @ResponseBody
    public String test(@RequestParam String username) {
        String str = "RequestParam:" + username;
        System.out.println(str);
        return str;
    }
}
~~~

访问 http://localhost:8001/test/username?username=wyz

浏览器页面输出：RequestParam:wyz

控制台输出：RequestParam:wyz



访问 http://localhost:8001/test/username

浏览器页面报错：

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190922165319.png"/></div>

控制台输出提示信息：`Resolved [org.springframework.web.bind.MissingServletRequestParameterException: Required String parameter 'username' is not present]`



- 参考：[stackoverflow](https://stackoverflow.com/questions/13715811/requestparamvspathvariable)