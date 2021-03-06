---
description: Lesson 5 Two Pointers
---

# 双指针

## 例题

### 283. Move Zeroes

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

**Example 1:**

```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

> 解法

从数组左边起始位置定义两个指针，分别用来记录当前零点和非零点的位置。

```java
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length <= 1) return;
    int left = 0, right = 1;
    while (left < nums.length && right < nums.length) {
        if (nums[left] == 0) {
            while (right < nums.length && nums[right] == 0) {
                right++;
            }
            if (right == nums.length) break;
            swap(nums, left, right);
            left++;
        } else {
            left++;
            right++;
        }
    }
}

private void swap(int[] nums, int i, int j) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
    return;
}
```

### Remove Duplicates Numbers in an Array

Given an array of integers, remove the duplicate numbers in it. You should:

1. do it in place in the array.
2. move the unique numbers to the front of the array.
3. return the total number of unique numbers

**Example:** Given numbers \[1, 3, 1, 4, 4, 2], you should:

1. move unique numbers to the left \[1,3,4,2,?,?]
2. return the number of unique numbers, 4

> 解法

定义起点和终点两个指针，相向而行，起点指针用来记录当前遍历到的不存在duplicate的位置，而终点指针用来记录duplicate存储的位置

```
public List<Integer> removeDuplicates(int[] nums) {
    if (nums == null || nums.length == 0) return new ArrayList<Integer>();]
    HashSet<Integer> set = new HashSet<>();
	int l = 0, r = nums.length - 1;
	while (l <= r) {
		if (set.contains(nums[l])) {
			int temp = nums[r];
			nums[r] = nums[l];
			nums[l] = temp;
			r--;
			continue;
		}
		l++;
	}
    return l;
}
```

还有一种解法就是首先将数组进行排序，然后定义一个指针用来记录当前遍历到的不存在duplicate的位置，接着进行遍历，并更新指针

```java
public List<Integer> removeDuplicates(int[] nums) {
    if (nums == null || nums.length == 0) return new ArrayList<Integer>();
    Arrays.sort(nums);

    int index = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != nums[index]) {
            index++;
            nums[index] = nums[i];
        }
    }
    return index + 1;
}
```

> 这种解法使用`index`来记录当前不存在duplicate的位置，而由于已经进行了排序，所以只需要判断`nums[index]`和`nums[i]`之间的关系

### 1. Two Sum

Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have \***exactly\* one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

> 解法

使用`HashSet`进行求解，可以一次或两次`for`循环求解

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    return null;
}
```

### 167. Two Sum II - Input Array Is Sorted

Given a **1-indexed** array of integers `numbers` that is already \***sorted in non-decreasing order\***, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return _the indices of the two numbers,_ `index1` _and_ `index2`_, **added by one** as an integer array_ `[index1, index2]` _of length 2._

The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.

**Example 1:**

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
```

> 解法

由于数组已经排序了，可以直接定义两个指针分别在两端；如果指针对应元素之和大于`target`，右指针向左移动；如果指针对应元素之和小于`target`，左指针向右移动。

```java
public int[] twoSum(int[] numbers, int target) {
    int left = 0;
    int right = numbers.length - 1;
    while (left < right) {
        if (numbers[left] + numbers[right] == target) {
            return new int[] {left + 1, right + 1};
        }
        if (numbers[left] + numbers[right] < target) {
            left++;
        } else {
            right--;
        }
    }
    return null;
}
```

### 167. Two Sum III - Data structure design

Design and implement a TwoSum class. It should support the following operations: add and find.

add - Add the number to an internal data structure. find - Find if there exists any pair of numbers which sum is equal to the value.

**Example 1:**

```
add(1); add(3); add(5);
find(4) -> true
find(7) -> false
```

**Example 2:**

```
add(3); add(1); add(2);
find(3) -> true
find(6) -> false
```

> 解法

```java
class TwoSum {
    private HashMap<Integer, Integer> map;
    private ArrayList<Integer> list;
    /** Initialize your data structure here. */
    public TwoSum() {
        this.map = new HashMap<Integer, Integer>();
        this.list = new ArrayList<Integer>();
    }
    
    /** Add the number to an internal data structure.. */
    public void add(int number) {
        if (this.map.containsKey(number)) {
            this.map.put(number, this.map.get(number) + 1);
        } else {
            this.map.put(number, 1);
            this.list.add(number);
        }
    }
    
    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {
        for (Integer num : this.list) {
            int num1 = num;
            int num2 = value - num1;
            if (num1 == num2 && this.map.get(num1) > 1) return true;
            if (num1 != num2 && this.map.containsKey(num2)) return true;
        }
        return false;
    }
}
```

### Two Sum-Unique pairs

Given an array of integers, find how many unique pairs in the array such that their sum is equal to a specific target number target number. Please return the number of pairs.

**Example:**

```
Given nums = [1, 1, 2, 45, 46, 46], target = 47
return 2

1 + 46 = 47
2 + 45 = 47
```

> 解法

首先对数组进行排序，然后定义左右指针，遍历数组然后找到所有 unique pairs

```java
publuc int twoSumUnique(int[] nums, int target) {
    if (nums == null || nums.length < 2) return 0;
    Arrays.sort(nums);
    int res = 0, left = 0, right = nums.length - 1;
    while (left < right) {
        if (nums[left] + nums[right] == target) {
            res++;
            left++;
            right--;            
            while (left < right && nums[left] == nums[left - 1]) {
                left++;
            }
            while (left < right && nums[right] == nums[right + 1]) {
                right++;
            }
        } else if (nums[left] + nums[right] < target) {
            left++;
        } else if (nums[left] + nums[right] > target) {
            right++;
        }
    }
    return res;
}
```

### 15. 3Sum

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

> 解法

固定一个取值，`3sum`问题就退化为`2sum`，所以只需要在`2sum`的解法外套一层`for`循环就可以了。需要注意的一点时，此题目中`nums`可.能含有重复的数字，比如说`[1, 1, 2, 4]`就可以跳过第二个`1`，因为会遍历相同的数字组合(1, 2, 4)。**由于需要返回所有可能的解，所以这个题目实际上也可以划分为求组合的题目，使用`DFS`求解。**

### 611. Valid Triangle Number

Given an integer array `nums`, return _the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle_.

**Example 1:**

```
Input: nums = [2,2,3,4]
Output: 3
Explanation: Valid combinations are: 
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3
```

> 解法

题目可以看做是`3Sum`的变种

首先 for 循环最大的数，循环最大的数是为了减少时间复杂度。 然后定义一对左右指针分别位于最左和最右的位置。当左右指针和刚好大于最大数时，固定右指针，如果左指针向右移动直到右指针所得到的的三条边的组合都满足要求，因此实际上解的个数增加 right - left 个。更新完解的个数之后将右指针向左移动一位，此时确保了当前左指针的左边不可能有解，因为右指针变小了。

如果左右指针和小于最大数时。右指针固定不动，左指针向右移动。 所以 for 循环最大的数就是为了确保，满足三角形条件越来越难，保证新的解只会出现在左指针的右边，因此左右指针都只进行单向的移动 **如果 for 循环最小的数，则无法保证指针的单向移动**

```java
public int triangleNumber(int[] nums) {
    if (nums == null || nums.length < 3) return 0;
    int res = 0;
    Arrays.sort(nums);
    for (int i = 2; i < nums.length; i++) {
        int left = 0;
        int right = i - 1;
        int num = nums[i];
        while (left < right) {
            if (nums[left] + nums[right] > num) {
                res += right - left;
                right--;
            } else {
                left++;
            }
        }
    }
    return res;
} // 解法没有列出每种可行组合的具体值，实际上计算起来并不复杂，可以直接定义一个辅助函数，在更新可行组合数目的之后，根据左右指针的位置，以及最大边的大小得到可行组合的具体值
```

题目依旧可以采用`DFS`的解法

### 16. 3Sum Closest

Given an integer array `nums` of length `n` and an integer `target`, find three integers in `nums` such that the sum is closest to `target`.

Return _the sum of the three integers_.

You may assume that each input would have exactly one solution.

**Example 1:**

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

> 解法

和`3sum`解法一致

```java
    public int threeSumClosest(int[] nums, int target) {
        if (nums == null || nums.length < 3) return -1;
        Arrays.sort(nums);
        int dist = Integer.MAX_VALUE;
        int res = 0;
        for (int i = 0; i < nums.length - 2; i++) {
            int num = nums[i];
            int left = i + 1, right = nums.length - 1;
            while (left < right) {
                int temp = num + nums[left] + nums[right];
                if (Math.abs(temp - target) < dist) {
                    dist = Math.abs(temp - target);
                    res = temp;
                }
                if (temp < target) {
                    left++;
                } else {
                    right--;
                }
            }
        }
        return res;
    }
```

### 209. Minimum Size Subarray Sum

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a **contiguous subarray** `[numsl, numsl+1, ..., numsr-1, numsr]` of which the sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

> 解法

同向双指针，右指针向右一个一个的移动，左指针跟着也向右移动直到恰好等于`target`的位置

```java
public int minSubArrayLen(int s, int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    int res = Integer.MAX_VALUE;
    int[] sum = new int[nums.length + 1];
    sum[0] = 0;
    for (int i = 1; i < sum.length; i++) {
        sum[i] = sum[i - 1] + nums[i - 1];
    }

    int left = 0;
    for (int i = 0; i < nums.length; i++) {
        if (sum[i + 1] - sum[left] < s) {
            continue;
        }
        while (sum[i + 1] - sum[left + 1] >= s) {
            left++;
        }
        if (i + 1 - left < res) {
            res = i + 1 - left;
        }
    }
    return (res==Integer.MAX_VALUE) ? 0:res;
}
```

### 3. Longest Substring Without Repeating Characters

Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

> 解法

和前面一题的解法一样，同向双指针；

```java
public int lengthOfLongestSubstring(String s) {
    int[] chars = new int[128];
    int left = 0, right = 0;
    int res = 0;
    while (right < s.length()) {
        char r = s.charAt(right);
        chars[r]++;
        while (chars[r] > 1) {
            char l = s.charAt(left);
            chars[l]--;
            left++;
        }
        res = Math.max(res, right - left + 1);
        right++;
    }
    return res;
}
```

### 76. Minimum Window Substring

Given two strings `s` and `t` of lengths `m` and `n` respectively, return _the **minimum window substring** of_ `s` _such that every character in_ `t` _(**including duplicates**) is included in the window. If there is no such substring, return the empty string_ `""`_._

The testcases will be generated such that the answer is **unique**.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

> 解法

解法依旧相同，同向双指针

```java
public String minWindow(String s, String t) {
    if (s == null || t == null) return "";
    char[] ss = s.toCharArray();
    char[] tt = t.toCharArray();
    int[] target = new int[128];
    int[] source = new int[128];

    int K = 0;
    for (char ch : tt) {
        target[ch]++;
        if (target[ch] == 1) {
            K++;
        }
    }
    int start = -1;
    int end = -1;
    int S = 0;
    int l = 0;
    int r = 0;
    for (l = 0; l < ss.length; l++) {
        while (S < K && r < ss.length) {
            source[ss[r]]++;
            if (source[ss[r]] == target[ss[r]]) {
                S++;
            }
            r++;
        }
        //r实际指向真实右指针的位置的右边一位
        if (S == K) {
            if (start == -1 || r - l < end - start) {
                start = l;
                end = r;
            }
        }
        source[ss[l]]--;
        if (source[ss[l]] == target[ss[l]] - 1) {
            S--;
        }
    }
    if (start == -1) {
        return "";
    } else {
        return s.substring(start, end);
    }
}
```

### 340 Longest Substring with At Most K Distinct Characters

Given a string, find the length of the longest substring `T` that contains at most `k` distinct characters.

Example 1:

Input: s = "eceba", k = 2 Output: 3 Explanation: T is "ece" which its length is 3. Example 2:

Input: s = "aa", k = 1 Output: 2 Explanation: T is "aa" which its length is 2.

> 解法

同向双指针.开一个数组用来记录每个字符出现的次数，当增加到或者减小到临界值时，更新distinct的字符个数

```java
public int lengthOfLongestSubstringKDistinct(String s, int k) {
    if (s == null || s.length() == 0 || k <=0) return 0;
    char[] ss = s.toCharArray();
    int[] occu = new int[128];
    int res = 0;
    int l = 0, r = 0;
    int dist = 0;
    for (r = 0; r < s.length(); r++) {
        occu[ss[r]]++;
        if (occu[ss[r]] == 1) {
            dist++;
        }
        while (dist > k) {
            occu[ss[l]]--;
            if (occu[ss[l]] == 0) {
                dist--;
            }
            l++;
        }
        res = Math.max(res, r - l + 1);
    }
    return res;
}
```

### 19. Remove Nth Node From End of List

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove\_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

> 解法

两个指针，一个指针领先另一个指针`N`步

```java
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null || n <= 0) return null;
        ListNode fast = head;
        ListNode slow = head;
        for (int i = 0; i < n; i++) {
            fast = fast.next;
        }
        if (fast == null) {
            return head.next;
        }
        while (fast.next != null) {
            slow = slow.next;
            fast = fast.next;
        }
        ListNode tmp = slow;
        slow.next = tmp.next.next;
        return  head;
    }
```

## Partition

### Quick Sort

```java
public static void quickSort(int[] nums, int low, int high) {
    if (low >= high) return;
    int left = low, right = high;
    int key = nums[left];
    // 确保了最终while循环结束的时候，一定有left=right
    while (left < right) {
        while (left < right && nums[right] >= key) {
            right--;
        }
        nums[left] = nums[right];
        while (left < right && nums[left] <= key) {
            left++;
        }
        nums[right] = nums[left];
    }
    nums[left] = key;
    quickSort(nums, low, left - 1);
    quickSort(nums, right + 1, high);
}
```

### Partition Array

Given an array `nums` of integers and an int `k`, Partition the array (i.e move the elements in `nums`) such that.

1. All elements < k are moved to the left
2. All elements >= k are moved to the right Return the partitioning Index, i.e the first index "i" nums\[i] >= k. If all elements in "nums" are smaller than k, then return "nums.length"

**Example**

```
If nums=[3,2,2,1] and k=2, a valid answer is 1.
```

> 解法

定义分别位于起始点和结束点的两个相向指针

```java
public int partitionArray(int[] nums, int k) {
    if (nums == null || nums.length == 0) return 0;
    int left = 0, right = nums.length - 1;
    while (left < right) {
        while (left < right && nums[left] < k) {
            left++;
        }
        while (left < right && nums[right] >=k ) {
            right++;
        }
        if (left < right) {
            int tmp = nums[left];
            nums[left] = nums[right];
            nums[right] = tmp;
            left++;
            right--;
        }
    }
    // the case when all the elements are smaller than target k
    return nums[left] < k ? left + 1 : left;
}
```

### 215. Kth Largest Element in an Array

Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

**Example 1:**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

> 解法

采用`partition`的求解方法。

```java
public int findKthLargest(int[] nums, int k) {
    return findKthLargest(nums, 0, nums.length - 1, k - 1);
}    
public int findKthLargest(int[] nums, int start, int end, int k) {
    if (start >= end) return nums[k];
    int left = start, right = end;
    int pivot = nums[left + (right - left) / 2];
    while (left <= right) {
        while (left <= right && nums[left] > pivot) {
            left++;
        }
        while (left <= right && nums[right] < pivot) {
            right--;
        }
        if (left <= right) {
            swap(nums, left, right);
            left++;
            right--;
        }
    }
    if (right < k && left >= start) {
        return findKthLargest(nums, left, end, k);
    } else if (left > k && right <= end) {
        return findKthLargest(nums, start, right, k);
    } else {
        return nums[k];
    }
}

public void swap(int[] nums, int a, int b) {
    int temp = nums[a];
    nums[a] = nums[b];
    nums[b] = temp;
}
```

### 75 Sort Colors

Given an array `nums` with `n` objects colored red, white, or blue, sort them [**in-place**](https://en.wikipedia.org/wiki/In-place\_algorithm) so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example 1:**

```
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

> 解法

定义左右指针，左指针的左边都是`0`，右指针的右边都是`2`，左右指针的中间都是`1`

```java
public void sortColors(int[] nums) {
    if (nums == null || nums.length == 0) return;
    int left = 0, right = nums.length - 1;
    int i = 0;
    while (i <= right) {
        if (nums[i] == 0) {
            swap(nums, left, i);
            left++;
            i++;
        } else if (nums[i] == 1) {
            i++;
        } else if (nums[i] == 2) {
            swap(nums, right, i);
            right--;
        }
    }
}
```
