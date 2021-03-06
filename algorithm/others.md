---
description: Lesson 8 Others
---

# 其他

## 146 LRU Cache

Design a data structure that follows the constraints of a [**Least Recently Used (LRU) cache**](https://en.wikipedia.org/wiki/Cache\_replacement\_policies#LRU).

Implement the `LRUCache` class:

* `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
* `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
* `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

> 解法

LRU主要有两个要求点：

1. 按照key快速获取元素；
2. 维护一串数，并随时更新数的位置；

所以采用`HashMap`加`Doubly-LinkedList`

> 为什么是Doubly-LinkedList？
>
> 双链表可以很方便的确定当前节点的前后节点，方便删除节点和插入节点；
>
> 为什么节点要存储key和value？
>
> 当达到capacity时需要删除第一个元素，如果不存储key，就无法对HashMap中的键值对进行删除

```java
class LRUCache {
    private class Node {
        int key;
        int val;
        Node next;
        Node prev;
        public Node(int key, int val) {
            this.key = key;
            this.val = val;
            this.next = null;
            this.prev = null;
        }
    }
    private int capavity;
    private head = new Node(-1, -1);
    private tail = new Node(-1, -1);
    private HashMap<Integer, Node> map = new HashMap<>();

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.head.next = tail;
        this.tail.prev = head;
    }
    
    public int get(int key) {
        if (!map.contains(key)) return -1;
        // 取出当前元素
        Node curr = map.get(key);
        Node prev = curr.prev;
        Node next = curr.next;
        prev.next = next;
        next.prev = prev;
        // 当前元素插入到tail前
        Node last = tail.prev;
        l.next = curr;
        curr.prev = l;
        curr.next = tail;
        tail.prev = curr;
        return curr.val;
    }
    
    public void put(int key, int value) {
        if (get(key)!= -1) {
            map.get(key).val = value;
            return;
        }
        if (map.size() == capacity) {
            // 删除第一个元素
            map.remove(head.next.key);
            head.next.next.prev = head;
            head.next = head.next.next;
        }
        Node last = tail.prev;
        Node curr = new Node(value);
        map.put(key, curr);
        // 当前元素插入到tail前
        last.next = curr;
        curr.prev = last;
        curr.next = tail;
        tail.prev = curr;
    }
}
```

## 堆与栈

### 堆

堆是一种特殊的二叉树，并且分为两种：最大堆、最小堆。在最大堆中，父节点的值大于每一个子节点的值；在最小堆中，父节点的值小于每一个子节点的值。下面就是一个最大堆的例子

![img](https://upload-images.jianshu.io/upload\_images/4064751-14a6cde25bdff968.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/190/format/webp)

### 例题

### 215. Kth Largest Element in an Array

Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

**Example 1:**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

> 解法

可以采用`partition`的求解方法。也可以使用很直接的`PriorityQueue`进行求解

```java
class Solution {
    public Comparator<Integer> comparator = new Comparator<>() {
        public int compare (Integer a, Integer b) {
            return a - b;
        }
    };
    
    public int findKthLargest(int[] nums, int k) {
        if (nums == null || nums.length < k) {
            return -1;
        }
        Queue<Integer> queue = new PriorityQueue<>(comparator);
        for (int i : nums) {
            queue.add(i);
            if (queue.size() > k) {
                queue.poll();
            }
        }
        return queue.poll();
    }
}
```

### 295. Find Median from Data Stream

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

* For example, for `arr = [2,3,4]`, the median is `3`.
* For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

* `MedianFinder()` initializes the `MedianFinder` object.
* `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
* `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.

**Example 1:**

```
Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

> 解法

采用heap进行求解，维护一个最大堆`maxHeap`和一个最小堆`minHeap`。最大堆用来存储较小的一半数字；最小堆用来存储较大的一半数字，且最小堆中的所有数字均大于最大堆中的任意数字。确保`maxHeap`中的元素个数大于等于`minHeap`中的元素个数；

```java
class MedianFinder {
	private PriorityQueue<Integer> minHeap;
    private PriorityQueue<Integer> maxHeap;
    public int size;
    public Comparator<Integer> comparator = new Comparator<>() {
        public int compare(Integer a, Integer b) {
            return b - a;
        }
    };
    public MedianFinder() {
        minHeap = PriorityQueue<Integer>();
        maxHeap = new PriorityQueue<Integer>(comparator);
        minHeap.add(Integer.MAX_VALUE);
        maxHeap.add(Integer.MIN_VALUE);
        size = 0;
    }
    // 总是确保maxHeap中的元素个数大于等于minHeap中的元素个数
    public void addNum(int num) {
        size++
        if (maxHeap.size() > minHeap.size()) {
            if (num < maxHeap.peek()) {
                int tmp = maxHeap.poll();
                minHeap.add(tmp);
                maxHeap.add(num);
            } else {
                minHeap.add(num);
            }
        } else {
            if (num < minHeap.peek()) {
                maxHeap.add(num);
            } else {
                int tmp = minHeap.poll();
                minHeap.add(num);
                maxHeap.add(tmp);
            }
        }
    }
    public double findMedian() {
        if (size % 2 == 0) {
            return maxHeap.peek() * 0.5 + minHeap.peek() * 0.5;
        } else {
            return maxHeap.peek() * 1.0;
        }
    }
}
```

### 480. Sliding Window Median

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle values.

* For examples, if `arr = [2,3,4]`, the median is `3`.
* For examples, if `arr = [1,2,3,4]`, the median is `(2 + 3) / 2 = 2.5`.

You are given an integer array `nums` and an integer `k`. There is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the median array for each window in the original array_. Answers within `10-5` of the actual value will be accepted.

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [1.00000,-1.00000,-1.00000,3.00000,5.00000,6.00000]
Explanation: 
Window position                Median
---------------                -----
[1  3  -1] -3  5  3  6  7        1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7        3
 1  3  -1  -3 [5  3  6] 7        5
 1  3  -1  -3  5 [3  6  7]       6
```

> 解法

采用和上面一题相同的解法。维护一个最大堆和一个最小堆。在`addNum`和`findMedian`的基础上新增一个`removeNum`的成员方法。

```java
class Solution {
    public Comparator<Integer> comparator = new Comparator<>() {
        public int compare(Integer a, Integer b) {
            return b.compareTo(a);
        }
    };
    PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>();
    PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(comparator);
    int size = 0;;
    public double[] medianSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length < k || k <= 0) return null;
        minHeap.add(Integer.MAX_VALUE);
        maxHeap.add(Integer.MIN_VALUE);
        while (size < k) {
            addNum(nums[size]);
            size++;
        }
        double[] result = new double[nums.length - k + 1];
        for (int i = 0; i < result.length; i++) {
            result[i] = findMedian();
            if (i == result.length - 1) break;
            removeNum(nums[i]);
            addNum(nums[i+k]);
        }
        return result;
    }
    
    public void addNum(int num) {
        size++;
        if (maxHeap.size() > minHeap.size()) {
            if (num < maxHeap.peek()) {
                int tmp = maxHeap.poll();
                minHeap.add(tmp);
                maxHeap.add(num);
            } else {
                minHeap.add(num);
            }
        } else {
            if (num < minHeap.peek()) {
                maxHeap.add(num);
            } else {
                int tmp = minHeap.poll();
                maxHeap.add(tmp);
                minHeap.add(num);
            }
        }
    }
    
    public void removeNum(int num) {
        size--;
        if (num < minHeap.peek()) {
            maxHeap.remove(num);
        } else {
            minHeap.remove(num);
        }
    }
    
    public double findMedian() {
        if (size % 2 == 0) {
            return maxHeap.peek() * 0.5 + minHeap.peek() * 0.5;
        } else {
            return maxHeap.peek() * 1.0;
        }
    }
}
```

### 42. Trapping Rain Water

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

> 解法

某一位置可以存储的水量等于此位置左右两侧最大高度中较小的一个减去此位置的高度。使用栈`stack`进行求解

### 栈

先入后出队列，栈可以使用链表来实现。

栈和队列之间可以互相实现：

1 两个栈可以实现一个队列：由于栈先入后出的性质，经过两次先入后出的栈，最终就会先入先出。

> 定义两个栈`s1`和`s2`
>
> push：直接压入`s1`即可 pop：如果`s2`不为空，把`s2`中的栈顶元素直接弹出；否则，把`s1`的所有元素全部弹出压入`s2`中，再弹出`s2`的栈顶元素

2 两个队列可以实现一个栈：由于要确保先入后出，所以需要每次`pop`的时候弹出队列最后一个元素

> 定义两个队列`q1`和`q2`
>
> push：直接压入到`q1`即可
>
> pop：将`q1`中除队尾元素外，其他元素全部`pop`然后压入到`q2`中，然后把`q1`剩余的一个元素`pop`除队列，最后把`q2`中的所有元素转移回`q1`中

### 例题

### Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

* `MinStack()` initializes the stack object.
* `void push(int val)` pushes the element `val` onto the stack.
* `void pop()` removes the element on the top of the stack.
* `int top()` gets the top element of the stack.
* `int getMin()` retrieves the minimum element in the stack.

**Example 1:**

```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

> 解法

定义两个`stack`，一个用来存储值，另一个`stack`存储从`stack`底部到当前层的最小值

```java
class MinStack {
    Stack<Integer> stack;
    Stack<Integer> minStack;
    
    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();
    }
    
    public void push(int x) {
        stack.push(x);
        if (!minStack.isEmpty()) {
            minStack.push(Math.min(x, minStack.peek()));
        } else {
            minStack.push(x);
        }
    }
    
    public void pop() {
        stack.pop();
        minStack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```

### 394. Decode String

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.

**Example 1:**

```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

> 解法

采用`Stack`求解，一个字符一个字符的处理。遇到数字进行进位计算，遇到`[`时，将数字压入栈，对于连续字母持续压入栈中，当碰到`]`时，首先将所有字符弹出来，然后将代表重复次数的数字弹出来，并重复指定次数。

```java
    public String decodeString(String s) {
        Stack<Object> stack = new Stack<>();
        int number = 0;
        
        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) {
            	// 一次读取一个字符，确保数字是多位数也是正确的
                number = number * 10 + c - '0';
            } else if (c == '[') {
                stack.push(Integer.valueOf(number));
                number = 0;
            } else if (c == ']') {
                String newStr = popStack(stack);
                Integer count = (Integer) stack.pop();
                for (int i = 0; i < count; i++) {
                    stack.push(newStr);
                }
            } else {
                stack.push(String.valueOf(c));
            }
        }
        return popStack(stack);
    }
    
    private String popStack(Stack<Object> stack) {
        // pop stack until get a number or empty
        Stack<String> buffer = new Stack<>();
        while (!stack.isEmpty() && (stack.peek() instanceof String)) {
            buffer.push((String) stack.pop());
        }
        
        StringBuilder sb = new StringBuilder();
        while (!buffer.isEmpty()) {
            sb.append(buffer.pop());
        }
        return sb.toString();
    }
```

## 扫描线

扫描线问题通常用来解决交集相关的问题。比如说求空中最大飞机数，安排会议室时间等

### LintCode 391. Number of Airplanes in the Sky

Given an list interval, which are taking off and landing time of the flight. How many airplanes are there at most at the same time in the sky?

**Example 1:**

```
Input: [(1, 10), (2, 3), (5, 8), (4, 7)]
Output: 3
Explanation:
The first airplane takes off at 1 and lands at 10.
The second ariplane takes off at 2 and lands at 3.
The third ariplane takes off at 5 and lands at 8.
The forth ariplane takes off at 4 and lands at 7.
During 5 to 6, there are three airplanes in the sky.
```

> 解法

每当遇到起飞的时间点时，空中飞机个数就加一，每当遇到降落的时间点时，空中飞机个数就减一。因此将所有的时间点进行排序，降落的和起飞的，起飞时间点标记为正的，降落时间点标记为负的。题目说明了当一个时间点同时有降落起飞时，先计算降落再计算起飞，于是`comparator`比较时间的时候，降落时间点在前

```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 * }
 */
class Point {
    int val;
    int flag;
    public Point(int v, int f) {
        val = v;
        flag = f;
    }
}

public class Solution {
    public Comparator<Point> comparator = new Comparator<Point>() {
        public int compare(Point a, Point b) {
            return (a.val != b.val) ? a.val - b.val : a.flag - b. flag;
        }
    };
     
    public int countOfAirplanes(List<Interval> airplanes) {
        // write your code here
        if (airplanes == null || airplanes.size() == 0) return 0;
        int n = airplanes.size();
        ArrayList<Point> points = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            points.add(new Point(airplanes.get(i).start, 1));
            points.add(new Point(airplanes.get(i).end, -1));
        }
        Collections.sort(points, comparator);
        int res = 0, count = 0;
        for (Point p : points) {
            if (p.flag == 1) {
                count++;
            } else {
                count--;
            }
            res = Math.max(count, res);
        }
        return res;
    }
}
```

### 252 Meeting Room

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...] (si < ei)`, determine if a person could attend all meetings.

**Example 1:**

```java
Input: 
[[0,30],[5,10],[15,20]]
Output: false
```

> 解法

题目只需要判断给定区间是否存在交集即可。最直观的解法就是，按照起始时间从小到大排序，只要每个开会起始时间大于等于前一个会议的结束时间则返回`true`，否则`false`。

### 253. Meeting Rooms II

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...] (si < ei)`, find the minimum number of conference rooms required.

**Example 1:**

```
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```

> 解法

题目就是求解同一时间下，最多有多少个会议在同时进行。每当遇到一个起始时间，数目加一；每当遇到一个结束时间，数目减一。

```java
class Solution {
    class Event {
        int time;
        int flag;
        public Event(int t, int f) {
            time = t;
            flag = f;
        }
    }
    Comparator<Event> comparator = new Comparator<>() {
        public int compare(Event a, Event b) {
            return (a.time == b. time) ? a.flag - b.flag; : a.time - b.time;
        }
    };
    
    public int minMeetingRooms(int[][] intervals) {
        if (intervals == null || intervals.length == 0) return 0;
        ArrayList<Event> list = new ArrayList<>();
        for (int i = 0; i < intervals.length; i++) {
            list.add(new Event(intervals[i][0], 1));
            list.add(new Event(intervals[i][1], -1));
        }
        Collections.sort(list, comparator);
        int res = 0, count = 0;
        for (Event e : list) {
            if (e.flag == 1) {
                count++;
            } else {
                count--;
            }
            res = Math.max(res, count);
        }
        return res;
    }
}
```

### LintCode 131 The Skyline Problem

Given `N` buildings in a x-axis，each building is a rectangle and can be represented by a triple `(start, end, height)`，where start is the start position on x-axis, end is the end position on x-axis and height is the height of the building. Buildings may overlap if you see them from far away，find the outline of them。

An outline can be represented by a triple, `(start, end, height)`, where start is the start position on x-axis of the outline, end is the end position on x-axis and height is the height of the outline. ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520035930201.png)

> 解法

将·中给的每个矩形，拆成两个事件，一个是正事件（一栋楼出现），一个是负事件（一栋楼消失）。在将所有事件进行排序，需要注意的是，事件排序完毕后，进行扫描时，天际线的高度取决于此处存在的所有楼的高度的最大值。遍历所有事件时，遇到一个正事件，将大楼高度加入到`PriorityQueue`中；遇到一个负事件，从`PriorityQueue`中删除这个大楼高度。每当PriorityQueue的最大值发生变化时就说明找到了一个拐点。 **`edge case`需要特别考虑：起始和结尾**

### Time Intersection

Given two sets of time sections, find the intersection between the two sets of time sections.

> 解法

可以将这两个sets看作两架飞机，不断的起飞和降落，intersection 就代表空中同时又两个飞机。intersection的起始点就是空中刚开始有两个飞机，intersection的结束点就是空中刚刚没有两架飞机。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200523034525160.png?x-oss-process=image/watermark,type\_ZmFuZ3poZW5naGVpdGk,shadow\_10,text\_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3NjAyNg==,size\_16,color\_FFFFFF,t\_70)

## Union Find

一种用于支持集合快速合并和查找操作的数据结构。跟动态连通性有关的问题，用`Union Find`。跟静态连通性（图是静态的）有关的问题，用`BFS`

Union Find实际上就是两个相关的`method`，一个`union`和一个`find`。`union`就是将两个节点进行连通，同时将一个节点的父节点标记为另一个节点。`find`就是寻找一个的最高父节点。

```java
class UnionFind {
    int[] father;
    public void UnionFind(int size) {
        father = new int[size + 1];
        for (int i = 0; i <= size; i++) {
            father[i] = i;
        }
    }
    
    public int find(int x) {
        int curr = x;
        while (father[curr] != curr) {
            curr = father[curr];
        }
        while (curr != x) {
            int temp = father[x];
            father[x] = curr;
            x = temp;
        }
        return curr;
    }
    
    public void union(intx, int y) {
        int father_x = find(x);
        int father_y = find(y);
        if (father_x != father_y) {
            father[father_x] = father_y;
            size[father_y] += size[father_x];
        }
    }
}
```

### 实现

使用哈希表或者数组来存储每个节点的父节点。当节点使用连续整数代表时，采用数组来存储，否则采用哈希表。

同时在过程中要进行压缩，将所有节点的父节点都压缩为最顶端的父节点。

### 例题 Connecting Graph

Given `n` nodes in a graph labeled from 1 to n. There is no edges in the graph at beginning. You need to support the following method: **connect(a, b)**, add an edge to connect node a and node b. **query(a, b)**, check if two nodes are connected

**Example 1:**

```
Input:
ConnectingGraph(5)
query(1, 2)
connect(1, 2)
query(1, 3) 
connect(2, 4)
query(1, 4) 
Output:
[false,false,true]
```

> 解法

采用路径压缩算法

```java
public class ConnectingGraph {
    int[] father;
    int[] size;
    public ConnectingGraph(int n) {
        father = new int[n + 1];
        size = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            father[i] = i;
            size[i] = 1;
        }
    }
    
    public int find(int x) {
        int j = x;
        int tmp = x;
        while (father[j] != j) {
            j = father[j];
        }
        while (tmp != j) {
            int temp = father[tmp];
            father[tmp] = j;
            tmp = temp;
        }
        return j;
    }
    
    public void connect(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if (root_b != root_a) {
            father[root_a] = root_b；
            size[root_b] += size[root_a];
        }
    }

    public boolean query(int a, int b) {
        return find(a) == find(b);
    }
}
```

### 200. Number of Islands

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

> 解法

首先定义好一个`father`数组用来存储每个节点的父节点。其次遍历`grid`中的所有元素当发现 `grid[i][j]` 为1时，island数目加一，将 `grid[i][j]` 修改为0，然后和四周的四个点`union`，当发生一次有效的`union`的时候，island数目减一，有效`union`是指`father`不同然后`union`起来。需要注意的是由于需要确保有效`union`，最好采用压缩路径的写法， 但是其实结果没有区别，因为`find()`最终返回的就是最上层的`father`。

或者可以理解为，如果一个位置为 1，则将其与相邻四个方向上的 1 在并查集中进行合并，最终岛屿的数量就是并查集中连通分量的数目。

上面的是`UnionFind`的算法，本题目也可以采用动态规划的算法。

### 305. Number of Islands II

A 2d grid map of `m` rows and `n` columns is initially filled with water. We may perform an `addLand` operation which turns the water at position `(row, col)` into a land. Given a list of positions to operate, count the number of islands after each `addLand` operation. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example:**

```
Input: m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
Output: [1,1,2,3]

Explanation:
Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).
0 0 0
0 0 0
0 0 0

Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.
1 0 0
0 0 0   Number of islands = 1
0 0 0

Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.
1 1 0
0 0 0   Number of islands = 1
0 0 0

Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.
1 1 0
0 0 1   Number of islands = 2
0 0 0

Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.
1 1 0
0 0 1   Number of islands = 3
0 1 0
```

## Trie
