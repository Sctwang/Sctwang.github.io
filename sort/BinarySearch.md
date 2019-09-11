## BinarySearch 二分查找算法

~~~java
	public int binarySearch(int[] A, int n, int val) {
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
~~~

