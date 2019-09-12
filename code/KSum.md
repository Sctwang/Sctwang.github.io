## KSum

求包含 n 个整数的数组和一个目标值 target ，判断 nums 中是否存在 n 个元素，使得 n 个元素之和的值与 target 相等？找出所有满足条件且不重复的组合。

- 例如：

    > 给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。
    >
    > 来源：力扣（LeetCode）</br>
    > 链接：https://leetcode-cn.com/problems/4sum</br>
    > 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。</br>

- 方法来自于：

  作者：mrxiong</br>
  链接：https://leetcode-cn.com/problems/4sum/solution/kshu-zhi-he-de-tong-yong-mo-ban-by-mrxiong/</br>
  来源：力扣（LeetCode）</br>
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。</br>

~~~java

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author wyz
 * @date 2019/9/12 17:30
 */

public class K_Sum_Recursive {
    /**
     * 我是一个接口，在系统提供的他们的方法里面调用我即可
     * <p>
     * 相当加了一层包装，对外提供了一个系统可以使用的接口
     *
     * @param nums   系统给定的数组
     * @param target 系统要求的目标值
     * @return 系统要求的返回值
     */
    public List<List<Integer>> kSum(int[] nums, int target, int k) {
        // 先排序，这个是必须的。
        Arrays.sort(nums);

        // 根据模板方法的要求，将该方法需要的输入都准备好。
        int[] stack = new int[k];
        Arrays.fill(stack, 0x3f3f3f3f);
        int stack_index = -1;
        int begin = 0;
        // 递归开始
        List<List<Integer>> ans = K_Sum_Recursive_Template(nums, stack, stack_index, k, begin, target);
        // 递归结束，返回解
        return ans;
    }

    /**
     * K数之和问题的模板方法，所有K数问题都可以调用这个方法
     *
     * @param nums        输入的数组
     * @param stack       定义的一个长度为 k_sum 问题中的 k 的数组，初始化为0x3f3f3f3f
     * @param stack_index 栈指针，初始化值为-1
     * @param k           表明当前问题被 分解/递归 成了 k数之和 的问题
     * @param begin       当前问题要固定的值的起点
     * @param target      当前 k数之和 的目标和
     * @return 当前 k数之和 的解集，要在上一层合并到最终解集里去
     */
    private List<List<Integer>> K_Sum_Recursive_Template(int[] nums, int[] stack, int stack_index, int k, int begin, int target) {
        List<List<Integer>> ans = new ArrayList<>();

        // 当递归到两数之和的时候，不再进行递归，直接使用左右指针法解决
        if (k == 2) {
            List<Integer> temp_ans;

            int left = begin;
            int right = nums.length - 1;

            while (left < right) {
                if (nums[left] + nums[right] > target) {
                    // 过大，因此右指针左移
                    right--;
                } else if (nums[left] + nums[right] < target) {
                    // 过小，因此左指针右移
                    left++;
                } else {
                    // 相等，加入序列中，左右指针同时向内移动一次
                    temp_ans = new ArrayList<>();
                    stack[++stack_index] = nums[left];
                    stack[++stack_index] = nums[right];

                    // 当前栈中的元素符合题目要求， 将其加入到List中去，并将该List加入到当前问题的解集中
                    for (int i = 0; i <= stack_index; i++) {
                        temp_ans.add(stack[i]);
                    }
                    ans.add(temp_ans);

                    // 栈的清理工作，其实不做也可以，因为栈指针属于形参，不会影响外面的那个栈指针，
                    // 但是还是清理掉比较好，方便调试。
                    stack[stack_index--] = 0x3f3f3f3f;
                    stack[stack_index--] = 0x3f3f3f3f;

                    left++;
                    right--;
                    while (left < right && nums[left] == nums[left - 1]) {
                        left++;
                    }
                    while (right > left && right < nums.length - 1 && nums[right] == nums[right + 1]) {
                        right--;
                    }
                }
            }
        } else {
            int target_temp;
            for (int i = begin; i < nums.length - k + 1; i++) {
                if (i > begin && nums[i] == nums[i - 1]) {
                    continue;
                }
                // 在固定一个数后，问题被降级为一个 k - 1 数之和 问题
                // 确定 k - 1 数之和 的目标和
                target_temp = target - nums[i];
                // 将当前选定的数字压入栈中，便于最后加入解集中
                stack[++stack_index] = nums[i];
                // 递归调用 k - 1 数之和 问题的求解
                List<List<Integer>> ans_temp = K_Sum_Recursive_Template(nums, stack, stack_index, k - 1, i + 1, target_temp);
                // 在选定当前数字的情况下， k - 1 数之和 问题求解完毕，
                // 将该数弹出栈，为选择下一个被选值做准备
                stack[stack_index--] = 0x3f3f3f3f;
                // 将当前解集加入当前 k数之和的解集中
                ans.addAll(ans_temp);
            }
        }
        return ans;
    }

    public static void test() {
        K_Sum_Recursive solution = new K_Sum_Recursive();
        int[] input = {0, 0, 0, 0};
        int k = 4;
        int target = 0;
        solution.kSum(input, target, k);
    }
}

~~~

