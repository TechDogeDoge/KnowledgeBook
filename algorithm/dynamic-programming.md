---
description: Lesson 7 Dynamic Programming
---

# 动态规划

## 思路

首先用动态规划解决问题需要问题具有最优子结构，所谓最优子结构就是指可以**通过子问题的最优解得到原问题的最优解**

### 动态规划求解步骤

1. 明确状态
2. 定义DP数组或DP函数的含义
3. 明确分支选择，即前一状态经过哪些分支转移成当前状态
4. 确定初始状态或base case

DP可以有两种写法，**自顶向下**或者**自底向上**，一般来说自底向上的写法更容易

```java
for 状态1 in 状态1的所有取值:
	for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = optimal(选择1，选择2，...)
```

\==**选择推动状态转移**==

## 坐标型DP

题目通常为一个给定二维grid，求解从某一点到另一点的最短距离等

\==**通用解法是按照grid的样子定义二维数组，并自顶向下计算结果**==

### 64 Minimum Path Sum

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: **You can only move either down or right at any point in time**. Example: Input: \[ \[1,3,1], \[1,5,1], \[4,2,1] ] Output: 7 Explanation: Because the path 1→3→1→1→1 minimizes the sum.

> 解法

由于只能向下或者向右移动，所以可以很方便的得到状态转移方程

```java
int[][] grid; // 给定的grid
// 转移方程，采用in place的写法，grid[i][j]代表从起点到(i,j)的最短距离
grid[i][j] = grid[i][j] + Math.min(grid[i-1][j], grid[i][j-1]);
```

### 62 Unique Paths

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below). The robot can **only move either down or right** at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below). How many possible unique paths are there? ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200217002010719.png?x-oss-process=image/watermark,type\_ZmFuZ3poZW5naGVpdGk,shadow\_10,text\_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3NjAyNg==,size\_16,color\_FFFFFF,t\_70) Note: m and n will be at most 100. Example 1: Input: m = 3, n = 2 Output: 3 Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:

1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right

> 解法

和前面题目一致，只能向下或者向右

```java
int[][] res; // res[i][j]代表从起点到（i,j）的路径数
// 转移方程
res[i][j] = res[i-1][j] + res[i][j-1]);
```

### 70 Climbing Stairs

You are climbing a stair case. It takes n steps to reach to the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top? Note: Given n will be a positive integer.

Example : Input: 2 Output: 2 Explanation: There are two ways to climb to the top.

1. 1 step + 1 step
2. 2 steps

> 解法

跳一层和跳两层分别对应一种状态转移

```java
int[] steps; // steps[i]代表从起点调到i层的跳跃方法数
steps[i] = step[i-1] + step[i-2];
```

### 55 Jump Game

Given an array of non-negative integers, you are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position. Determine if you are able to reach the last index.

Example 1: Input: \[2,3,1,1,4] Output: true Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

> 解法

只要可以从当前位置调到一个可以调到终点的位置，则当前位置一定可以跳到终点

```java
int[] steps; // 记录每个index最大跳跃距离的数据
bool[] jump; // jump[i]代表是否可以从i调到最后一个索引
jump[i] = for (int t = 0; t < steps[i]; t++) {
    if any jump[i+t] is true;
}
```

### 45 Jump Game II

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

Example: Input: \[2,3,1,1,4] Output: 2 Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

Note: You can assume that you can always reach the last index.

> 解法

当想不到如何定义状态方程的时候，不妨试试直接按照求解的结果定义

```java
int[] steps; // 记录每个index最大跳跃距离的数据
int[][] jump; // jump[i][j]从i跳到j需要的最小跳跃数

jump[i][j] = Math.min(
    for (int t = 0; t < steps[i]; i++) {
        jump[i+t][j] + 1;
    }
)
```

还可以按照前面一题的思路求解

```java
int[] steps; // 记录每个index最大跳跃距离的数据
int[] jump; // jump[i]从i跳到终点需要的最小跳跃数

jump[i] = Math.min(
    for (int t = 0; t < steps[i]; t++) {
        jump[i+t] + 1;
    }
)
```

## 接龙型DP

通常为求解满足某特定规律的一个连续的序列的大小。规律可能是递增，可以整除等

**==解法是定义到`index i`为止或者从`index i`到`index j`的满足条件序列的最大长度==**

### 300 Longest Increasing Subsequence

Given an unsorted array of integers, find the length of longest increasing subsequence.

Example: Input: \[10,9,2,5,3,7,101,18] Output: 4 Explanation: The longest increasing subsequence is \[2,3,7,101], therefore the length is 4.

Note: There may be more than one LIS combination, it is only necessary for you to return the length. Your algorithm should run in O(n2) complexity. Follow up: Could you improve it to O(n log n) time complexity?

> 解法

由于需要得到一个递增子序列，所以如果简单的定义从`i`到`j`的最长递增子序列，无法写出转移方程，因为无从判断是否递增。因此需要额外增加限制，帮助确定是否递增。

```java
int[] nums;
// length[i]代表从起始点开始到index i结束的区间内，以index i数字为结尾的最长递增子序列长度
int[] length; 
length[i] = Math.max(
    for (int t = 0; t < i; t++) {
        if (nums[t] < nums[i]) {
            length[t] + 1
		}
    }
)
```

### 368 Largest Divisible Subset

Given a set of distinct positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies:

S~~i~~ % S~~j~~ = 0 or S~~j~~ % S~~i~~ = 0.

If there are multiple solutions, return any subset is fine.

Example 1: Input: \[1,2,3] Output: \[1,2] (of course, \[1,3] will also be ok)

Example 2: Input: \[1,2,4,8] Output: \[1,2,4,8]

> 解法

当判断是否可以新增一个元素的时候，要求新增的元素可以整除子集中的所有元素，但是由于当前子集中的最大数以及整除了其他元素，只需要新增元素整除当前子集最大数即可

同样需要添加约束用来判断是否可以整除

```java
int[] nums;
int[] largest; // largest[i]代表到index i为止，以index i数字为最大元素的子集的大小
largest[i] = Math.max(
    for (int t = 0; t < i; t++) {
        if (nums[i] % nums[t] == 0) {
            largest[t] + 1;
        }
    }
)
```

但是此题目要求的不是子集的大小，而是子集，所以需要记录当前添加一个额外的数组用来记录它整除的前一个数的坐标位置 `pre`

```java
class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        List<Integer> res = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        Arrays.sort(nums);
        int[] memo = new int[nums.length];
        int[] pre = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            memo[i] = 1;
            pre[i] = i;
        }
        
        int length = 1;
        int pos = 0;
        for (int i = 1; i < nums.length; i++) {
            for (int j = i-1; j >= 0; j--) {
                if (nums[i] % nums[j] == 0 && memo[j] + 1 > memo[i]) {
                    memo[i] = memo[j] + 1;
                    pre[i] = j;
                }
            }
            if (memo[i] > length) {
                length = memo[i];
                pos = i;
            }
        }
        
        int divi = nums[pos];
        res.add(nums[pos]);
        while (true) {
            if (pos == pre[pos]) {
                break;
            }
            pos = pre[pos];
            res.add(nums[pos]);
        }
        return res;
    }
}
```

### 354 Russian Doll Envelopes

You have a number of envelopes with widths and heights given as a pair of integers (`w`, `h`). One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope. What is the maximum number of envelopes can you Russian doll? (put one inside other) Note: Rotation is not allowed.

Example: Input: `[[5,4],[6,4],[6,7],[2,3]]` Output: 3 Explanation: The maximum number of envelopes you can Russian doll is `3` (\[2,3] => \[5,4] => \[6,7]).

> 解法

和前面的思路一致，添加约束来帮助判断一个信封包裹前面一个信封

当宽度递增排序时，如果有相等的情况出现会发生错误，比如 `[ (1,2), (1,3), (1,4) ]`，如果不加处理，这三个信封都会被选中。 为了解决问题，排序时以宽度为第一排序关键字升序排列，以长度为第二排序关键字降序排列，这样就变成了`[ (1,4), (1,3), (1,2) ]`，而这个长度序列中最大增序列长度为 `1`。

```java
int[][] envelops; 
// 首先将信封按照宽度由小到大排序
Arrays.sort(envelops, new Comparator<int[]>() {});
int[] size; // size[i]代表到envelope i为止，且以envelope i为最后一个envelope，可以得到的套娃信封数
size[i] = Math.max(
    for (int t = 0; t < i; t++) {
        if (envelops[t][1] < envelops[i][1]) {
            size[t] + 1;
        }
    }
)
```

### 403 Frog Jump

A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water. Given a list of stones' positions (in units) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit. If the frog's last jump was k units, then its next jump must be either `k - 1`, `k`, or `k + 1` units. Note that the frog can only jump in the forward direction.

Note: The number of stones is ≥ 2 and is < 1,100. Each stone's position will be a non-negative integer < 231. The first stone's position is always 0.

Example 1: \[0,1,3,5,6,8,12,17] There are a total of 8 stones. The first stone at the 0th unit, second stone at the 1st unit, third stone at the 3rd unit, and so on... The last stone at the 17th unit.

Return true. The frog can jump to the last stone by jumping

1. 1 unit to the 2nd stone, then 2 units to the 3rd stone, then
2. 2 units to the 4th stone, then 3 units to the 6th stone,
3. 4 units to the 7th stone, and 5 units to the 8th stone.

> 解法

由于是否可以跳到下一个石头上面还取决于上一跳的距离，所以需要记录每一个可能跳到当前石头的跳跃距离

```java
int[] stones;
// jumps的key代表石头的位置，value代表所有可以调到当前石头的跳跃距离
HashMap<Integer, HashSet<Integer>> jumps = new HashMap<>();
for (int i = 0; i < stones.length; i++) {
    jumps.put(stones[i], new HashSet<Integer>());
}
// 初始化到stones[0]跳了0步
jumps.get(0).add(0);
for (int i = 1; i < stones.length; i++) {
    for (int j = 0; j < i; j++) {
        int dist = stones[i] - stones[j];
        for (int k = dist - 1; k <= dist + 1; k++) {
            if (jumps.get(stones[j]).contains(k)) {
                jumps.get(stones[i]).add(dist);
            }
        }
    }
}
return jumps.get(stones[stones.length - 1]).size() != 0;
```

### 5 Longest Palindromic Substring

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example 1: Input: "babad" Output: "bab" Note: "aba" is also a valid answer.

Example 2: Input: "cbbd" Output: "bb"

> 解法

一个子字符串是否为回文，取决于两边的字符是否相同。由于和两边的字符都有关系，所以必须要定义二维数组来表示状态状态方程定义为从`i`到`j`的子字符串是否为回文

```java
string s;
bool[][] isPalindromic; // isPalindromic[i][j]表示从index i到index j的子字符串是回文
isPalindromic[i][j] = isPalindromic[i+1][j-1] && s.charAt(i) == s.charAt(j);
```

## 划分型DP

给定一个序列或者一个字符串，按照要求将序列或者字符串划分成一定段数

### 91 Decode Ways

A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1 'B' -> 2 ... 'Z' -> 26 Given a non-empty string containing only digits, determine the total number of ways to decode it.

Example 1: Input: "12" Output: 2 Explanation: It could be decoded as "AB" (1 2) or "L" (12).

Example 2: Input: "226" Output: 3 Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

> 解法

由于对应的数字最多只有两位，所以当前位置位置的解码数只与前一位置和前前一位置有关

```java
string s;
int[] count; // count[i]代表从起始到index i为止，可能的解码数
count[i] += if (后两位数字组成的数大于等于10小于等于26) count[i-2] 
count[i] += if (后一位数字不是0) count[i-1]
```

### 639 Decode Ways II

A message containing letters from A-Z is being encoded to numbers using the following mapping way: 'A' -> 1 'B' -> 2 ... 'Z' -> 26 Beyond that, now the encoded string can also contain the character `*`, which can be treated as one of the numbers from 1 to 9.

Given the encoded message containing digits and the character `*`, return the total number of ways to decode it.

Also, since the answer may be very large, you should return the output mod 109 + 7.

Example 1: Input: " `*`" Output: 9 Explanation: The encoded message can be decoded to the string: "A", "B", "C", "D", "E", "F", "G", "H", "I".

Example 2: Input: "1 `*`" Output: 9 + 9 = 18 Note: The length of the input string will fit in range \[1, 105]. The input string will only contain the character `*` and digits '0' - '9'.

> 解法

思路和前面题目完全一样，只不过需要分更多的情况进行讨论

## 博弈型DP

通常是两个人进行游戏，常常是下棋，求某人先手的游戏结果。不区分先手后手，而是考虑当下马上要下棋的人的结果

**==解法是求面对当下棋局可以得到的结果==**

### 394 Coins in a Line

There are n coins in a line. Two players take turns to take one or two coins from right side until there are no more coins left. The player who take the last coin wins. Could you please decide the first player will win or lose? If the first player wins, return true, otherwise return false.

Example 1: Input: 1 Output: true

Example 2: Input: 4 Output: true Explanation: The first player takes 1 coin at first. Then there are 3 coins left. Whether the second player takes 1 coin or two, then the first player can take all coin(s) left.

Challenge： O(n) time and O(1) memory

> 解法

不关心谁是先手，只关心现在下棋的人的结果。当前下棋人只能取走一个或者两个。

只要当取走一个或者两个后，对手会输，那么现在下棋的人一定可以赢。

```java
int coins;
bool canWin[]; // canWin[i]代表当前下棋的人面对 i 个棋子，是否可以赢
canWin[i] = !canWin[i - 1] || !canWin[i - 2]
```

### 395 Coins in a Line II

There are n coins with different value in a line. Two players take turns to take one or two coins from left side until there are no more coins left. The player who take the coins with the most value wins. Could you please decide the first player will win or lose? If the first player wins, return true, otherwise return false.

Example 1: Input: \[1, 2, 2] Output: true Explanation: The first player takes 2 coins.

Example 2: Input: \[1, 2, 4] Output: false Explanation: Whether the first player takes 1 coin or 2, the second player will gain more value.

> 解法

由于是求是否可以总分数最大赢得比赛，所以可以将状态定义为面对当前棋子，所能得到的最大分数差

```java
int[] coins;
int[] scores; // scores[i]代表到棋子index i为止，当前棋手所能得到的最大分数差
scores[0] = conis[0];
scores[1] = coins[0] + coins[1];
scores[i] = Math.max(coins[i] - scores[i-1], coins[i] + coins[i-1] - scores[i-2]);
```

或者也可以

```java
int[] coins;
int n = coins.length;
int[] scores; // scores[i]代表面对剩余的棋子index i到index n-1，当前棋手所能得到的最大分数差
scores[n] = 0;
scores[n-1] = coins[n-1];
scores[i] = Math.max(coins[i] - scores[i+1], coins[i] + coins[i+1] - scores[i+2]);
```

### Coins in a Line lll

在 Coins in a Line ll 的基础上添加了可以从队列的两边拿取的规定，不再局限于从前面取。

> 解法

由于现在可以从两边拿取，所以不能继续简单使用一维数组储存结果，而是用二维数组分别限制起始和结束位置

```java
int[] coins;
int[][] scores; // scores[i][j]代表面对[i, j]的硬币，所能得到的最大分数差
scores[i][j] = Math.max(coins[i] - scores[i+1][j], coins[i] + coin[i+1] - scores[i+2][j], coins[j] - scores[i][j-1], coins[j] + coins[j+1] - scores[i][j-2]);
```

## 区间型DP

求一段区间的最值，同时状态转移其实就是区间的转移

### 312 Burst Balloons

Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums\[left] \* nums\[i] \* nums\[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent. Find the maximum coins you can collect by bursting the balloons wisely.

Note: You may imagine nums\[-1] = nums\[n] = 1. They are not real therefore you can not burst them. 0 ≤ n ≤ 500, 0 ≤ nums\[i] ≤ 100

Example: Input: \[3,1,5,8] Output: 167

Explanation: nums = \[3,1,5,8] --> \[3,5,8] --> \[3,8] --> \[8] --> \[] coins = `3*1*5` + `3*5*8` + `1*3*8` + `1*8*1` = 167

> 解法

![image-20211114104313007](C:%5CUsers%5Cjiaoy%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20211114104313007.png)

```java
    public int maxCoins(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        int  n = nums.length;
        
        // define the new balloons sequnce
        int[] num = new int[n + 2];
        num[0] = num[n+1] = 1;
        for (int i = 1; i <= n; i++) {
            num[i] = nums[i-1];
        }
        
        // define the result matrix
        int[][] res = new int[n+2][n+2];
        // edge case
        for (int i = 0; i < n + 1; i++) {
            res[i][i+1] = 0;
        }
        
        // traverse the lenth of the interval, start from 3
        for (int len = 3; len <= n + 2; len++) {
            // traverse the interval start point
            for (int i = 0; i <= n - len + 2; i++) {
                // end point
                int j = i + len - 1;
                res[i][j] = 0;
                // find the maximum
                for (int k = i + 1; k < j; k++) {
                    res[i][j] = Math.max(res[i][k] + res[k][j] + num[i] * num[j] * num[k], res[i][j]);
                }
            }
        }
        return res[0][n+1];
    }
```

### 887 Super Egg Drop

You are given `k` identical eggs and you have access to a building with `n` floors labeled from `1` to `n`.

You know that there exists a floor `f` where `0 <= f <= n` such that any egg dropped at a floor **higher** than `f` will **break**, and any egg dropped **at or below** floor `f` will **not break**.

Each move, you may take an unbroken egg and drop it from any floor `x` (where `1 <= x <= n`). If the egg breaks, you can no longer use it. However, if the egg does not break, you may **reuse** it in future moves.

Return _the **minimum number of moves** that you need to determine **with certainty** what the value of_ `f` is.

**Example 1:**

```
Input: k = 1, n = 2
Output: 2
Explanation: 
Drop the egg from floor 1. If it breaks, we know that f = 0.
Otherwise, drop the egg from floor 2. If it breaks, we know that f = 1.
If it does not break, then we know f = 2.
Hence, we need at minimum 2 moves to determine with certainty what the value of f is.
```

**Example 2:**

```
Input: k = 2, n = 6
Output: 3
```

**Example 3:**

```
Input: k = 3, n = 14
Output: 4
```

**Constraints:**

* `1 <= k <= 100`
* `1 <= n <= 104`

> 解法

`dp(k, n)`代表剩余`k`个鸡蛋，面对`n`个楼层，最少扔鸡蛋次数

```java
HashMap<Integer, Integer> memo = new HashMap<>();

public int dp(int k, int n) {
    if (k == 0) {
        return n;
    }
    if (n == 0) {
        return 0;
    }
    if (memo.containsKey(100 * n + k)) {
        return memo.get[100 * n + k];
    }
    int res = Integer.MAX_VALUE;
    for (int i = 1; i < n; i++) {
        res = Math.min(res, Math.max(dp(k, n - i), dp(k - 1, i - 1)) + 1);
    }
    memo.put(100 * n + k, res);
    return res;
}
```

还有一种解法就是改变一下状态方程的含义以及状态转移方程

`dp[k][m]`代表剩余`k`个鸡蛋，扔`m`次鸡蛋，可以测试的楼层数

所以问题就转化成求`d[k][m] = n`时的`m`的值

状态转移方程：

`dp[k][m] = dp[k-1][m-1] + dp[k][m-1] + 1`

> 虽然由于最终要求的值在二维数组的`index`中，无法确定二维数组的大小，但是可以确定的是对于一个`n`层楼，最大扔鸡蛋次数不会超过`n`，所以可以将二维数组的大小定义为`k+1`h和`n+1`

```java
int solution(int k, int n) {
    int[][] dp = new int[k + 1][n + 1];
    int m = 0;
    while (dp[k][m] < n) {
        m++;
        for (int i = 1; i <= k; i++) {
            dp[i][m] = dp[i-1][m-1] + dp[i][m-1] + 1;
        }
    }
    return m;
}
```

## 双序列型DP

两个序列或者字符串，两者之间需要满足指定条件

### 1143 Longest Common Subsequence

Given two strings text1 and text2, return the length of their longest common subsequence. A subsequence of a string is a new string generated from the original string with some characters(can be none) deleted without changing the relative order of the remaining characters. (eg, "ace" is a subsequence of "abcde" while "aec" is not). A common subsequence of two strings is a subsequence that is common to both strings. If there is no common subsequence, return 0.

Example 1: Input: text1 = "abcde", text2 = "ace" Output: 3 Explanation: The longest common subsequence is "ace" and its length is 3.

Example 2: Input: text1 = "abc", text2 = "abc" Output: 3 Explanation: The longest common subsequence is "abc" and its length is 3.

Example 3: Input: text1 = "abc", text2 = "def" Output: 0 Explanation: There is no such common subsequence, so the result is 0.

Constraints:

1. 1 <= text1.length <= 1000
2. 1 <= text2.length <= 1000
3. The input strings consist of lowercase English characters only.

> 解法

判断一个字符是否属于公共子序列，关键在于这个字符在两个字符串中是否相等。为了增加约束来判断字符是否相同，定义状态方程`maxLen[i][j]`为在两个字符串中分别以字符`i`和`j`为结尾的公共子序列的最大长度

```java
string text1;
string text2;
int[][] maxLen; // maxLen[i][j]为在两个字符串中分别以字符i和j为结尾的公共子序列的最大长度
maxLen[i][j] = Math.max(text1.charAt(i) == text.charAt(j) ? maxLen[i-1][j-1] + 1 : maxLen[i-1][j-1], maxLen[i-1][j], maxLen[i][j-1]);
```

### 72 Edit Distance

Given two words word1 and word2, find the minimum number of operations required to convert word1 to word2.

You have the following 3 operations permitted on a word: Insert a character Delete a character Replace a character

Example 1: Input: word1 = "horse", word2 = "ros" Output: 3 Explanation: horse -> rorse (replace 'h' with 'r') rorse -> rose (remove 'r') rose -> ros (remove 'e')

Example 2: Input: word1 = "intention", word2 = "execution" Output: 5 Explanation: intention -> inention (remove 't') inention -> enention (replace 'i' with 'e') enention -> exention (replace 'n' with 'x') exention -> exection (replace 'n' with 'c') exection -> execution (insert 'u')

> 解法

和上面一题类似，需要额外添加约束来判断当前字符是否相等，定义状态方程`editDist[i][j]`为到两个字符串的字符`i`和`j`为止的编辑距离

```java
string word1;
string word2;
editDist[i][j] = Math.min(editDist[i-1][j], editDist[i][j-1], editDist[i-1][j-1]) + 1;
if (word1.charAt(i) == word2.charAt(j)) {
    editDist[i][j] = Math.min(editDist[i][j], editDist[i-1][j-1]);
}
editDist[i-1][j] // 
```

> `editDist[i-1][j] + 1`可以理解为字符串`word1`多一个字符，需要删除
>
> `editDist[i][j-1] + 1`可以理解为字符串`word1`少一个字符，需要增加
>
> `editDist[i-1][j-1] + 1`可以理解为两个字符不一致需要替换，需要替换

如果想要得到编辑的路径，可以新定义一个数据类型`Node`，存储编辑距离和最后一步操作，原本代码中的`int` 二维数组转换为`Node`二维数组。得到最小编辑距离后，从`res[m][n]`结果向前推，因为`res[m][n]`记录了最后一步操作，而不同操作对应不同的前一步状态

![image-20211016175629236](https://c/Users/jiaoy/AppData/Roaming/Typora/typora-user-images/image-20211016175629236.png)

### 97 Interleaving String

Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

Example 1: Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac" Output: true

Example 2: Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc" Output: false

> 解法

同样需要判断字符是否相同，添加额外约束

```java
string s1, s2, s3;
bool[][] isInterl; // isInterl[i][j]代表到字符串s1, s2的i, j字符为止，s3是不是由s1, s2交织而成
isInterl[i][j] = (isInterl[i-1][j] and s1.charAt(i) == s2.charAt(i+j)) or (isInterl[i][j-1] and s2.charAt(j) == s2.charAt(i+j));
```

### 516 Longest Palindromic Subsequence

Given a string `s`, find _the longest palindromic **subsequence**'s length in_ `s`.

A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

```
Input: s = "bbbab"
Output: 4
Explanation: One possible longest palindromic subsequence is "bbbb".
```

**Example 2:**

```
Input: s = "cbbd"
Output: 2
Explanation: One possible longest palindromic subsequence is "bb".
```

**Constraints:**

* `1 <= s.length <= 1000`
* `s` consists only of lowercase English letters.

> 解法

回文涉及到字符串的两端，所以需要定义二维数组

```java
string s;
int[][] longest; // longest[i][j]从i到j的最大回文子序列长度
longest[i][j] = s.charAt(i) == s.charAt(j) ? longest[i+1][j-1] + 2 : Math.max(longest[i+1][j], longest[i][j-1]);
```

### 10 Regular Expression Matching

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

* `'.'` Matches any single character.
* `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

**Example 1:**

```
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**

```
Input: s = "aab", p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
```

**Example 5:**

```
Input: s = "mississippi", p = "mis*is*p*."
Output: false
```

**Constraints:**

* `1 <= s.length <= 20`
* `1 <= p.length <= 30`
* `s` contains only lowercase English letters.
* `p` contains only lowercase English letters, `'.'`, and `'*'`.
* It is guaranteed for each appearance of the character `'*'`, there will be a previous valid character to match.

> 解法

首先可以采用递归的方法进行解答

```java
class Solution {
    public boolean isMatch(String text, String pattern) {
        if (pattern.isEmpty()) return text.isEmpty();
        boolean first_match = (!text.isEmpty() &&
                               (pattern.charAt(0) == text.charAt(0) || 
                                pattern.charAt(0) == '.'));

        if (pattern.length() >= 2 && pattern.charAt(1) == '*'){
            return (isMatch(text, pattern.substring(2)) ||
                    (first_match && isMatch(text.substring(1), pattern)));
        } else {
            return first_match && isMatch(text.substring(1), pattern.substring(1));
        }
    }
}
```

本题的难点主要在于对`*`的匹配，因为我们没法确定`*`用来匹配多少个字符，但是对应于每一个字符只有两种情况，匹配或者没有匹配，所以每当遇到可以使用`*`匹配的时候，就匹配；且携带这个`*`进入下一个字符的匹配

```java
class Solution {
    public boolean isMatch(String text, String pattern) {
        boolean[][] dp = new boolean[text.length() + 1][pattern.length() + 1];
        dp[text.length()][pattern.length()] = true;

        for (int i = text.length(); i >= 0; i--){
            for (int j = pattern.length() - 1; j >= 0; j--){
                boolean first_match = (i < text.length() &&
                                       (pattern.charAt(j) == text.charAt(i) ||
                                        pattern.charAt(j) == '.'));
                if (j + 1 < pattern.length() && pattern.charAt(j+1) == '*'){
                    dp[i][j] = dp[i][j+2] || first_match && dp[i+1][j];
                } else {
                    dp[i][j] = first_match && dp[i+1][j+1];
                }
            }
        }
        return dp[0][0];
    }
}
```

## 背包型DP

问题通常为给定一些物品和一个固定容量的背包，求如何在背包中装物品最优

### 92 Backpack

Given `n` items with size `A[i]`, an integer `m` denotes the size of a backpack. How full you can fill this backpack?

Example Example 1: Input: \[3,4,8,5], backpack size=10 Output: 9

Example 2: Input: \[2,3,5,7], backpack size=12 Output: 12 Challenge

1. `O(nm)` time and `O(m)` memory.
2. `O(nm)` memory is also acceptable if you do not know how to optimize memory.

> 解法

一个物品是否放入背包收到重量的限制，定义二维数组状态方程

```java
int[] a;
int m;
bool[][] canFit; // canFit[i][j]代表前 i 个物品是否可以拼出 j 的总重量
canFit[i][j] = canFit[i-1][j-a[i]] or canFit[i-1][j]
```

### Backpack ll

There are `n` items and a backpack with size `m`. Given array `A` representing the size of each item and array `V` representing the value of each item.

What's the maximum value can you put into the backpack?

Example 1: Input: m = 10, A = \[2, 3, 5, 7], V = \[1, 5, 2, 4] Output: 9 Explanation: Put A\[1] and A\[3] into backpack, getting the maximum value V\[1] + V\[3] = 9

Example 2: Input: m = 10, A = \[2, 3, 8], V = \[2, 5, 8] Output: 10 Explanation: Put A\[0] and A\[2] into backpack, getting the maximum value V\[0] + V\[2] = 10 Challenge O(nm) memory is acceptable, can you do it in O(m) memory?

> 解法

多了物品价值这个参数，同样根据物品数和重量定义二维数组

```java
int[] A, V;
int m;
int[][] maxVal; // maxVal[i][j]代表前i个东西，拼出重量j，得到的最大物品价值，总价值-1代表不能拼出j重量
maxVal[i][j] = Math.max(maxVal[i-1][j], maxVal[i-1][j-A[i]] + V[i])
public int backPackII(int m, int[] A, int[] V) {
    // write your code here
    if (A == null || A.length == 0) return 0;

    int[][] res = new int[A.length + 1][m + 1];
    for (int i = 0; i < A.length + 1; i++) {
        for (int j = 0; j < m + 1; j++) {
            if (i == 0 && j == 0) {
                res[i][j] = 0;
                continue;
            }

            if (i == 0) {
                res[i][j] = -1;
                continue;
            }

            res[i][j] = res[i-1][j];
            if (A[i-1] <= j && res[i-1][j-A[i-1]] != -1) {
                res[i][j] = Math.max(res[i][j], res[i-1][j - A[i-1]] + V[i - 1]); 
            }
        }
    }

    int temp = 0;
    for (int k = m; k >= 0; k--) {
        temp = Math.max(temp, res[A.length][k]);
    }
    return temp;
} 
```

### Backpack lll

其他条件和问题与Backpack ll一样，区别在于，每种物品都有无穷多个，背包中可以放入任意数量的相同物品。

> 解法

一种物品可以重复选择，

```java
int[] A, V;
int m;
int[][] maxVal; // maxVal[i][j]代表使用前 i 个东西，拼出重量 j，得到的最大物品价值
maxVal[i][j] = Math.max(maxVal[i-1][j-k*A[i]] + k*V[i], k = 0, 1, 2...)
```

> 题目看似需要遍历 k 的取值，来得到最大值，但是实际上由于物品重量是不变的，所以在遍历稍小重量时的 k，实际上已经遍历了一遍，所以在重量稍大的时候，只需要遍历恰好k = 1的情况，存在冗余，可以参考下面的数学推导，f代表得到的最大总价值，v代表 i 物品的价值， a代表 i 物品的重量

`f[i][j] = Max(f[i-1][j], f[i-1][j-a] + v, f[i-1][j-2a] + 2v,...)`

`f[i][j+a] = Max(f[i-1][j+a], f[i-1][j] + v, f[i-1][j-a] + 2v,...) = Max(f[i-1][j+a], f[i][j] + v)`;

所以可以得到`f[i][j] = Max(f[i-1][j], f[i][j-a] + v)`;

在代码上和前面一体基本一样，只有`if`语句内的状态转移方程有所变化

```java
if (A[i-1] <= j && res[i-1][j-A[i-1]] != -1) {
    res[i][j] = Math.max(res[i][j], res[i][j - A[i-1]] + V[i - 1]); 
}
```

### 518 Coin Change 2

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the number of combinations that make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example 1:**

```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**Example 2:**

```
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```

**Example 3:**

```
Input: amount = 10, coins = [10]
Output: 1
```

**Constraints:**

* `1 <= coins.length <= 300`
* `1 <= coins[i] <= 5000`
* All the values of `coins` are **unique**.
* `0 <= amount <= 5000`

> 解法

可以转化成背包问题，给定`n`种物品，物品的价值用`coins`数组表示，求有多少种方式可以恰好装满给定容量的背包。两个因素，重量和物品；求解总的装包方法数。解法上和Backback III一样，因为相同物品有无数个

```java
int amount;
int[] coins;
int[][] res; // res[i][j]代表使用前 i 个硬币，恰好总面额为 j 的方法数
res[i][j] = res[i-1][j] + res[i][j-coins[i]]
```

### K sum

给定数组`A`，包含`n`个互不相等的正整数，问有多少种方式从中找出`K`个数，使得他们的和是`Target`

> 解法

`f[i][k][s]`代表前`i`个数中选出`k`个数，使得和为`s`的方法个数

```java
int[] A;
f[i][k][s] = f[i-1][k][s] + f[i-1][k-1][s-A[i]];
```

### 416 Partition Equal Subset Sum

Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Example 1:**

```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

**Example 2:**

```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
```

**Constraints:**

* `1 <= nums.length <= 200`
* `1 <= nums[i] <= 100`

> 解法

可以直接理解为一个背包问题，有`n`件物品，物品的重量用`nums`数组表示，求是否可以用这些物品恰好装满一个容量为`sum/2`的背包

```java
int[] nums;
int sum;
bool[][] canFit[i][j]; // canFit[i][j]代表前 i 个物品是否可以恰好重量为 j
canFit[i][j] = canFit[i-1][j] || canFit[i-1][j-nums[i]];
```

## 其他DP

### 651 4 Keys Keyboard

Imagine you have a special keyboard with the following keys:

* Key 1: (A): Print one ‘A’ on screen.
* Key 2: (Ctrl-A): Select the whole screen.
* Key 3: (Ctrl-C): Copy selection to buffer.
* Key 4: (Ctrl-V): Print buffer on screen appending it after what has already been printed.

Now, you can only press the keyboard for N times (with the above four keys), find out the maximum numbers of ‘A’ you can print on screen.

Example 1:

```
Input: N = 3
Output: 3
Explanation: 
We can at most get 3 A's on screen by pressing following key sequence:
A, A, A
```

Example 2:

```
Input: N = 7
Output: 9
Explanation: 
We can at most get 9 A's on screen by pressing following key sequence:
A, A, A, Ctrl A, Ctrl C, Ctrl V, Ctrl V
```

Note:

1. 1 <= N <= 50
2. Answers will be in the range of 32-bit signed integer.

> 解法

一开始最直观的思路就是四个`key`对应四种选择，而剩余按键次数、当前屏幕上`A`的数量和剪切板上`A`的数量则作为状态的参数

```java
public maxA(int n) {
    return dp(n, 0, 0);
}

int dp(int pressLeft, int aOnScreen, int aForCopy) {
    if (pressLeft <= 0) return aOnScreen;
    return Math.max(dp(pressLeft - 1, aOnScreen + 1, aForCopy), // press a
                   dp(pressLeft - 1, aOnScreen + aForCopy, aForCopy), // press ctrl + v
                   dp(pressLeft - 2, aOnScreen, aOnScreen)) // press ctrl + a, ctrl c
}
```

但实际上这个解法的时间复杂度较高，可以采用其他方法进行计算

采用一个新的思路：经过分析我们发现得到最多的字母只有两种情况：

一直按`A`: 当按键次数较少的时候

或者采用：A, A, ..., Ctrl-A, Ctrl-C, Ctrl-V, Ctrl-V....Ctrl-A, Ctrl-C, Ctrl-V，Ctrl-V...

所以最后一次按键只可能是`A`或者`Ctrl-V`

于是只定义用`i`表示按键次数作为状态

```java
int n;
int[] dp = new int[n+1]; // dp[i]表示 i 次按键次数的所能得到的最多字母 A 的数量
dp[0] = 0;
for (int i = 1; i <= N; i++) {
    // 按 A 键
    dp[i] = dp[i - 1] + 1;
    for (int j = 2; j < i; j++) {
        // 全选 & 复制 dp[j-2]，连续粘贴 i - j 次
        // 屏幕上共 dp[j - 2] * (i - j + 1) 个 A
        dp[i] = Math.max(dp[i], dp[j - 2] * (i - j + 1));
    }
}
// N 次按键之后最多有⼏个 A？
return dp[N];
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

某一位置可以存储的水量等于此位置左右两侧最大高度中较小的一个减去此位置的高度。

```java
public int trap(int[] height) {
    if (height == null || height.length == 0) return 0;
    int[] leftmax = new int[height.length];
    int[] rightmax = new int[height.length];
    leftmax[0] = height[0];
    for (int i = 1; i < height.length; i++) {
        leftmax[i] = Math.max(leftmax[i-1], height[i]);
    }
    rightmax[height.length - 1] = height[height.length - 1];
    for (int i = height.length - 2; i >= 0; i--) {
        rightmax[i] = Math.max(rightmax[i+1], height[i]); 
    }
    int res = 0;
    for (int i = 0; i < height.length; i++) {
        res+=Math.min(leftmax[i], rightmax[i])- height[i];
    }
    return res;
}
```

### 407. Trapping Rain Water II

Given an `m x n` integer matrix `heightMap` representing the height of each unit cell in a 2D elevation map, return _the volume of water it can trap after raining_.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/08/trap1-3d.jpg)

```
Input: heightMap = [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]
Output: 4
Explanation: After the rain, water is trapped between the blocks.
We have two small ponds 1 and 3 units trapped.
The total volume of water trapped is 4.
```

> 解法

某一位置可以存储的水量等于此位置前后左右四个方向最大高度中较小的一个减去此位置的高度。

### 区间调度问题

给定多个形如`[start, end]`的闭区间，设计一个算法，计算出这些区间中最多有⼏个互不相交的区间

```
int intervalScheduling(int[][] intervals);
```

举个例子，`intervals=[[1, 3], [2, 4], [3, 6]]`，这三个区间中最多有两个区间互不相交，即`[[1,3], [3,6]]`。

> 解法

这个问题可以采用贪心的思路来求解。贪心思路就是首先贪心选择区间结束(`end`)最小的区间，以题目为例就是首先选择区间`[1, 3]`，然后再选择与区间`[1, 3]`没有交集且`end`最小的区间即`[3, 6]`。最终总结得到的算法为：

1. 首先将所有区间按照`end`由小到大排列
2. 选择`end`最小的区间
3. 选择下一个与当前区间没有交集且`end`最小的区间
4. 重复前面两个操作直到遍历完`intervals`中所有区间

```java
int intervalScheduling(int[][] intervals) {
    Arrays.sort(intervals, new Comparator<int[], int[]>() {
        public int compare(int[] a, int[] b) {
            return a[1] - b[1];
        }
    });
    int count = 1;
    int end = intervals[0][1];
    for (int[] interv : intervals) {
        if (interv[0] >= end) {
            count++;
            end = interv[1];
        }
    }
    return count;
}
```

#### 435 Non-overlapping Intervals

Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return _the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping_.

**Example 1:**

```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```

**Example 2:**

```
Input: intervals = [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
```

**Example 3:**

```
Input: intervals = [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

**Constraints:**

* `1 <= intervals.length <= 105`
* `intervals[i].length == 2`

> 解法

前面区间调度问题可以得到不重叠的的区间，而**总的区间数减去不重叠的区间数就是至少要去除的区间**

#### 452 Minimum Number of Arrows to Burst Balloons

There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where `points[i] = [xstart, xend]` denotes a balloon whose **horizontal diameter** stretches between `xstart` and `xend`. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up **directly vertically** (in the positive y-direction) from different points along the x-axis. A balloon with `xstart` and `xend` is **burst** by an arrow shot at `x` if `xstart <= x <= xend`. There is **no limit** to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return _the **minimum** number of arrows that must be shot to burst all balloons_.

**Example 1:**

```
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].
- Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].
```

**Example 2:**

```
Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4
Explanation: One arrow needs to be shot for each balloon for a total of 4 arrows.
```

**Example 3:**

```
Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 2, bursting the balloons [1,2] and [2,3].
- Shoot an arrow at x = 4, bursting the balloons [3,4] and [4,5].
```

> 解法

其实这个题目和区间调度一模一样，因为实际上有多少不重叠区间就需要多少箭来射穿所有气球。**有一个小的区别就是在这个题目中一个区间的`end`和另一个区间的`start`相等也视为重叠**。

### 股票买卖问题

基本问题就是给定一支股票在一段时间内的价格变动，一天内最多进行一次买入或者卖出操作，且仅能操作一股，求所能得到的最大收益。不同的问题会在基本问题的基础上添加更多的约束和条件，比如总交易次数的限制、冷却期的限制等。

下面来分析整个问题：

状态：

1. 时间天数
2. 剩余交易次数
3. 当前是否持有

选择：

1. 不操作
2. 买入
3. 卖出

状态转移：

![image-20211120205913590](https://c/Users/jiaoy/AppData/Roaming/Typora/typora-user-images/image-20211120205913590.png)

图中的`0`代表不持有股票，`1`代表持有股票。选择推动状态转移，上图代表选择和状态之间的关系

状态转移方程：

```java
/* 
dp[i][j][0]代表到第 i 天，剩余 j 次交易，且不持有股票所能得到的最大收益
dp[i][j][1]代表到第 i 天，剩余 j 次交易，且持有股票所能得到的最大收益
*/
int[][][] dp;
int[] prices; // prices[i]代表第 i 天股票价格
// 选择今天卖出股票的情况下，剩余交易次数要减一是因为由于今天进行了一次交易（卖出），所以前 i-1 天最多只能进行 j-1 次交易
dp[i][j][0] = Math.max(dp[i-1][j][0], dp[i-1][j-1][1] + prices[i])
dp[i][j][1] = Math.max(dp[i-1][j][1], dp[i-1][j][0] - prices[i])
```

#### 121 Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

> 解法

此题目相当于允许的操作次数`k`为1

#### 122 Best Time to Buy and Sell Stock II

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return _the **maximum** profit you can achieve_.

> 解法

此题目相当于允许的操作次数`k`为无穷次，于是可以直接去掉`k`这个状态

#### 123 Best Time to Buy and Sell Stock III

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

> 解法

此题目相当于允许的操作次数`k`为2

#### 188 Best Time to Buy and Sell Stock IV

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `k`.

Find the maximum profit you can achieve. You may complete at most `k` transactions.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

> 解法

此题目相当于允许的操作次数`k`就是给定的`k`

#### 309 Best Time to Buy and Sell Stock with Cooldown

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

* After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

> 解法

此题目相当于允许的操作次数`k`是无穷，可以直接删掉操作次数`k`这个状态参数。同时，由于冷却期的存在，状态转移方程需要进行调整

```java
/* 
dp[i][0]代表到第 i 天且不持有股票所能得到的最大收益
dp[i][1]代表到第 i 天，且持有股票所能得到的最大收益
*/
int[][] dp;
int[] prices; // prices[i]代表第 i 天股票价格
dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
// 在持有股票的状态下，如果是第 i 天购买股票，那么 i-1 天只能是不操作，那么 i-2 天也就没持有股票
dp[i][1] = Math.max(dp[i-1][1], dp[i-2][0] - prices[i]);
```

#### 714 Best Time to Buy and Sell Stock with Transaction Fee

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

> 解法

此题目相当于允许的操作次数`k`是无穷，可以直接删掉操作次数`k`这个状态参数。同时，由于交易费用的存在，状态转移方程需要进行调整

```java
/* 
dp[i][0]代表到第 i 天且不持有股票所能得到的最大收益
dp[i][1]代表到第 i 天，且持有股票所能得到的最大收益
*/
int[][] dp;
int fee; // 交易费用可以认为是在
int[] prices; // prices[i]代表第 i 天股票价格
dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i] - fee);
dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] - prices[i]);
```

### House Robber问题

基本问题就是给定一组店铺内的现金，求打劫最多可以得到的数目

状态：

1. 店铺位置
2. 当前总打劫现金数目

选择：

1. 是否打劫当前店铺

状态转移方程：

```java
/*
dp[i][1]代表到店铺 i 为止，且打劫店铺 i，所能得到的最大抢劫现金
dp[i][0]代表到店铺 i 为止，且不打劫店铺 i，所能得到的最大抢劫现金
*/
int[][] dp;
int[] cash; // cash[i]代表店铺 i 的现金数目
dp[i][1] = dp[i-1][0] + cash[i];
dp[i][0] = Math.mac(dp[i-1][0], dp[i-1][1])
```

#### 198 House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

> 解法

#### 213. House Robber II

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 3:**

```
Input: nums = [1,2,3]
Output: 3
```

> 解法

现在店铺的分布变成了环形，使得首尾的两家店铺不可以同时被抢，而这样就变化出了三种情况：

1. 首尾都不被抢，店铺为\[1, n-1]
2. 首个店铺不被抢，店铺为\[1, n]
3. 最后一个店铺不被抢。店铺为\[0, n-1]

事实上这三个问题都可以用前面一题的方法分别解决。

**但是第二第三种情况的收益是一定大于等于第三种情况的，且可以把第二第三种情况抽象成给定抢劫范围，求最大抢劫金额**

```java
public int maxRob(int[] cash, int from, int to)
```

#### 337. House Robber III

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called `root`.

Besides the `root`, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if **two directly-linked houses were broken into on the same night**.

Given the `root` of the binary tree, return _the maximum amount of money the thief can rob **without alerting the police**_.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

```
Input: root = [3,2,3,null,3,null,1]
Output: 7
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)

```
Input: root = [3,4,5,1,3,null,1]
Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
```

> 解法

遇到二叉树问题首先想到就是采用递归的写法，于是采用递归 + 记忆化的方法

```java
Map<TreeNode, Integer> memo = new HashMap<>();
public int rob(TreeNode root) {
    if (root == null) return 0;
    // 利⽤备忘录消除重叠⼦问题
    if (memo.containsKey(root))
        return memo.get(root);
    // 抢，然后去下下家
    int do_it = root.val;
    if (root.left != null) {
        do_it += rob(root.left.left) + rob(root.left.right);
    }
    if (root.right != null) {
        do_it += rob(root.right.left) + rob(root.right.right);
	}
    // 不抢，然后去下家
    int not_do = rob(root.left) + rob(root.right);
    int res = Math.max(do_it, not_do);
    memo.put(root, res);
    return res;
}
```

```java
int rob(TreeNode root) {
    int[] res = dp(root);
    return Math.max(res[0], res[1]);
}
/* 返回⼀个⼤⼩为 2 的数组 arr
arr[0] 表⽰不抢 root 的话，得到的最⼤钱数
arr[1] 表⽰抢 root 的话，得到的最⼤钱数 */
int[] dp(TreeNode root) {
    if (root == null)
    return new int[]{0, 0};
    int[] left = dp(root.left);
    int[] right = dp(root.right);
    // 抢，下家就不能抢了
    int rob = root.val + left[0] + right[0];
    // 不抢，下家可抢可不抢，取决于收益⼤⼩
    int not_rob = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
    return new int[]{not_rob, rob};
}
```
