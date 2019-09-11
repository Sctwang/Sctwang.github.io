## InsertSort 插入排序

~~~java
	// 选择排序的核心思想是把一个待排序序列，分成 2 部分，前半部分为有序序列，后半部分为无序序列，遍历后半部分数据，插入到前半部分已经排序好的序列，最终得到一个有序序列。
    public static int[] doInsertSort(int[] arr) {
        int target;
        // 假定第一个元素被放到了正确的位置上
        // 这样，仅需遍历1 ~ n-1
        for (int i = 1; i < arr.length; i++) {
            target = arr[i];
            while (i > 0 && target < arr[i - 1]) {
                arr[i] = arr[i - 1];
                i--;
            }
            arr[i] = target;
        }
        return arr;
    }
~~~

