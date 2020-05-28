---
title: BFS和DFS
date: 2020-05-19 10:06:03
categories: 数据结构与算法
tags: 
	- BFS
	- DFS
	- LeetCode
---

下面我们通过一个二叉树层序遍历的题目来分析一下BFS和DFS方法的区别。

# [例题](https://leetcode-cn.com/problems/binary-tree-level-order-traversal)

给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。

示例：
二叉树：[3,9,20,null,null,15,7],

        3
       / \
      9  20
        /  \
       15   7

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

<!-- more -->

左边是BFS，按照层进行搜索；右边是DFS，先一路走到底，当无路可走则回溯上来，走其他路。

![image-20200519101430606](https://yuanchangjian.github.io/cloudImage/images/20200519101431.png)



# BFS（breadth First Search）广度优先搜索

BFS总共有两个通用模板。

1.如果不需要确定当前遍历到了哪一层，BFS模板如下：

```
while queue 不空：
    cur = queue.pop()
    for 节点 in cur的所有相邻节点：
        if 该节点有效且未访问过：
            queue.push(该节点)
```

2.如果要确定当前遍历到了哪一层，BFS模板如下：

```
level = 0
while queue 不空：
    size = queue.size()
    while (size --) {
        cur = queue.pop()
        for 节点 in cur的所有相邻节点：
            if 该节点有效且未被访问过：
                queue.push(该节点)
    }
    level ++;
```

本题要求二叉树的层次遍历，所以同一层的节点应该放在一起，故使用模板二。

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
var levelOrder = function(root) {
    var result = [];
    if (!root) return result;
    var queue = [];
    queue.push(root);
    while (queue.length !== 0) {
        var length = queue.length;
        result.push([]);
        for (var i = 0; i < length; i++) {
            var treeNode = queue.shift();
            result[result.length - 1].push(treeNode.val);
            if (treeNode.left) queue.push(treeNode.left);
            if (treeNode.right) queue.push(treeNode.right);
        }
    }
    return result;
};
```

实现过程如下所示，我们先将根节点放到队列中。

![image-20200519110046014](https://yuanchangjian.github.io/cloudImage/images/20200519110047.png)

首先拿出根节点，如果左子树/右子树不为空，就将他们放入队列中。第一遍处理完后，根节点已经从队列中拿走了，而根节点的两个孩子已放入队列中了，现在队列中就有两个节点 `2` 和 `5`。

![image-20200519110133339](https://yuanchangjian.github.io/cloudImage/images/20200519110134.png)

第二次处理，会将 `2` 和 `5` 这两个节点从队列中拿走，然后再将 2 和 5 的子节点放入队列中，现在队列中就有三个节点 `3`，`4`，`6`。

![image-20200519110206085](https://yuanchangjian.github.io/cloudImage/images/20200519110207.png)

我们把每层遍历到的节点都放入到一个结果集中，最后返回这个结果集就可以了。

时间复杂度： O(n)*O*(*n*)
空间复杂度：O(n)*O*(*n*)



# DFS（Deep First Search）深度优先搜索

![0203_1.gif](https://pic.leetcode-cn.com/aeed09e12573ec00d83663bb4f77562e8904ac58cdb2cbe6e995f2ac33b12934-0203_1.gif)

 DFS 不是按照层次遍历的。为了让递归的过程中同一层的节点放到同一个列表中，在递归时要记录每个节点的深度 level。递归到新节点要把该节点放入 level 对应列表的末尾。

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
var levelOrder = function(root) {
    var result = [];
    if (!root) return result;

    var dfs = function(node, level) {
        if (result.length - 1 < level) {
            result.push([]);
        }

        result[level - 1].push(node.val);
        if (node.left) dfs(node.left, level + 1);
        if (node.right) dfs(node.right, level + 1);
    }

    dfs(root, 0);

    return result;
};
```

时间复杂度：O(N)*O*(*N*)
空间复杂度：O(h)*O*(*h*)，`h` 是树的高度

# 总结

## 数据结构上的运用

DFS用递归的形式，用到了栈结构，先进后出。

BFS选取状态用队列的形式，先进先出。



## 复杂度

DFS的复杂度与BFS的复杂度大体一致，不同之处在于遍历的方式与对于问题的解决出发点不同，DFS适合目标明确，而BFS适合大范围的寻找。



## 思想

思想上来说这两种方法都是穷竭列举所有的情况。



## 使用场景

DFS暂遇到的两个场景：层序遍历和最短路径（多源BFS）



# 参考

* [二叉树层序遍历（leetcode）](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

* [套模板！BFS 和 DFS 都可以解决](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/tao-mo-ban-bfs-he-dfs-du-ke-yi-jie-jue-by-fuxuemin/)
* [BFS 的使用场景总结：层序遍历、最短路径问题](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/bfs-de-shi-yong-chang-jing-zong-jie-ceng-xu-bian-l/)
* [搜索思想——DFS & BFS（基础基础篇）](https://zhuanlan.zhihu.com/p/24986203)

