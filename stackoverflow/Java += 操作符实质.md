[GitHubStackOverflow](https://github.com/giantray/stackoverflow-java-top-qa)



~~~java
package stackoverflow;

/**
 * Java += 操作符实质:i += j，实际是等同于 i= (type of i) (i + j)
 * 对复合赋值表达式来说，E1 op= E2 (诸如 i += j; i -= j 等等)，其实是等同于 E1 = (T)((E1) op (E2))，其中，T是E1这个元素的类型。
 *
 * @author wyz
 * @date 2019/9/5 15:59
 */
public class I_J {
    public static void main(String[] args) {
        int i = 5;
        long j = 8;
        short k = 2;
        i += j;
        j += k;
        // i = i + j;
        System.out.println(i);
        // 判断i的类型 : Integer
        System.out.println(getType(i));

        System.out.println(j);
        // 判断j的类型 : Long
        System.out.println(getType(j));
    }


    /**
     * 返回对象的类型
     * @param object
     * @return
     */
    public static String getType(Object object) {
        return object.getClass().toString();
    }
}

~~~

