```java
/**
 * @Author: wyz
 * @Date: 2019/5/11 18:05
 * @Version 1.0
 */

import java.util.Arrays;

/**
 * 给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。
 *
 * 示例 1:
 *
 * 输入: "Let's take LeetCode contest"
 * 输出: "s'teL ekat edoCteeL tsetnoc"
 */


public class leetcode_557 {
    //先把字符串用空格分割开，剩下的是各个小字符串（类似于每个单词一组）
    public String reverseWords(String s) {
        String[] words = s.split(" ");
        StringBuilder str = new StringBuilder();
        for(String word : words) {
            str.append(swapWord(0, word.length()-1, word.toCharArray())).append(" ");
        }

        return str.toString().trim();
    }

    public String swapWord(int s, int e, char[] c) {
        if(s >= e) {
            return String.valueOf(c);
        }

        char temp = c[s];
        c[s] = c[e];
        c[e] = temp;
        return swapWord(s+1, e-1, c);
    }
}
```