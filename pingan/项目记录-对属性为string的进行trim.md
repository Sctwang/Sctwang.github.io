## 反射获取对象的属性，属性值，添加逻辑处理

> 需求：有很多对象，再前端页面进行编辑，在保存时对 String 类型的进行 trim 操作。



~~~java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

/**
 * @author: wyz
 * @date: 2020/6/2 15:29
 * @description: 若对象的属性为 String, 对其进行 trim() 操作
 */
public class TrimFieldValue {

    private static final Logger logger = LoggerFactory.getLogger(TrimFieldValue.class);

    public static <T> T trimValue(T t) {
        Field[] fields = t.getClass().getDeclaredFields();
        for (Field field : fields) {
            // 不加的话会抛权限异常
            // field.setAccessible(true);
            // Make the given field accessible, explicitly setting it accessible if necessary.
            ReflectionUtils.makeAccessible(field);
            try {
                foo(t, field);
            } catch (IllegalAccessException e) {
                e.printStackTrace();
                logger.info("Error message:{}", e.getMessage());
            }
        }
        return t;
    }

    private static void foo(Object t, Field field) throws IllegalAccessException {
        if ((field.get(t) instanceof String) 
            && !Modifier.isStatic(field.getModifiers())) {
            Object object = field.get(t);
            String str = (String) object;
            field.set(t, str.trim());
        }
    }
}
~~~





### 参考

- [oschina](https://my.oschina.net/ordinance/blog/1845440)
- [csdn](https://blog.csdn.net/beiouwolf/article/details/7090700)