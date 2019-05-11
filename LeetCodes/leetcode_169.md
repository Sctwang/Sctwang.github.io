```java
/**
 * @Author: wyz
 * @Date: 2019/5/11 22:07
 * @Version 1.0
 */

import java.util.Arrays;

/**
 * 给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。
 *
 * 你可以假设数组是非空的，并且给定的数组总是存在众数。
 *
 * 示例 1:
 *
 * 输入: [3,2,3]
 * 输出: 3
 * 示例 2:
 *
 * 输入: [2,2,1,1,1,2,2]
 * 输出: 2
 */

public class leetcode_169 {
    public static void main(String[] args) {
        int[] arr = {1,2,3,4,5,6,7,7,7,7,7,7,7};
        int i = majorityElement(arr);
        int j = majorityElement_API(arr);
        System.out.println(i);
        System.out.println(j);
    }

    public static int majorityElement(int[] nums) {
        int count = 1;
        int n = nums[0];
        for (int i = 1; i < nums.length; i++) {
            if (n == nums[i]) {
                count++;
            } else {
                count--;
                if (count == 0) {
                    n = nums[i + 1];
                }
            }
        }
        return n;
    }

    //该数出现次数大于n/2, 最简答的就是先排序，再返回中间的数
    public static int majorityElement_API(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length / 2];
    }
}

```