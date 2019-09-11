### KMP 算法

> 用来解决查找最小子串问题。相对于传统的暴力匹配算法，避免了重复比较的过程。

- file.KMP 算法是一种改进的字符串匹配算法，由 D.E.Knuth，J.H.Morris 和 V.R.Pratt 提出的，因此人们称它为克努特 — 莫里斯 — 普拉特操作（简称 file.KMP 算法）。file.KMP 算法的核心是利用匹配失败后的信息，尽量减少模式串与主串的匹配次数以达到快速匹配的目的。具体实现就是通过一个 next () 函数实现，函数本身包含了模式串的局部匹配信息。file.KMP 算法的时间复杂度 O(m+n)。

- 具体实现如下：

~~~java
/**
 * @author wyz
 * @date 2019/8/17 11:26
 */
public class KMP {

    public static void main(String[] args) {
        int i = kmpMatch("abcabaabaabcacb", "abaabcac");
        System.out.println(i);
    }


    /**
     * 对主串s和模式串t进行KMP模式匹配
     *
     * @param s 主串
     * @param t 模式串
     * @return 若匹配成功，返回t在s中的位置（第一个相同字符对应的位置），若匹配失败，返回-1
     */
    public static int kmpMatch(String s, String t) {
        char[] s_arr = s.toCharArray();
        char[] t_arr = t.toCharArray();
        int[] next = getNextArray(t_arr);
        int i = 0, j = 0;
        while (i < s_arr.length && j < t_arr.length) {
            if (j == -1 || s_arr[i] == t_arr[j]) {
                i++;
                j++;
            } else {
                j = next[j];
            }
        }
        if (j == t_arr.length) {
            return i - j;
        } else {
            return -1;
        }
    }

    /**
     * 求出一个字符数组的next数组
     *
     * @param t 字符数组
     * @return next数组
     */
    public static int[] getNextArray(char[] t) {
        int[] next = new int[t.length];
        next[0] = -1;
        next[1] = 0;
        int k;
        for (int j = 2; j < t.length; j++) {
            k = next[j - 1];
            while (k != -1) {
                if (t[j - 1] == t[k]) {
                    next[j] = k + 1;
                    break;
                } else {
                    k = next[k];
                }
                // 当k == -1而跳出循环时，next[j] = 0，否则next[j]会在break之前被赋值
                next[j] = 0;
            }
        }
        return next;
    }

}
~~~

