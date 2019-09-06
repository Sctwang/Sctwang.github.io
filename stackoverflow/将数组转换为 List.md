[GitHubStackOverflow](https://github.com/giantray/stackoverflow-java-top-qa)



~~~java
package stackoverflow;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

/**
 * 将数组转换为List
 *
 * @author wyz
 * @date 2019/9/5 16:04
 */
public class ArrayToList {

    public static void main(String[] args) {
        String[] str =  {"test", "str", "stringToList"};

        // 方法1
        // asList：Returns a fixed-size list backed by the specified array.返回指定数组支持的固定数组大小的列表
        List<String> arrayList = new ArrayList<>(Arrays.asList(str));
        System.out.println(arrayList);

        // 方法2
        // Arrays.asList(array)或者Arrays.asList(new Element(1),new Element(2),new Element(3))

        // 优缺点：
        //这样做生成的list，是定长的。也就是说，如果你对它做add或者remove，都会抛UnsupportedOperationException。
        //如果修改数组的值，list中的对应值也会改变！
        //Arrays.asList() 返回的是Arrays内部静态类，而不是Java.util.ArrayList的类。这个java.util.Arrays.ArrayList有set(),get(),contains()方法，但是没有任何add() 方法，所以它是固定大小的


        // 标准方式：
        List<String> stringArrayList = new ArrayList<>();
        Collections.addAll(stringArrayList, str);
        System.out.println(stringArrayList);
    }

}
~~~

