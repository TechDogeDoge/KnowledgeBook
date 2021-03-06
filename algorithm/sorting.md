---
description: Lesson 1 Sorting
---

# 排序

## Quick Sort

写法一

```java
    public static void quickSort(int[] nums) {
        shuffle(nums);
        quickSort(nums, 0, nums.length - 1);
    }
    public static void quickSort(int[] nums, int lo, int hi) {
        if (lo >= hi) return;
        int index = partition(nums, lo, hi);
        quickSort(nums, lo, index - 1);
        quickSort(nums, index + 1, hi);
    }
    public static int partition (int[] nums, int lo, int hi) {
        int from = lo, to = hi + 1;
        int par = nums[lo];
        while (true) {
            while (nums[++from] <= par) {
                if (from == hi) break;
            }
            while (nums[--to] >= par) {
                if (to == lo) break;;
            }
            if (from >= to) break;
            swap(nums, from, to);
        }
        swap(nums, to, lo);
        return to;
    }
    public static void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
    public static void shuffle(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int tmp = (int) (Math.random()*(nums.length-1));
            swap(nums, i, tmp);
        }
    }
```

写法二

```java
    public static void quickSort(int[] nums) {
        quickSort(nums, 0, nums.length - 1);
    }

    public static void quickSort(int[] nums, int start, int end) {
        if (start >= end) return;
        int pivot = nums[(start + end) / 2];
        int left = start, right = end;
        while (left <= right) {
            while(left <= right && nums[left] < pivot) {
                left++;
            }
            while(left <= right && nums[right] > pivot) {
                right--;
            }
            if (left <= right) {
                int tmp = nums[left];
                nums[left] = nums[right];
                nums[right] = tmp;
                //swap(nums, left, right);
                left++;
                right--;
            }
        }
        quickSort(nums, start, right);
        quickSort(nums, left, end);
    }
```

## Merge Sort

## Bubble Sort

## Bucket Sort
