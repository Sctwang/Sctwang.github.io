```java
package com.mortre.leetcode;
/**
 * @Author: wyz
 * @Date: 2019/5/10 17:37
 * @Version 1.0
 */

/**
 * 给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
 *
 * 说明：
 *
 * 你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？
 *
 * 示例 1:
 *
 * 输入: [2,2,1]
 * 输出: 1
 * 示例 2:
 *
 * 输入: [4,1,2,1,2]
 * 输出: 4
 */
public class leetcode_136 {
    public static void main(String[] args) {
        int[] arr = {4,1,2,1,2};
        System.out.println(singleNumber(arr));
    }


    /**
     * 根据异或的性质 任何一个数字异或它自己都等于 0，得到这个数字二进制形式中任意一个为 1 的位都是我们要找的那一位。
     * 再然后，以这一位是 1 还是 0 为标准，将数组的 n 个元素分成两部分。
     *
     * 将这一位为 0 的所有元素做异或，得出的数就是只出现一次的数中的一个
     *
     * 将这一位为 1 的所有元素做异或，得出的数就是只出现一次的数中的另一个。
     *
     * 这样就解出题目。忽略寻找不同位的过程，总共遍历数组两次，时间复杂度为O(n)。
     * @param nums
     * @return
     */

    public static int singleNumber(int[] nums) {
        int sum = nums[0];
        for (int i = 1; i < nums.length; i++) {
            sum ^= nums[i];
        }
        return sum;
    }
}

```