## 从一个多层嵌套循环中直接跳出

### 问题
Java 中如何从一个多层嵌套循环中退出，例如下面，有两个循环，break 只能退出一个 for 循环，不能直接跳过第二个 for 循环

```java
for (Type type : types) {  
    for (Type t : types2) {  
         if (some condition) {  
             // Do something and break...  
             break; // 这样只退出了最里的 for 循环  
         }  
}}  
```

### 回答

可以用 break + label 的语法，例子如下
```java
public class Test {  
  public static void main(String[] args) {  
    outerloop:  
    for (int i=0; i < 5; i++) {  
      for (int j=0; j < 5; j++) {  
        if (i * j > 6) {  
          System.out.println("Breaking");  
          break outerloop;  
        }  
        System.out.println(i + " " + j);  
      }  
    }  
    System.out.println("Done");  
  }  
}  
```

首先在 for 循环前加标签，如例子中的 outerloop，然后在 for 循环内 break label(如本例的 outerloop),就会跳出该 label 指定的 for 循环。

[stackoverflow 链接](
http://stackoverflow.com/questions/886955/breaking-out-of-nested-loops-in-java)
