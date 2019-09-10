## Java 8 - Hash

- Hash : 把任意长度的输入，通过散列算法，变换成固定长度的输出，该输出就是散列值。

### hash 源码

~~~java
/**
     * Computes key.hashCode() and spreads (XORs) higher bits of hash
     * to lower.  Because the table uses power-of-two masking, sets of
     * hashes that vary only in bits above the current mask will
     * always collide. (Among known examples are sets of Float keys
     * holding consecutive whole numbers in small tables.)  So we
     * apply a transform that spreads the impact of higher bits
     * downward. There is a tradeoff between speed, utility, and
     * quality of bit-spreading. Because many common sets of hashes
     * are already reasonably distributed (so don't benefit from
     * spreading), and because we use trees to handle large sets of
     * collisions in bins, we just XOR some shifted bits in the
     * cheapest possible way to reduce systematic lossage, as well as
     * to incorporate impact of the highest bits that would otherwise
     * never be used in index calculations because of table bounds.
     */
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
~~~





### hashCode 源码

~~~java
/**
     * Returns a hash code for this string. The hash code for a
     * {@code String} object is computed as
     * <blockquote><pre>
     * s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
     * </pre></blockquote>
     * using {@code int} arithmetic, where {@code s[i]} is the
     * <i>i</i>th character of the string, {@code n} is the length of
     * the string, and {@code ^} indicates exponentiation.
     * (The hash value of the empty string is zero.)
     *
     * @return  a hash code value for this object.
     */
    public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
~~~



#### 例子

~~~java
/**
 * Java 8 的 hash 算法
 * @author wyz
 * @date 2019/9/10 14:46
 */
public class Java8Hash {
    // Default to 0
    private int hash;
    
    /**
     * The value is used for character storage.
     */
    private final char[] value;

    public Java8Hash(char[] value) {
        this.value = value;
    }

    /**
     * hashCode
     * @return h
     */
    @Override
    public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char[] val = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }

    /**
     * hash
     * @param key
     * @return
     */
    static final int Java8hash(Object key) {
        int h;
        h = key.hashCode();
        int i = h >>> 16;
        System.out.println("i:" + i);
        int a = h ^ (h >>> 16);
        System.out.println("a(h >>> 16 ):" + a);
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

    public static void main(String[] args) {
        String str = "Java";
        int i = str.hashCode();
        // hashCode 值为：2301506
        System.out.println("hashCode:" + i);

        int hash = Java8hash(str);
        // hash 值为：2301537
        System.out.println("hash:" + hash);

        System.out.println("2301506 ^ 35: " + (2301506 ^ 35));
    }
}
~~~



#### 参考

- [DreamMakers](https://blog.csdn.net/majinggogogo/article/details/80260400)