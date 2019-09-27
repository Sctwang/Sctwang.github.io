## 欧几里得算法


> 欧几里德算法算法又称辗转相除法，是指用于计算两个正整数a, b 的最大公约数

- 具体实现如下：


~~~java
	public static int gcd(int m, int n) {
        if (n == 0) {
            return m;
        }
        int r = m % n;
        return gcd(n, r);
    }
~~~



---



