---
title: "AVL tree 学习笔记"
date: 2019-03-26
lastmod: 2019-03-26
draft: false
categories: ["数据结构与算法"]
author: "sherry"
---

# 前言

最近在复习数据结构，学习了一下AVL树，现记录如下。

## 什么是AVL树

AVL树是一种平衡二叉查找树(self-balancing binary search tree),由苏联两位科学家[Georgy Adelson-Velsky](https://en.wikipedia.org/wiki/Georgy_Adelson-Velsky)和[Evgenii Landis](https://en.wikipedia.org/wiki/Evgenii_Landis)于1962年在论文《[An algorithm for the organization of information](http://professor.ufabc.edu.br/~jesus.mena/courses/mc3305-2q-2015/AED2-10-avl-paper.pdf)》中首次提出。

<!--more-->

## AVL树的性质

AVL是一个平衡二叉查找树，首先它应是一个二叉查找树（又称二叉排序树），因此，它首先具备如下特性:

- 若左子树不空，则左子树上所有结点的值均小于它的根结点的值。
- 若右子树不空，则右子树上所有结点的值均大于它的根结点的值。
- 左、右子树也分别为二叉排序树。

AVL引入了平衡因子，即：

- 每个节点的左右子树高度之间的差异小于或等于1。

## AVL树的旋转

通过对节点的旋转来重新平衡树，通常对AVL树的旋转有以下四种情况：

LL(Left Left Case):

![LL](http://wx1.sinaimg.cn/large/ea5eda6dly1g1gm6mz4y3j20ix04m0sq.jpg)

LR(Left Right Case):

![LR](http://wx1.sinaimg.cn/large/ea5eda6dly1g1gm6mywftj20cf0bdt8v.jpg)

RR(Right Right Case):

![RR](http://wx1.sinaimg.cn/large/ea5eda6dly1g1gm6mxjg7j20il04hweh.jpg)

RL(Right Left Case):

![RL](http://wx2.sinaimg.cn/large/ea5eda6dly1g1gm6my92oj20c00azjri.jpg)

_p.s. 以上图片通过draw.io绘制而成，凑活能看（_

再放一张gif，便于理解，图源来自Wikipedia:

![AVL](http://wx2.sinaimg.cn/large/ea5eda6dly1g1f6ae2fq2g208w050dm1.gif)

## AVL树的时间空间复杂度

以下图表来源于Wikipedia.

| Algorithm | Average | Worst case |
|:---:|:---:|:---:|
| space | O(n) | O(n) |
| search | O(log n) | O(log n) |
| insert | O(log n) | O(log n) |
| delete | O(log n) | O(log n) |

## 参考资料

> https://en.wikipedia.org/wiki/AVL_tree  
> https://www.geeksforgeeks.org/avl-tree-set-1-insertion/