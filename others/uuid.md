## UUID

> UUID 唯一缺陷在于生成的结果串较长，而且无序。

- 介绍：
  
  - UUID 是 通用唯一识别码（Universally Unique Identifier）的缩写，是一种软件建构的标准。其目的，是让分布式系统中的所有元素，都能有唯一的辨识信息，而不需要通过中央控制端来做辨识信息的指定。
  
- 定义

  - UUID 是由一组 32 位数的 16 进制数字所构成，所以 UUID 理论上的总数为 16^32=2^128，约等于 3.4 x 10^38。也就是说若每纳秒产生 1 兆个 UUID，要花 100 亿年才会将所有 UUID 用完。

    UUID 的标准型式包含 32 个 16 进制	数字，以连字号分为五段，形式为 8-4-4-4-12 的 32 个字符。示例：

    `550e8400-e29b-41d4-a716-446655440000`

    UUID 亦可刻意重复以表示同类。例如说微软的 COM 中，所有组件皆必须实现出 IUnknown 接口，方法是产生一个代表 IUnknown 的 UUID。无论是程序试图访问组件中的 IUnknown 接口，或是实现 IUnknown 接口的组件，只要 IUnknown 一被使用，皆会被参考至同一个 ID：00000000-0000-0000-C000-000000000046。

- 组成

  - **UUID 是指在一台机器上生成的数字，它保证对在同一时空中的所有机器都是唯一的。通常平台会提供生成的 API。按照开放软件基金会 (OSF) 制定的标准计算，用到了以太网卡地址、纳秒级时间、芯片 ID 码和随机数。**

    UUID 由以下几部分的组合：

    （1）当前日期和时间，UUID 的第一个部分与时间有关，如果你在生成一个 UUID 之后，过几秒又生成一个 UUID，则第一个部分不同，其余相同。

    （2）时钟序列。

    （3）全局唯一的 IEEE 机器识别号，如果有网卡，从网卡 MAC 地址获得，没有网卡以其他方式获得。

    UUID 的唯一缺陷在于生成的结果串会比较长。关于 UUID 这个标准使用最普遍的是微软的 GUID (Globals Unique Identifiers)。在 ColdFusion 中可以用 CreateUUID () 函数很简单地生成 UUID，其格式为：xxxxxxxx-xxxx- xxxx-xxxxxxxxxxxxxxxx (8-4-4-16)，其中每个 x 是 0-9 或 a-f 范围内的一个十六进制的数字。而标准的 UUID 格式为：xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (8-4-4-4-12)，可以从 cflib 下载 CreateGUID () UDF 进行转换。

    （4）在 hibernate（Java orm 框架）中， 采用 IP-JVM 启动时间 - 当前时间右移 32 位 - 当前时间 - 内部计数（8-8-4-8-4）来组成 UUID

- 源码

~~~java
	/**
     * Static factory to retrieve a type 4 (pseudo randomly generated) UUID.
     *
     * The {@code UUID} is generated using a cryptographically strong pseudo
     * random number generator.
     *
     * @return  A randomly generated {@code UUID}
     */
    public static UUID randomUUID() {
        SecureRandom ng = Holder.numberGenerator;

        byte[] randomBytes = new byte[16];
        ng.nextBytes(randomBytes);
        randomBytes[6]  &= 0x0f;  /* clear version        */
        randomBytes[6]  |= 0x40;  /* set to version 4     */
        randomBytes[8]  &= 0x3f;  /* clear variant        */
        randomBytes[8]  |= 0x80;  /* set to IETF variant  */
        return new UUID(randomBytes);
    }
~~~



- 实现

~~~java
	import java.util.UUID;

    /**
     * @author wyz
     * @date 2019/9/11 10:39
     */
	public static void main(String[] args) {
        UUID uuid = UUID.randomUUID();
        
        // 输出为：4aa3c6cb-d3f9-44fc-8c7a-2a5f2252b576
        System.out.println(uuid);
    }

~~~

