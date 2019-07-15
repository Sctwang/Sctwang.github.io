# Java 考试

## 选择题 11 道

## 填空题 5 道

## 编程题 3 道

00.输入一个正数n，输出所有和为n的连续正数序列。

~~~java
import java.util.Scanner;

/**
 * @author wyz
 * @date 2019/7/15 8:42
 *
 * 输入一个正数n，输出所有和为n的连续正数序列。
 */
public class Exam00 {
    public static void main(String[] args) {
        findSum();
    }

    public static void findSum () {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int first = 1, last = 2;
        int sum = first + last;
        while (first < last && first < (n + 1) / 2) {
            if (sum < n) {
                last++;
                sum += last;
            } else if (sum > n) {
                sum -= first;
                first++;
            } else {
                System.out.println(first + "~" + last);
                sum -= first;
                first++;
            }
        }
    }
}
~~~

01.对于一个字符串，请设计一个高效算法，找到第一次重复出现的字符。给定一个字符串(不一定全为字母)A及它的长度n。请返回第一个重复出现的字符。保证字符串中有重复字符，字符串的长度小于等于500。

~~~java
import java.util.HashSet;
import java.util.Set;
import java.util.TreeSet;

/**
 * @author wyz
 * @date 2019/7/15 9:29
 *
 * 对于一个字符串，请设计一个高效算法，找到第一次重复出现的字符。给定一个字符串(不一定全为字母)A及它的长度n。请返回第一个重复出现的字符。保证字符串中有重复字符，字符串的长度小于等于500。
 *
 */
public class Exam01 {
    public static void main(String[] args) {
        String str = "wwan23gy2322zhew";
        int n = str.length();
        System.out.println(findStr(str, n));

    }

    public static String findStr (String A, int n) {
        n = A.length();
        if (n > 500 || n < 0) {
            return "A 不符合要求";
        }

        char[] chars = A.toCharArray();

        TreeSet<String> treeSet = new TreeSet<String>();
        for (int i = 0; i < n; i++) {
            treeSet.add(String.valueOf(chars[i]));
        }
        return treeSet.first();
    }
}
~~~

02.对于一个有序数组，我们通常采用二分查找的方式来定位某一元素，请编写二分查找的算法，在数组中查找指定元素。给定一个整数数组A及它的大小n，同时给定要查找的元素val，请返回它在数组中的位置(从0开始)，若不存在该元素，返回-1。若该元素出现多次，请返回第一次出现的位置。

~~~java
import java.util.Arrays;

/**
 * @author wyz
 * @date 2019/7/15 11:15
 *
 * 对于一个有序数组，我们通常采用二分查找的方式来定位某一元素，请编写二分查找的算法，在数组中查找指定元素。给定一个整数数组A及它的大小n，同时给定要查找的元素val，请返回它在数组中的位置(从0开始)，若不存在该元素，返回-1。若该元素出现多次，请返回第一次出现的位置。
 */
public class Exam02 {
    public static void main(String[] args) {
        int[] a = {1, 5, 3, 4, 5, 6, 7, 8, 9};
        int target = binarySearch(a, 9, 10);
        System.out.println(target);
    }

    public static int binarySearch(int[] A, int n, int val) {
        Arrays.sort(A);
        n = A.length;
        int low = 0;
        int high = n - 1;
        while (low <= high) {
            int middle = (low + high) / 2;
            if (val == A[middle]) {
                return middle;
            }

            if (val > A[middle]) {
                low = middle + 1;
            }

            if (val < A[middle]) {
                high = middle - 1;
            }
        }
        return -1;
    }
}
~~~



## Maven



- SNAPSHOT 快照

- packaging 默认打的是 jar 包



生命周期(重要的几个)：

- 编译阶段compile：只编译，不打包
- 测试阶段test：单元测试
- 打包package
- 安装install
- 部署deploy



## Git

### 合并分支

>  [参考链接](https://gitee.com/help/articles/4196#article-header2)

- git checkout -b wyz    （创建 wyz 分支并切换到 wyz 分支）

- 在 wyz 分支下进行操作，git add <对应文件>

- git commit -m "合并分支测试"

- 切换到主分支 git checkout master

- 分支合并 git merge wyz
- 提交到远程 git push



### Docker

- 打标签 tag
  - docker tag 起的名字 地址
- 启动容器 docker start ID（后台启动）

- docker-compose.yaml

- 执行运行命令 ：docker-compose up -d
- 执行停止命令 ：docker-compose down -d
- 查看日志：docker logs ID

同时启动多个容器：

- docker-compose.yaml 里写多个容器的命令







