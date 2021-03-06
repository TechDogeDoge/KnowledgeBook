---
description: Lesson 4 Linked List and Array
---

# 链表与数组

## 链表

### 链表顺序

#### 206. Reverse Linked List

Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
Input: head = [1,2]
Output: [2,1]
```

> 解法

链表调换顺序的题目通常都采用类似递归的`while`写法，从左到右依次执行

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode curr = head;
    ListNode prev = null;
    ListNode next = curr.next;
    while (next != null) {
        curr.next = prev;
        prev = curr;
        curr = next;
        next = next.next;
    }
    curr.next = prev;
    return curr;
}

public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode curr = head;
    ListNode prev = null;
    while (curr != null) {
        ListNode temp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = temp;
    }
    return prev;
}
```

#### 92. Reverse Linked List II

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return _the reversed list_.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

**Example 2:**

```
Input: head = [5], left = 1, right = 1
Output: [5]
```

> 解法

采用和上面一模一样的解法，区别在于要首先确定翻转起始点的位置

```java
public ListNode reverseBetween(ListNode head, int m, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    int index = 0;
    ListNode ptr = dummy;
    ListNode start = null;
    while (index < m) {
        start = ptr;
        ptr = ptr.next;
        index++;
    }
    // start is the node before ptr
    ListNode cur = ptr.next;
    ListNode prev = ptr;
    // start -> node1 -> node2...-> node(m-n) -> cur...
    // start -> node(m-n)...-> node2 -> node1 -> cur...
    while (index < n) {
        ListNode tmp = cur.next;
        cur.next = prev;
        prev = cur;
        cur = tmp;
        index++;
    }
    // when finish, prev is the last node that needs to be reversed
    // cur is the node after prev
    start.next = prev;
    ptr.next = cur;
    return dummy.next;
}
```

#### 25. Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list _k_ at a time and return its modified list.

_k_ is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of _k_ then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse\_ex1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse\_ex2.jpg)

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

> 解法

采用相同的思路

```java
public ListNode reverseKGroup(ListNode head, int k) {
    if (head == null) return null;
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode prev = dummy;
    while (prev != null) {
        prev = reverseK(prev, k);
    }
    return dummy.next;
}
// prev -> n1 -> n2... -> nk-1 -> nk -> nk+1
// prev -> nk -> nk-1... -> n2 -> n1 -> nk+1
// return n1
public ListNode reverseK(ListNode prev, int k) {
    if (prev == null) return null;
    ListNode test = prev;
    for (int i = 0; i < k; i++) {
        if (test.next == null) return null;
        test = test.next;
    }

    ListNode first = prev.next;
    ListNode start = first;
    ListNode cur = first.next;
    int count = 1;

    while(count < k) {
        ListNode tmp = cur.next;
        cur.next = first;
        first = cur;
        cur = tmp;
        count++;
    }
    prev.next = first;
    start.next = cur;
    return start;
}
```

### 链表排序

#### 21. Merge Two Sorted Lists

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return _the head of the merged linked list_.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge\_ex1.jpg)

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

> 解法

采用`merge sort`相同的写法

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if (l1 == null) return l2;
    if (l2 == null) return l1;

    ListNode dummy = new ListNode(Integer.MIN_VALUE);
    ListNode ptr = dummy;
    ListNode h1 = l1;
    ListNode h2 = l2;
    while (h1 != null && h2 != null) {
        if (h1.val < h2.val) {
            ptr.next = h1;
            ptr = ptr.next;
            h1 = h1.next;
        } else {
            ptr.next = h2;
            ptr = ptr.next;
            h2 = h2.next;
        }
    }
    if (h1 == null) ptr.next = h2;
    if (h2 == null) ptr.next = h1;
    return dummy.next;
}
```

#### 148. Sort List

Given the `head` of a linked list, return _the list after sorting it in **ascending order**_.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort\_list\_1.jpg)

```
Input: head = [4,2,1,3]
Output: [1,2,3,4]
```

> 解法

采用数组的`merge sort或` `quick sort`的方法进行排序

### 其他

#### 138. Copy List with Random Pointer

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object\_copying#Deep\_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return _the head of the copied linked list_.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

* `val`: an integer representing `Node.val`
* `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/12/18/e1.png)

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

> 解法

之前在`BFS`的题目中有一道题目(No. 133)是要获取图的深拷贝，使用`HashMap`来映射旧节点和新节点的关系。此题目可以采用相同的思路，同样使用`HashMap`来映射旧节点和新节点的关系

```java
public Node copyRandomList(Node head) {
    if (head == null)  return null;
    HashMap<Node, Node> copy = new HashMap<>();
    Node start = head;
    Node res = head;
    // copy the nodes
    while (head != null) {
        copy.put(head, new Node(head.val));
        head = head.next;
    }
    // copy the link (next, random)
    while (start != null) {
        copy.get(start).next = copy.get(start.next);
        copy.get(start).random = copy.get(start.random);
        start = start.next;
    }
    return copy.get(res);
}
```

还有一种比较巧妙的算法：

假设原始的链表结构是

```java
1 -> 2 -> 3 -> 4
```

现在把每一个copy节点都插入到被copy的节点的后面

```java
1 -> 1' -> 2 -> 2' -> 3 -> 3' -> 4 -> 4'
```

于是整个解法分为三个步骤

1. copy 每个节点并且把点插入到相应位置
2. 指定每一个新节点的`node.random` ==node.next.random = node.random.next;==
3. 拆分链表，把1 -> 1' -> 2 -> 2' -> 3 -> 3' -> 4 -> 4' 变回 1 -> 2 -> 3 -> 4 和 1' -> 2' -> 3' -> 4'

尤其需要注意的是链表中是否 null 的判断

#### 141. Linked List Cycle

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

> 解法

定义两个指针，一个一次前进一个节点，另一个一次前进两个节点。如果链表中存在环，两者必定会相遇。

```java
public boolean hasCycle(ListNode head) {
    if (head == null) return false;
    ListNode fast = head;
    ListNode slow = head;
    while (fast != null) {
        if (fast.next == null || fast.next.next == null) {
            return false;
        }
        fast = fast.next.next;
        slow = slow.next;
        if (fast.equals(slow)) {
            return true;
        }
    }
    return false;
}
```

#### 142. Linked List Cycle II

Given the `head` of a linked list, return _the node where the cycle begins. If there is no cycle, return_ `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

> 解法

依旧采用前面快慢指针的解法。当快慢指针同时遍历到相同点时，定义两个慢指针分别从起始点和当前重合点，开始遍历。这两个慢指针重合的地方就是环的起始点

```java
public ListNode detectCycle(ListNode head) {
    if(head == null) return null;
    ListNode fast = head;
    ListNode slow = head;
    while (fast != null) {
        if (fast.next == null || fast.next.next == null) {
            return null;
        }
        fast = fast.next.next;
        slow = slow.next;
        if (fast.equals(slow)) {
            break;
        }
    }

    ListNode secSlow = head;
    while (!secSlow.equals(slow)) {
        slow = slow.next;
        secSlow = secSlow.next;
    }
    return slow;
}
```

还有一个和这个问题相关的follow up。就是判断两个链表之间是否有重合的部分。

假设有 L1，L2 两个链表，遍历 L1 到结尾，然后把 L1 结尾和 L2 的 head 连在一起，如果两个链表有重合，那么这样得到的链表一定有 cycle。于是这个问题就转换为判断是否有 cycle 的问题

## 数组

#### 88. Merge Sorted Array

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

> 解法

创建一个辅助数组，直接进行`merge`即可，需要判断一下数组是否遍历结束

```java
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        if (nums1 == null || nums2 == null) return;
        if (nums2.length != n || n <= 0 || m + n > nums1.length) return;
        int[] aux = new int[m + n];
        for (int i = 0; i < m; i++) {
            aux[i] = nums1[i];
        }
        int tmp = 0;
        for (int i = m; i < m + n; i++) {
            aux[i] = nums2[tmp++]; 
        }
        
        int ptr1 = 0, ptr2 = m;
        for (int i = 0; i < m + n; i++) {
            if (ptr1 == m) {
                nums1[i] = aux[ptr2++];
            } else if (ptr2 == nums1.length) {
                nums1[i] = aux[ptr1++];
            } else if (aux[ptr1] <= aux[ptr2]) {
                nums1[i] = aux[ptr1++];
            } else if (aux[ptr1] > aux[ptr2]) {
                nums1[i] = aux[ptr2++];
            }
        }
    }
```

上面的算法需要`O(m+n)`的额外空间，下面的解法不需要额外空间：从后往前比较哪个数更大。更大的数放在后面，可以这样做的原因是因为 nums1 的后面留有空位，所以改变后面的值不会损失任何信息

```java
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        if (nums1 == null || nums2 == null) return;
        if (nums2.length != n || n <= 0 || m + n > nums1.length) return;
        
        int ptr1 = m - 1;// nums1
        int ptr2 = n - 1;// nums2
        for (int i = m + n - 1; i >= 0; i--) {
            if (ptr1 < 0) {
                nums1[i] = nums2[ptr2--];
            } else if (ptr2 < 0) {
                nums1[i] = nums1[ptr1--];
            } else if (nums1[ptr1] >= nums2[ptr2]) {
                nums1[i] = nums1[ptr1--];
            } else if (nums1[ptr1] < nums2[ptr2]) {
                nums1[i] = nums2[ptr2--];
            }
        }
    }
```

#### 349. Intersection of Two Arrays

Given two integer arrays `nums1` and `nums2`, return _an array of their intersection_. Each element in the result must be **unique** and you may return the result in **any order**.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```

> 解法

主要分为两类解法：1 采用`HashSet`判断；2 先将两个数组进行排序，然后再进行比较

```java
    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> set = new HashSet<>();
        for (int i = 0; i < nums1.length; i++) {
            set.add(nums1[i]);
        }
        HashSet<Integer> res = new HashSet<>();
        for (int i = 0; i < nums2.length; i++) {
            if (set.contains(nums2[i])) {
                res.add(nums2[i]);
            }
        }
        int[] result = new int[res.size()];
        int i = 0;
        for (Integer tmp : res) {
            result[i++] = tmp;
        }
        return result;
    } // 采用hashset的解法。使用排序的算法不在此处赘述，可以参考各种排序方法
```

#### 4. Median of Two Sorted Arrays

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

#### 53. Maximum Subarray

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return _its sum_.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

> 解法

采用前缀和进行求解。首先求出数组的前缀和数组，然后任意区间元素的总和就可以通过前缀和数组求得，最终找到其中的最大值即可

```
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    int sum = 0, max = Integer.MIN_VALUE, minSum = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
        max = Math.max(sum - minSum, max);
        minSum = Math.min(sum, minSum);
    }
    return max;
}
```

也可以采用`DP`进行求解，定义`dp[i][j]`代表从`i`到`j`的元素和。

#### 442. Find All Duplicates in an Array

Given an integer array `nums` of length `n` where all the integers of `nums` are in the range `[1, n]` and each integer appears **once** or **twice**, return _an array of all the integers that appears **twice**_.

You must write an algorithm that runs in `O(n)` time and uses only constant extra space.

**Example 1:**

```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [2,3]
```

**Example 2:**

```
Input: nums = [1,1,2]
Output: [1]
```

> 解法

最为直观的解法就是，建立一个`HashSet`或者数组用来记录数字是否出现。**还有一个十分巧妙的解法，可以直接背诵**

遍历数组，对于每一个元素值$num\[i]$, 将位置为 $num\[i] - 1$的元素的值改成负数，如果此时位置为 $num\[i] - 1$的元素的值已经是负数，说明$num\[i]$ 已经出现过一次了，是 duplicates。由于数组中的元素值都在$\[1,n]$间，且一个数字只可能出现一次或者两次

```java
public List<Integer> findDuplicates(int[] nums) {
    List<Integer> res = new ArrayList<>();
    for (int i = 0; i < nums.length; ++i) {
        int index = Math.abs(nums[i])-1;
        if (nums[index] < 0)
            res.add(Math.abs(index+1));
        nums[index] = -nums[index];
    }
    return res;
}
```

###
