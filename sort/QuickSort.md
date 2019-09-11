## QuickSort 快速排序

> 快速排序也分为好几种，比如 双轴排序，三轴排序，中心思想都差不多。
>
> Java8 版本的默认排序为快速排序（双轴排序）

- 源码如下：

~~~java
	/**
     * Sorts the specified array into ascending numerical order.
     *
     * <p>Implementation note: The sorting algorithm is a Dual-Pivot Quicksort
     * by Vladimir Yaroslavskiy, Jon Bentley, and Joshua Bloch. This algorithm
     * offers O(n log(n)) performance on many data sets that cause other
     * quicksorts to degrade to quadratic performance, and is typically
     * faster than traditional (one-pivot) Quicksort implementations.
     *
     * @param a the array to be sorted
     */
    public static void sort(int[] a) {
        DualPivotQuicksort.sort(a, 0, a.length - 1, null, 0, 0);
    }
~~~

- 实现示例如下：

~~~java
	// 递归使用快速排序,对arr[l...r]的范围进行排序
    public static void QuickSort(int[] arr,int l,int r){
        if(l>=r) {
            return;
        }

        int p = partition(arr,l,r);
        QuickSort(arr,l,p-1);
        QuickSort(arr,p+1,r);
    }

    // 将数组通过p分割成两部分
	// 对arr[l...r]部分进行partition操作
	// 返回p, 使得arr[l...p-1] < arr[p] ; arr[p+1...r] > arr[p]
    public static int partition(int[] arr, int l, int r) {
        // 加入这一行变成随机快速排序
        swap(arr, l, (int) (Math.random() % (r - l + 1)) + l);

        int v = arr[l];
        int j = l;
        for(int i = j +1;i<=r;i++){
            if(arr[i] < v){
                j++;
                swap(arr,i,j);
            }
        }
        swap(arr,l,j);
        return j;
    }

    public static void swap(int[] arr,int i,int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
~~~

