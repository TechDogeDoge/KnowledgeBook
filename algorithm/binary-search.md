---
description: Lesson 2 Binary Search
---

# 二分法

## 二分法代码模板

### 九章模板

可以适用于所有类型的二分法题目，搜索`target`的位置、搜索`target`第一次出现的位置、搜索`target`最后一次出现的位置。**唯一的不同点在于当`target == nums[mid]`时，边界的更新。**`while`循环结束时，上下界刚好相差1，根据所求题目选取正确的。

```java
int[] nums;
int lo = 0;
int hi = nums.length - 1;
while(lo + 1 < hi)  {
    int mid = lo + (hi - lo) / 2;
    if (target > nums[mid]) {
        lo = mid;
    } else if (target < nums[mid]) {
        hi = mid;
    } else if (target == nums[mid]) {
    	// 两种情况：找到第一次出现的坐标(左边)或者最后一次出现的坐标(右边)
    	// 第一次出现时：
    	// hi = mid;
    	// 最后一次出现时：
    	// lo = mid;
    }
}

if (nums[lo] == target) {
    return lo;
}

if (nums[hi] == target) {
    return hi;
}
```

### labuladong模板

模板较为复杂，三种问题对应的模板区别较大，此处不赘叙。

## 例题

278 First Bad Version

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example 1:**

```
Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.
```

**Example 2:**

```
Input: n = 1, bad = 1
Output: 1
```

> 解法

直接套用九章模板，此题目相当于寻找目标第一次出现的位置

```java
public int firstBadVersion(int n) {
    int lo = 1;
    int hi = n;
    while (lo + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (isBadVersion(mid)) {
            hi = mid;
        } else {
            lo = mid;
        }
    }
    return isBadVersion(lo) ? lo : hi;
}
```

### 702 Search in a Sorted Array of Unknown Size

Given an integer array sorted in ascending order, write a function to search target in `nums`. If target exists, then return its index, otherwise return -1. However, the array size is unknown to you. You may only access the array using an `ArrayReader` interface, where `ArrayReader.get(k)` returns the element of the array at index `k` (0-indexed).

You may assume all integers in the array are less than 10000, and if you access the array out of bounds, `ArrayReader.get` will return 2147483647.

**Example 1:**

````
Input: array = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4```
````

**Example 2:**

```
Input: array = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

**Note:** You may assume that all elements in the array are unique. The value of each element in the array will be in the range \[-9999, 9999].

> 解法

直接套用九章模板，此题目相当于搜索目标的位置，由于不知数组大小，所以采用倍增法确定搜索区间上界

```java
public int search(ArrayReader reader, int target) {
    if (reader == null) return -1;
    int lo = 0;
    int hi = 1;
    while (reader.get(hi) < target) {
        lo = hi;
        hi *= 2;
    }
    while (lo + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (reader.get(mid) < target) {
            lo = mid;
        } else if (reader.get(mid) >= target) {
            hi = mid;
        }
    }

    if (reader.get(lo) == target) {
        return lo;
    }
    if (reader.get(hi) == target) {
        return hi;
    }
    return -1;
}
```

### 153. Find Minimum in Rotated Sorted Array

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

* `[4,5,6,7,0,1,2]` if it was rotated `4` times.
* `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of **unique** elements, return _the minimum element of this array_.

You must write an algorithm that runs in `O(log n) time.`

**Example 1:**

```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

**Example 3:**

```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
```

> 解法

还是采用九章模板，但是由于数组现在不是单调的，判断条件和上下界更新需要改变

两种思路：

1. 求顺序反转点pivot点的位置，当中间值小于左边界值，右界左移；当中间值大于左边界值，左界右移
2. 求顺序反转点pivot点的位置，当中间值小于`nums[0]`，右界左移；当中间值大于`nums[0]`，左界右移
3. 求顺序反转点pivot点的位置，当中间值小于`nums[length-1]`，左界右移；当中间值大于`nums[length-1]`，右界左移

```java
public int findMin(int[] nums) {
    int lo = 0;
    int hi = nums.length - 1;
    while (lo + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] > nums[lo]) {
            lo = mid;
        } else if (nums[mid] < nums[lo]) {
            hi = mid;
        }
    }
    return nums[lo] > nums[hi] ? nums[hi] : nums[0];
}

public int findMin(int[] nums) {
    int lo = 0;
    int hi = nums.length - 1;
    while (lo + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] > nums[0]) {
            lo = mid;
        } else if (nums[mid] < nums[0]) {
            hi = mid;
        }
    }
    return nums[0] > nums[hi] ? nums[hi] : nums[0];
}

public int findMin(int[] nums) {
    int lo = 0;
    int hi = nums.length - 1;
    while (lo + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] > nums[nums.length-1]) {
            lo = mid;
        } else if (nums[mid] < nums[nums.length-1]) {
            hi = mid;
        }
    }
    return nums[nums.length-1] > nums[hi] ? nums[hi] : nums[0];
}
```

### 302. Smallest Rectangle Enclosing Black Pixels

An image is represented by a binary matrix with 0 as a white pixel and 1 as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location (x, y) of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

**Example:**

```
Input:
[
  "0010",
  "0110",
  "0100"
]
and x = 0, y = 2
Output: 6
```

> 解法

![image-20211124224334244](https://c/Users/jiaoy/AppData/Roaming/Typora/typora-user-images/image-20211124224334244.png) ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191222095839209.png?x-oss-process=image/watermark,type\_ZmFuZ3poZW5naGVpdGk,shadow\_10,text\_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3NjAyNg==,size\_16,color\_FFFFFF,t\_70)

```java
class Solution {
    public int minArea(char[][] image, int x, int y) {
        int m = image.length;
        int n = image[0].length;
        int l = 0, r = 0, t = 0, b b = 0;
        // vertical (column) 
        // left
        int lo = 0;
        int hi = y;
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            if (emptyColumn(image, mid)) {
                lo = mid+1;
            } else {
                hi = mid;
            }
        }
        l = lo;
        // right
        lo = y;
        hi = n - 1;
        while (lo < hi) {
            int mid = lo + (hi - lo + 1) / 2;
            if (emptyColumn(image, mid)) {
                hi = mid - 1;
            } else {
                lo = mid;
            }
        }
        r = lo;
        // horizontal (row)
        // top
        lo = 0;
        hi = x;
        while (lo + 1 < hi) {
            int mid = lo + (hi - lo) / 2;
            if (emptyRow(image, mid)) {
                lo = mid;
            } else {
                hi = mid;
            }
        }
        t  = (emptyRow(image, lo)) ? hi:lo;
        // bottom
        lo = x;
        hi = m - 1;
        while (lo + 1 < hi) {
            int mid = lo + (hi - lo) / 2;
            if (emptyRow(image, mid)) {
                hi = mid;
            } else {
                lo = mid;
            }
        }
        b = (emptyRow(image, hi)) ? lo:hi;
        return (b - t + 1) * (r - l + 1);
    }
    
    public boolean emptyRow(char[][] image, int num) {
        for (char tmp : image[num]) {
            if (tmp == '1') return false;
        }
        return true;
    }
    
    public boolean emptyColumn(char[][] image, int num) {
        int m = image.length;
        for (int i = 0; i < m; i++) {
            if (image[i][num] == '1') return false;
        }
        return true;
    }
}
```

### 74. Search a 2D Matrix

Write an efficient algorithm that searches for a value in an `m x n` matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```

> 解法

由于二维数组从左至右，从上至下已经排好序，可以把它看成一个一维数组

```java
public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix == null || matrix[0].length == 0) return dalse;
    int n = matrix.length;
    int m = matrix[0].length;
    int lo = 0;
    int hi = n * m;
    while (lo + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        int row = mid / m;
        int col = mid % m;
        if (matrix[row][col] < target) {
            lo = mid;
        } else if (matrix[row][col] > target) {
            hi = mid;
        } else if (matrix[row][col] == target) {
            return true;
        }
    }
    return (matrix[lo / n][lo % n] == target) || (matrix [hi / n][hi % n] == target);
}
```

还有一种解法就是首先确定寻找的数可能所在的行，然后到这行里面再进行寻找

### 240 Search a 2D Matrix II

Write an efficient algorithm that searches for a `target` value in an `m x n` integer `matrix`. The `matrix` has the following properties:

* Integers in each row are sorted in ascending from left to right.
* Integers in each column are sorted in ascending from top to bottom.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/24/searchgrid2.jpg)

```
Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
Output: true
```

> 解法

由于数字在二维数组中呈现出从左上至右下逐渐增大的趋势，可以从左下角开始搜索。如果当前位置的数大于目标值，向上移动一格；如果当前位置的数小于目标值，向右移动一格。

也可以采用二分法对单一的一行或者一列逐一进行搜索。代码略去不表。

### 162. Find Peak Element

A peak element is an element that is strictly greater than its neighbors.

Given an integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`.

You must write an algorithm that runs in `O(log n)` time.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

**Example 2:**

```
Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

> 解法

根据`mid`处于四种不同位置：上升段、下降段、低谷点、峰值点，分类讨论

```
public int findPeakElement(int[] nums) {
    if (nums.length == 1) return 0;
    if (nums.length == 2) return (nums[0] > nums[1]) ? 0 : 1;
    int lo = 0， hi = nums.length - 1;
    
    while (lo  + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] < nums[mid - 1] && nums[mid] > nums[mid + 1]) {
            hi = mid; 
        } else if (nums[mid] > nums[mid - 1] && nums[mid] < nums[mid + 1]) {
            lo = mid;
        } else if (nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) {
            return mid;
        } else if (nums[mid] < nums[mid - 1] && nums[mid] < nums[mid + 1]) {
            lo = mid;
        }
    }
    return Math.max(nums[lo], nums[hi]);
}
```

另一个解法就是每当发现一个`mid`位置的值大于后面一个位置的值，这个`mid`位置就是一个peak点

```
public int findPeakElement(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
        int mid = (l + r) / 2;
        if (nums[mid] > nums[mid + 1])
            r = mid;
        else
            l = mid + 1;
    }
    return l;
}
```

### 33. Search in Rotated Sorted Array

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

> 解法

最直接的解法就是按照前面153题的解法求出pivot点的位置，然后恢复递增的数组，并用二分法求出目标值

另一个思路就是根据`mid`位置、target可能位置进行分类讨论

1. mid在左半边：条件是 nums\[mid] >= nums\[0] 1.1 target在mid的左边：target >= nums\[0] && target < nums\[mid] 1.2 target在mid的右边：target > nums\[mid] || target < nums\[0]
2. mid在右半边：条件是nums\[mid] < nums\[0] 2.1 target在mid的左边：target >= nums\[0] || target < nums\[mid] 2.2 target在mid的右边：target > nums\[mid] && target < nums\[0]

![示意图](https://img-blog.csdnimg.cn/20191223105539904.png?x-oss-process=image/watermark,type\_ZmFuZ3poZW5naGVpdGk,shadow\_10,text\_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3NjAyNg==,size\_16,color\_FFFFFF,t\_70)

```java
public int search(int[] nums, int target) {
    if (nums == null || nums.length == 0) return -1;
    if (nums.length == 1) return (nums[0] == target) ? 0:-1;
    int lo = 0;
    int hi = nums.length - 1;
    while (lo + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] >= nums[0]) {
            // mid before pivot
            if (target >= nums[0] && target < nums[mid]) {
                hi = mid;
            } else if (target > nums[mid] || target < nums[0]) {
                lo = mid;
            }
        } else if (nums[mid] < nums[0]) {
            // mid after pivot
            if (target > nums[mid] && target < nums[0]) {
                lo = mid;
            } else if (target >= nums[0] || target < nums[mid]) {
                hi = mid;
            }
        }
    }

    if (nums[lo] == target) {
        return lo;
    }
    if (nums[hi] == target) {
        return hi;
    }
    return -1;
}
```

### 4. Median of Two Sorted Arrays

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

> 解法

经典题目，可以使用二分法求解，由于中位数实际上就是要将一个数组划分为两个部分，只需要尝试不同的分割方法就可以得到。且由于数组大小已知，确定了在`nums1`中的分割位置就可以确定在`nums2`中的分割位置

以`nums1 = [1,3,6,7], nums2 = [2,4,5,8]`为例子

```
假设在nums1中，1后为分割点，则在nums2中，分割点一定在5后，以为要确保分割点左边的数字个数等于右边的数字个数
1,|| 3, 6, 7
2, 4, 5,|| 8
```

```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    if (nums1.length > nums2.length) return findMedianSortedArrays(nums2, nums1);
    int m = nums1.length;
    int n = nums2.length;
    if ( m == 0 && n == 0) throw new IllegalArgumentException("Invalid input");
    else if (m == 0 && n % 2 == 0) return (nums2[n/2-1]+nums2[n/2]) / 2.0;
    else if (m == 0) return nums2[n/2];
    else if (n == 0 && m % 2 == 0) return (nums1[m/2-1]+nums1[m/2]) / 2.0;
    else if (n == 0) return nums1[m/2];
    else {
        int cut1 = 0, cut2 = 0, lo =0, hi = m;// cut1 : 分割点左边的数字个数
        while (lo <= hi) {
            cut1 = (hi-lo)/2+lo;
            cut2 = (m+n)/2-cut1;
            int l1 = (cut1 == 0)? Integer.MIN_VALUE:nums1[cut1-1];
            int l2 = (cut2 == 0)? Integer.MIN_VALUE:nums2[cut2-1];
            int r1 = (cut1 == m)? Integer.MAX_VALUE:nums1[cut1];
            int r2 = (cut2 == n)? Integer.MAX_VALUE:nums2[cut2];
            /*
                             L1  R1
                    nums1  3 5 | 8 9        cut1: 左有几个元素
                    nums2  1 2 | 7 10 13    cut2：左有几个元素
                             L2  R2
                 */
            if (l1 > r2) hi = cut1 - 1;
            else if (l2 > r1) lo = cut1 + 1;
            else {
                if ((m+n)%2 == 0) return (Math.max(l1,l2)+Math.min(r1,r2)) / 2.0;
                else return Math.min(r1,r2);
            }
        }
    }
    return -1;
}
```

### 162. Find Peak Element

A peak element is an element that is strictly greater than its neighbors.

Given an integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`.

You must write an algorithm that runs in `O(log n)` time.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

> 解法

采用二分法进行求解

1. 只要当前数字大于下一个数字，那么当前数字即当前数字之前必定有`peak`；
2. 按照四种情况，分别判断，采用了模板写法；

```java
public int findPeakElement(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
        int mid = (l + r) / 2;
        if (nums[mid] > nums[mid + 1])
            r = mid;
        else
            l = mid + 1;
    }
    return l;
}

public int findPeakElement(int[] nums) {
    if (nums.length == 1) return 0;
    if (nums.length == 2) return (nums[0] > nums[1]) ? 0 : 1;
    int lo = 0, hi = nums.length - 1;
    while (lo  + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] < nums[mid - 1] && nums[mid] > nums[mid + 1]) {
            hi = mid; 
        } else if (nums[mid] > nums[mid - 1] && nums[mid] < nums[mid + 1]) {
            lo = mid;
        } else if (nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) {
            return mid;
        } else if (nums[mid] < nums[mid - 1] && nums[mid] < nums[mid + 1]) {
            lo = mid;
        }
    }
    return (nums[lo] > nums[hi]) ? lo : hi;
}
```

### 69. Sqrt(x)

Given a non-negative integer `x`, compute and return _the square root of_ `x`.

Since the return type is an integer, the decimal digits are **truncated**, and only **the integer part** of the result is returned.

**Note:** You are not allowed to use any built-in exponent function or operator, such as `pow(x, 0.5)` or `x ** 0.5`.

**Example 1:**

```
Input: x = 4
Output: 2
```

> 解法

先确定一个上下界，然后根据上下界二分，避免overflow，采用 long型

```java
public int mySqrt(int x) {
    if (x < 2) return x;
    int l = 1, r = x/2;
    while (l + 1 < r) {
        int mid = l + (r - l) / 2;
        long temp = (long)mid * mid;
        if (temp == x) {
            return mid;
        } else if (temp > x) {
            r = mid;
        } else {
            l = mid;
        }
    }
    long t = (long)r * r;
    return (t > x) ? l : r;
}
```

### LintCode 183 Wood Cut

Given `n` pieces of wood with length `L[i]` (integer array). Cut them into small pieces to guarantee you could have equal or more than `k` pieces with the same length. What is the longest length you can get from the n pieces of wood? Given `L` & `k`, return the maximum length of the small pieces.

**Example 1**

```
Input:
L = [232, 124, 456]
k = 7
Output: 114
Explanation: We can cut it into 7 pieces if any piece is 114cm long, however we can't cut it into 7 pieces if any piece is 115cm long.
```

> 解法

```java
public int woodCut(int[] L, int k) {
    if (L == null | L.length == 0) return 0;
    Arrays.sort(L);
    int l = 1, r = L[L.length - 1];
    int res = 0;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        int temp = 0;
        for (int length : L) {
            temp += length/ mid;
        }
        if (temp >= k) {
            res = Math.max(res, mid);
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
    return res;
}
```

### LintCode 437 Copy Books

Given `n` books and the `i-th` book has `pages[i]` pages. There are `k` persons to copy these books. These books list in a row and each person can claim a continous range of books. For example, one copier can copy the books from `i-th` to `j-th` continously, but he can not copy the 1st book, 2nd book and 4th book (without 3rd book). They start copying books at the same time and they all cost 1 minute to copy 1 page of a book. What's the best strategy to assign books so that the slowest copier can finish at earliest time? Return the shortest time that the slowest copier spends.

**Example 1:**

```
Input: pages = [3, 2, 4], k = 2
Output: 5
Explanation: 
First person spends 5 minutes to copy book 1 and book 2.
Second person spends 4 minutes to copy book 3.
```

> 解法

直接求十分困难，转换一下问题。把问题变成给定要求完成的时间，求需要最小的人数。采用贪心的策略，每个人都尽量一直抄到时间即将超过指定时间为止。

```java
public class Solution {
    public int copyBooks(int[] pages, int k) {
        // write your code here
        int temp = 0;
        int max = 0;
        for (int t : pages) {
            temp += t;
            max = Math.max(max, t);
        }
        int l = max, r = temp;
        int res = Integer.MAX_VALUE ;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            int tmp = numCal(pages, mid);
            if (tmp <= k) {
                res = Math.min(res, mid);
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return res;
    }
    // 计算在t时间内完成，需要的人数
    public int numCal(int[] pages, int t) {
        int res = 0;
        int temp = 0;
        for (int p : pages) {
            if (temp + p > t) {
                res++;
                temp = p;
            } else {
                temp += p;
            }
        }
        return res + 1;
    }
}
```
