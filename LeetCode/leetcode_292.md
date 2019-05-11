```java
/**
 * @Author: wyz
 * @Date: 2019/5/11 17:37
 * @Version 1.0
 */

/**
 * 你和你的朋友，两个人一起玩 Nim 游戏：桌子上有一堆石头，每次你们轮流拿掉 1 - 3 块石头。 拿掉最后一块石头的人就是获胜者。你作为先手。
 *
 * 你们是聪明人，每一步都是最优解。 编写一个函数，来判断你是否可以在给定石头数量的情况下赢得游戏。
 *
 * 示例:
 *
 * 输入: 4
 * 输出: false
 * 解释: 如果堆中有 4 块石头，那么你永远不会赢得比赛；
 *      因为无论你拿走 1 块、2 块 还是 3 块石头，最后一块石头总是会被你的朋友拿走。
 */

public class leetcode_292 {
    public static void main(String[] args) {
        int m = 3, n = 6;
        System.out.println(canWinNim_test(n, m));

    }

    //如果堆中石头的数量 n 不能被 4 整除，那么你总是可以赢得 Nim 游戏的胜利。
    public boolean canWinNim(int n) {
        return (n % 4 != 0);
    }

    //巴什博弈：只有一堆 n 个物品，两个人轮流从这堆物品中取物，规定每次至少取一个，最多取 m 个。最后取光者得胜。
    public static boolean canWinNim_test(int n, int m) {
        return (n % (m + 1) != 0);
    }
}

```