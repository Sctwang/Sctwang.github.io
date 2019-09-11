## BubbleSort 冒泡排序

<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/sort.jpg"/></div>


- 冒泡实现：

~~~java
	public static int[] doBubbleSort(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - 1 - i; j++) {
                if (arr [j] > arr[j + 1]) {
                    int temp = arr[j + 1];
                    arr[j + 1] = arr[j];
                    arr[j] = temp;
                }
            }
        }
        return arr;
    }
~~~

