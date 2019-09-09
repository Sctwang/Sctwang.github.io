## 给 3 个布尔变量，当其中有 2 个或者 2 个以上为 true 才返回 true

### 问题
给 3 个 boolean 变量，a, b, c，当其中有 2 个或 2 个以上为 true 时才返回 true？
* 最笨的方法：
```java
boolean atLeastTwo(boolean a, boolean b, boolean c) {
    if ((a && b) || (b && c) || (a && c)) {
        return true;
    } else {
        return false;
    }
}
```
* 优雅解法 1
```java
    return a ? (b || c) : (b && c);
```

* 优雅解法 2
```java
    return (a==b) ? a : c;
```

* 优雅解法 3
```java
   return a ^ b ? c : a
```

* 优雅解法 4
```java
    return a ? (b || c) : (b && c);
```

[stackoverflow 链接]( http://stackoverflow.com/questions/3076078/check-if-at-least-two-out-of-three-booleans-are-true)