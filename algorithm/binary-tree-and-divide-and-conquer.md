---
description: Lesson 3 Binary Tree and Divide & Conquer
---

# 二叉树与分治法

## 二叉树

### 递归遍历

#### 前序遍历

前序遍历是指首先遍历父节点然后遍历左子节点、右子节点

```java
// 递归
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;
    traverse(root, res);
    return res;
}

public void traverse(TreeNode root, List<Integer> res) {
    if (root == null) return;
    res.add(root.val);
    traverse(root.left, res);
    traverse(root.right, res);
}
// 非递归，使用stack
public List<Integer> preorderTraversal(TreeNode root) {
    Stack<TreeNode> stack = new Stack<TreeNode>();
    List<Integer> preorder = new ArrayList<Integer>();

    if (root == null) {
        return preorder;
    }

    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        preorder.add(node.val);
        if (node.left != null) {
            stack.push(node.left);
        }
        if (node.right != null) {
            stack.push(node.right);
        }
    }

    return preorder;
}
```

#### 中序遍历

中序遍历是指首先遍历左子节点，然后遍历父节点，最后遍历右子节点

```java
// 递归
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;
    traverse(res, root);
    return res;
}

public void traverse(List<Integer> res, TreeNode node) {
    if (node == null) return;
    traverse(res, node.left);
    res.add(node.val);
    traverse(res, node.right);
}
// 非递归，采用Stack
public ArrayList<Integer> inorderTraversal(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    ArrayList<Integer> result = new ArrayList<>();
    while (root != null) {
        stack.push(root);
        root = root.left;
    }

    while (!stack.isEmpty()) {
        TreeNode node = stack.peek();
        result.add(node.val);
        if (node.right == null) {
            node = stack.pop();
            while (!stack.isEmpty() && stack.peek().right == node) {
                node = stack.pop();
            }
        } else {
            node = node.right;
            while (node != null) {
                stack.push(node);
                node = node.left;
            }
        }
    }
    return result;
}
public ArrayList<Integer> inorderTraversal(TreeNode root) {
    Stack<TreeNode> stack = new Stack<TreeNode>();
    ArrayList<Integer> result = new ArrayList<Integer>();
    TreeNode curt = root;
    while (curt != null || !stack.empty()) {
        while (curt != null) {
            stack.add(curt);
            curt = curt.left;
        }
        curt = stack.pop();
        result.add(curt.val);
        curt = curt.right;
    }
    return result;
}
```

#### 后序遍历

后序遍历是指首先遍历左子节点、右子节点，然后遍历父节点

```java
// 递归
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;
    traverse(res, root);
    return res;
}

public void traverse(List<Integer> res, TreeNode node) {
    if (node == null) return;
    traverse(res, node.left);
    traverse(res, node.right);
    res.add(node.val);
}

public ArrayList<Integer> postorderTraversal(TreeNode root) {
    ArrayList<Integer> result = new ArrayList<Integer>();
    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode prev = null; // previously traversed node
    TreeNode curr = root;
    if (root == null) return result;

    stack.push(root);
    while (!stack.isEmpty()) {
        curr = stack.peek();
        if (prev == null || prev.left == curr || prev.right == curr) { // traverse down the tree
            if (curr.left != null) {
                stack.push(curr.left);
            } else if (curr.right != null) {
                stack.push(curr.right);
            }
        } else if (curr.left == prev) { // traverse up the tree from the left
            if (curr.right != null) {
                stack.push(curr.right);
            }
        } else { // traverse up the tree from the right
            result.add(curr.val);
            stack.pop();
        }
        prev = curr;
    }

    return result;
}
```

### 递归分治

**分治**是指将一个复杂的问题分成两个或多个**相同或相似**的**子问题**直到最终子问题**可以简单的直接求解**，而原问题的解即为**子问题解的合并**。

> divide conquer 和 traversal的区别是什么？
>
> 这两个算法都是采用递归的写法，但是遍历一般会定义一个全局变量，在整个遍历过程中，不断更新这个值，最终达到求解的目的；而分治法则是会将一个大问题转化成多个小问题的求解，然后将小问题的解进行组合得到大问题的解，通常会采用带有返回值的函数。
>
> 举个例子：假如说要调查全国人口总数。采用divide conquer就是说把任务下发到每个省，每个省得到的结果汇总在一起得到全国的结果；而采用traversal就是派一个人到每个去调查每个省的人口，一个一个人口的调查，最后得到最终结果。

![image-20211126220118026](https://c/Users/jiaoy/AppData/Roaming/Typora/typora-user-images/image-20211126220118026.png)

## 例题

### 104. Maximum Depth of Binary Tree

Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![image-20211126220525925](https://c/Users/jiaoy/AppData/Roaming/Typora/typora-user-images/image-20211126220525925.png)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

> 解法

典型的分治法解决二叉树问题，将对一个父节点的求解拆解成对左子节点和右子节点，以及两个子问题结果的组合

```java
// 遍历
private int depth;
public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    depth = 0;
    helper(root, 1);
    return depth;
}
private void helper(TreeNode root, int curDepth) {
    if (root == null) return;
    depth = Math.max(depth, curDepth);
    helper(root.left, curDepth + 1);
    helper(root.right, curDepth + 1);
}
// 分治法
public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    int res = 0;
    int left = maxDepth(root.left);
    int right = macDepth(root.right);
    res = Math.max(left, right) + 1;
    return res;
}
```

### 257. Binary Tree Paths

Given the `root` of a binary tree, return _all root-to-leaf paths in **any order**_.

A **leaf** is a node with no children.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
```

> 解法

采用递归遍历法或者递归分治法

```java
// 遍历
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<String>();
        if (root == null) return result;
        StringBuffer tmp = new StringBuffer();
        helper(root, result, tmp);
        return result;
    }
    
    private void helper(TreeNode root, List<String> result, StringBuffer cur) {
        if (cur.length() == 0) {
            cur.append("" + root.val);
        } else {
            cur.append("->" + root.val);
        }
        
        if (root.left == null && root.right == null) {
            result.add(cur.toString());
            return;
        }
        
        if (root.left != null) {
            helper(root.left, result, new StringBuffer(cur));
        }
        if (root.right != null) {
            helper(root.right, result, new StringBuffer(cur));
        }
    }
}
// 分治法
public List<String> binaryTreePaths(TreeNode root) {
    if (root == null) return new ArrayList<String>();
    List<String> result = new ArrayList<String>();
    if (root.left == null && root.right == null) {
        result.add(root.val + "");
        return result;
    }

    List<String> left = binaryTreePaths(root.left);
    List<String> right = binaryTreePaths(root.right);
    for (String str : left) {
        result.add(root.val + "->" + str);
    }      

    for (String str : right) {
        result.add(root.val + "->" + str);
    }
    return result;
}
```

### Minimum Subtree

Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.

> 解法

很显然由于要求最小值，所以采用定义全局变量，在遍历过程中不断更新最小值的遍历解法更为合适

```java
private TreeNode minNode;
private int sum = Integer.MAX_VALUE;

public TreeNode findSubtree(TreeNode root) {
    helper(root);
    return minNode; 
}
private void helper(TreeNode root) {
    if (root == null) return 0;
    int res = helper(root.left) + helper(root.right) + root.val;
    if (res < sum) {
        sum = res;
        minNode = root;
    }

}
```

### 110. Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of _every_ node differ in height by no more than 1.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance\_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

> 解法

当前 root 是否balanced取决于左右两个 subtree ，当两个subtree都是balanced同时两个subtree depth之差小于等于1时，root是balanced。分析可知，为了可以将两个子问题的结果组合出父问题的结果，不但需要知道子节点是不是高度平衡的，同时还需要知道子节点的树的高度是多少。如果采用一般的返回一个结果的函数写法是不行的，所以自定义一个类型来封装这两个参数

```java
class resultType {
    public boolean isBalanced;
    public int depth;
    public resultType(boolean isBalanced, int depth) {
        this.isBalanced = isBalanced;
        this.depth = depth;
    }
}

class Solution {
    public boolean isBalanced(TreeNode root) {
        resultType res = helper(root);
        return res.isBalanced;
    }
    // helper function return the pre-defined resultType
    public resultType helper(TreeNode root) {
        if (root == null) return new resultType(true, 0);
        
        resultType left = helper(root.left);
        resultType right = helper(root.right);
        int length = Math.max(left.depth, right.depth) + 1;
        if (left.isBalanced == false || right.isBalanced == false)  {
            return new resultType(false, length);
        } 
        if (Math.abs(left.depth - right.depth) > 1) {
            return new resultType(false, length);
        }
        return new resultType(true, length);
    }
}
```

### 1120. Maximum Average Subtree

Given the root of a binary tree, find the maximum average value of any subtree of that tree. (A subtree of a tree is any node of that tree plus all its descendants. The average value of a tree is the sum of its values, divided by the number of nodes.)

**Example 1:** ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191227113725648.png)

```
Input: [5,6,1]
Output: 6.00000
Explanation: 
For the node with value = 5 we have an average of (5 + 6 + 1) / 3 = 4.
For the node with value = 6 we have an average of 6 / 1 = 6.
For the node with value = 1 we have an average of 1 / 1 = 1.
So the answer is 6 which is the maximum.
```

```
Note:
The number of nodes in the tree is between 1 and 5000.
Each node will have a value between 0 and 100000.
Answers will be accepted as correct if they are within 10^-5 of the correct answer.
```

> 解法

两个子树的和相加，然后除以两个子树的节点总数就可以得到父节点树的平均值。又由于需要求最大平均值，可以采用分治+遍历的写法

```java
class resultType {
    public int num;
    public int sum;
    
    public resultType(int num, int sum) {
        this.sum = sum;
        this.num = num;
    }
}

class Solution {
    double ave = 0.0;
    public double maximumAverageSubtree(TreeNode root) {
        resultType res = helper(root);
        return ave;
    }
    public resultType helper(TreeNode root) {
        if (root == null) {
            return new resultType(0, 0);
        }
        resultType left = helper(root.left);
        resultType right = helper(root.right);
        int num_sum = left.num + right.num + 1;
        int sum_sum = left.sum + right.sum + root.val;

        double tmp = sum_sum/1.0/num_sum;
        ave = Math.max(tmp, ave);
        return new resultType(num_sum, sum_sum);
    }
}
```

### 114. Flatten Binary Tree to Linked List

Given the `root` of a binary tree, flatten the tree into a "linked list":

* The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
* The "linked list" should be in the same order as a pre-order traversal of the binary tree.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

```
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
```

> 解法

采用分治法的写法，关键点在于递归调用`flatten`函数时，一定要把子树当做已经被flatten

```java
public void flatten(TreeNode root) {
    if (root == null) return;
    flatten(root.left);
    flatten(root.right);
    TreeNode temp = root.right;
    root.right = root.left;
    root.left = null;
    
    TreeNode node = root.right;
    while (node.right != null) {
        node = node.right;
    }
    node.right = temp;
}
```

### 236. Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest\_common\_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

> 解法

采用递归的写法，将`lowestCommonAncestor`函数的定义拓展为当`root`子树不能找到最低共同祖先，但是`root`为`p`或`q`的父节点时，返回`p`或`q`

```java
// if lca is found, return lca
// if find q, return q
// if find p, return p
// else return null
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) {
        return null;
    }
    if (root == p || root == q) {
        return root;
    }

    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    // 如果两者同时不为null,那么只有可能left和right分别为
    if (left != null && right != null) {
        return root;
    }
    if (left != null) {
        return left;
    }
    if (right != null) {
        return right;
    }
    return null;
}
```

### 298. Binary Tree Longest Consecutive Sequence

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse). (根据后面提供的示例可以判断这里的consecutive是指连续递增，递减的不算)

**Example 1:**

Input:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191229060032940.png)

Output: 3 Explanation: Longest consecutive sequence path is 3-4-5, so return 3.

**Example 2:**

Input:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191229060052962.png)

Output: 2 Explanation: Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.

> 解法

采用遍历和分治的写法，`helper`函数用来计算`root`子树的最大连续递增序列并返回，同时在过程中更新`longest`的值。`longest`是全局最大连续递增序列长度，由于最长连续递增序列不一定起始于根节点`root`，所以需要维护一个全局最大值`longest`

```java
class Solution {
    public int longest = 0;
    
    public int longestConsecutive(TreeNode root) {
        helper(root);
        return longest;
    }
    
    public void helper(TreeNode root) {
        if (root == null) return 0;
        int left = helper(root.left);
        int right = helper(root.right);
        int curr = 1;
        if (root.left != null && root.left.val == root.val + 1) {
            curr = Math.max(curr, left + 1);
        }
        if (root.right != null && root.right.val == root.val + 1) {
            curr = Math.mac(curr, right + 1);
        }
        longest = Math.max(curr, longest);
        return curr;
    }
}
```

此题目还可以采用另一种写法，定义函数`dfs`更新以`p`为子节点，`parent`为父节点，当前到`parent`节点的最大连续递增序列长度为`length`时，全局最大连续递增序列的长度。

```java
class Solution {
    private int maxLength = 0;
    public int longestConsecutive(TreeNode root) {
        dfs(root, null, 0);
        return maxLength;
    }

    private void dfs(TreeNode p, TreeNode parent, int length) {
        if (p == null) return;
        length = (parent != null && p.val == parent.val + 1) ? length + 1 : 1;
        maxLength = Math.max(maxLength, length);
        dfs(p.left, p, length);
        dfs(p.right, p, length);
    }
}
```

### 549. Binary Tree Longest Consecutive Sequence II

Given a binary tree, you need to find the length of Longest Consecutive Path in Binary Tree.

Especially, this path can be either increasing or decreasing. For example, `[1,2,3,4]` and `[4,3,2,1]` are both considered valid, but the path `[1,2,4,3]` is not valid. On the other hand, the path can be in the child-Parent-child order, where not necessarily be parent-child order.

**Example 1:** Input:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191229094153806.png)

Output: 2 Explanation: The longest consecutive path is \[1, 2] or \[2, 1].

**Example 2:** Input:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191229094216874.png)

Output: 3 Explanation: The longest consecutive path is \[1, 2, 3] or \[3, 2, 1].

Note: All the values of tree nodes are in the range of \[-1e7, 1e7].

> 解法

```java
public class Solution {
    int maxval = 0;
    public int longestConsecutive(TreeNode root) {
        longestPath(root);
        return maxval;
    }
	
	// 返回值为最长增序列和最长减序列
    public int[] longestPath(TreeNode root) {
        if (root == null) return new int[] {0,0};
        int inr = 1, dcr = 1;
        if (root.left != null) {
            int[] l = longestPath(root.left);
            if (root.val == root.left.val + 1)
                dcr = l[1] + 1; // 更新减序列长度
            else if (root.val == root.left.val - 1)
                inr = l[0] + 1; // 更新增序列长度
        }
        if (root.right != null) {
            int[] r = longestPath(root.right);
            if (root.val == root.right.val + 1)
                dcr = Math.max(dcr, r[1] + 1); // 更新减序列长度
            else if (root.val == root.right.val - 1)
                inr = Math.max(inr, r[0] + 1); // 更新增序列长度
        }
        // 这里直接采用dcr + inr - 1计算长度是因为，inc/dcr初值为1，只有在子树和root形成序列才会被更新 
        maxval = Math.max(maxval, dcr + inr - 1); 
        return new int[] {inr, dcr};
    }
}
```

### 113. Path Sum II

Given the `root` of a binary tree and an integer `targetSum`, return _all **root-to-leaf** paths where the sum of the node values in the path equals_ `targetSum`_. Each path should be returned as a list of the node **values**, not node references_.

A **root-to-leaf** path is a path starting from the root and ending at any leaf node. A **leaf** is a node with no children.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
Explanation: There are two paths whose sum equals targetSum:
5 + 4 + 11 + 2 = 22
5 + 8 + 4 + 5 = 22
```

> 解法

典型采用回溯法进行求解的问题。一个节点的左右两个子节点即为两个不同选择分支。**实际上和分治法没有什么关系。**

```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int target) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        List<Integer> path = new ArrayList<>();
        path.add(root.val);
        helper(root, target, path, result, root.val);
        return result;
    }
    
    public void helper(TreeNode root, int target, List<Integer> path, List<List<Integer>> result, int currSum) {
        if (currSum > target) return;
        if (root.left == null && root.right == null) {
            if (currSum == target) {
                result.add(new ArrayList<Integer>(path));
            }
            return;
        }
        if (root.left != null) {
            path.add(root.left.val);
            helper(root.left, target, path, result, currSum + root.left.val);
            path.remove(path.size - 1);
        }
        if (root.right != null) {
            path.add(root.right.val);
            helper(root.right, target, path, result, currSum + root.right.val);
            path.remove(path.size - 1);
        }
    }
}
```

## Binary Search Tree

Binary Search Tree 是一种特殊的 Binary Tree，它的要求是，root 左子树中的值都小于 root，root 右子树的值都大于 root。

## 例题

### 98. Validate Binary Search Tree

Given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.

A **valid BST** is defined as follows:

* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
Input: root = [2,1,3]
Output: true
```

#### 解法一

> 解法

采用分治法，分别判断左子树和右子树是不是`BST`，以及父节点、左子节点、右子节点之间是否满足条件

> 最直观的解法，判断左子树的最大值是否小于根节点的值，根节点的值是否小于右子树的最小值

```java
public boolean isValidBST(TreeNode root) {
    if (root == null) return true;
    if (root.left == null && root.right == null) return true;
    if (!isValidBST(root.left) || !isValidBST(root.right)) {
        return false;
    }
    double leftMax;
    TreeNode left = root.left;
    if (left == null) {
        leftMax = -Double.MAX_VALUE;
    } else {
        while (left.right != null) {
            left = left.right;
        }
        leftMax = left.val;
    }
    
    double rightMax;
    TreeNode right = root.right;
    if (right == null) {
        rightMax = Double.MAX_VALUE;
    } else {
        while (right.left != null) {
            right = right.left;
        }
        rightMax = right.val;
    }
    
    return (leftMax < root.val && rightrMax > root.val);
}
```

> 还有一种写法可以定义 `helper`函数返回三个值，以当前点为`root`的 `subtree` 是不是`BST`，`subtree` 最大值以及最小值

> `LeetCode` 提供了一种写法，不需要定义`resultType`来返回三个值，写法比较巧妙，很难当场想到，可以直接记下来

```java
class Solution {
	// 以node为根节点，是否所有子节点对的值在lower到upper之间，(lower, upper)
	public boolean helper(TreeNode node, Integer lower, Integer upper) {
	  if (node == null) return true;
	
	  int val = node.val;
	  if (lower != null && val <= lower) return false; 
	  if (upper != null && val >= upper) return false;
	  if (! helper(node.right, val, upper)) return false;
	  if (! helper(node.left, lower, val)) return false;
	  return true;
	}

    public boolean isValidBST(TreeNode root) {
        return helper(root, null, null);
    }
}
```

#### 解法二

> 解法

中序遍历时，判断当前节点是否大于中序遍历的前一个节点，如果大于，说明满足`BST`，继续遍历；否则直接返回`false`。

```java
class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        // 访问左子树
        if (!isValidBST(root.left)) {
            return false;
        }
        // 如果当前节点小于等于中序遍历的前一个节点，说明不满足BST，返回 false；否则继续遍历。
        if (root.val <= pre) {
            return false;
        }
        pre = root.val;
        // 访问右子树
        return isValidBST(root.right);
    }
}
```

### 426. Convert Binary Search Tree to Sorted Doubly Linked List

Convert a Binary Search Tree to a sorted Circular Doubly-Linked List in place.

You can think of the left and right pointers as synonymous to the predecessor and successor pointers in a doubly-linked list. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

We want to do the transformation in place. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. You should return the pointer to the smallest element of the linked list.

**Example 1:**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191229111330890.png?x-oss-process=image/watermark,type\_ZmFuZ3poZW5naGVpdGk,shadow\_10,text\_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3NjAyNg==,size\_16,color\_FFFFFF,t\_70)

Input: root = \[4,2,5,1,3]

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191229111347896.png?x-oss-process=image/watermark,type\_ZmFuZ3poZW5naGVpdGk,shadow\_10,text\_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3NjAyNg==,size\_16,color\_FFFFFF,t\_70)

Output: \[1,2,3,4,5] Explanation: The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship.

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191229111426834.png?x-oss-process=image/watermark,type\_ZmFuZ3poZW5naGVpdGk,shadow\_10,text\_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3NjAyNg==,size\_16,color\_FFFFFF,t\_70)

#### 解法一

> 解法

采用递归分治的解法

```java
class Solution {
	// the smallest (first) and the largest (last) nodes
	Node first = null;
	Node last = null;

	// 完成以node为起始转换成doublely linked list，但不进行首尾封闭
	public void helper(Node node) {
	  if (node != null) {
	    // left
	    helper(node.left);
	    // node 
	    if (last != null) {
	      // link the previous node (last) with the current one (node)
	      last.right = node;
	      node.left = last;
	    }
	    else {
	      // keep the smallest node to close DLL later on
	      first = node;
	    }
	    last = node;
	    // right
	    helper(node.right);
	  }
	}

	public Node treeToDoublyList(Node root) {
		if (root == null) return null;
		helper(root);
		// close DLL
		last.right = first;
		first.left = last;
		return first;
	}
}
```

#### 解法二

> 解法

由于最终需要返回一个排好序的双向队列，所以可以采用递归中序遍历的写法

```java
/**
 * Definition for Doubly-ListNode.
 * public class DoublyListNode {
 *     int val;
 *     DoublyListNode next, prev;
 *     DoublyListNode(int val) {
 *         this.val = val;
 *         this.next = this.prev = null;
 *     }
 * }
 */ 

class ResultType {
    DoublyListNode first, last;
    
    public ResultType(DoublyListNode first, DoublyListNode last) {
        this.first = first;
        this.last = last;
    }
}

public class Solution {
    /**
     * @param root: The root of tree
     * @return: the head of doubly list node
     */
    public DoublyListNode bstToDoublyList(TreeNode root) {  
        if (root == null) {
            return null;
        }
        
        ResultType result = helper(root);
        return result.first;
    }
    
    //中序遍历函数 
    public ResultType helper(TreeNode root) {  
        if (root == null) {
            return null;
        }
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);
        DoublyListNode node = new DoublyListNode(root.val);
        ResultType result = new ResultType(null, null);
        
        //构造单链表
        if (left == null) {
            result.first = node;
        } else {
            result.first = left.first;
            left.last.next = node;
            node.prev = left.last;
        }
        if (right == null) {
            result.last = node;
        } else {
            result.last = right.last;
            right.first.prev = node;
            node.next = right.first;
        }
        return result;
    }
}
```
