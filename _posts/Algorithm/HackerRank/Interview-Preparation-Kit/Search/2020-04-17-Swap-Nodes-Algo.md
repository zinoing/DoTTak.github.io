---
layout: post
title: "[Algorithm/HackerRank] Search - Swap Nodes Algo"
date: 2020-04-17 10:08:21 +0900
categories: 
    - Algorithm
tags:
    - Algorithm
    - HackerRank
    - Search
toc: true
---

<!-- more -->


## Probleam
A binary tree is a tree which is characterized by one of the following properties:

- It can be empty (null).
- It contains a root node only.
- It contains a root node with a left subtree, a right subtree, or both. These subtrees are also binary trees.

In-order traversal is performed as
1. Traverse the left subtree.
2. Visit root.
3. Traverse the right subtree.

For this in-order traversal, start from the left child of the root node and keep exploring the left subtree until you reach a leaf. When you reach a leaf, back up to its parent, check for a right child and visit it if there is one. If there is not a child, you've explored its left and right subtrees fully. If there is a right child, traverse its left subtree then its right in the same manner. Keep doing this until you have traversed the entire tree. You will only store the values of a node as you visit when one of the following is true:

- it is the first node visited, the first time visited
- it is a leaf, should only be visited once
- all of its subtrees have been explored, should only be visited once while this is true
- it is the root of the tree, the first time visited
**Swapping:** Swapping subtrees of a node means that if initially node has left subtree L and right subtree R, then after swapping, the left subtree will be R and the right subtree, L.

For example, in the following tree, we swap children of node 1.
```

                                Depth
    1               1            [1]
   / \             / \
  2   3     ->    3   2          [2]
   \   \           \   \
    4   5           5   4        [3]
```

In-order traversal of left tree is 2 4 1 3 5 and of right tree is 3 5 1 2 4.

**Swap operation:**
We define depth of a node as follows:
- The root node is at depth 1.
- If the depth of the parent node is d, then the depth of current node will be d+1.

Given a tree and an integer, k, in one operation, we need to swap the subtrees of all the nodes at each depth h, where h ??? [k, 2k, 3k,...]. In other words, if h is a multiple of k, swap the left and right subtrees of that level.

You are given a tree of n nodes where nodes are indexed from [1..n] and it is rooted at 1. You have to perform t swap operations on it, and after each swap operation print the in-order traversal of the current state of the tree.

## Function Description
Complete the swapNodes function in the editor below. It should return a two-dimensional array where each element is an array of integers representing the node indices of an in-order traversal after a swap operation.

swapNodes has the following parameter(s):
- indexes: an array of integers representing index values of each `node[i]`, beginning with `[node[1]`, the first element, as the root.
- queries: an array of integers, each representing a `k` value.

## Input Format
The first line contains n, number of nodes in the tree.

Each of the next n lines contains two integers, a b, where a is the index of left child, and b is the index of right child of i^th node.

**Note:** -1 is used to represent a null node.

The next line contains an integer, t, the size of `queries`.
Each of the next t lines contains an integer `queries[i]`, each being a value `k`.

## Output Format
For each k, perform the swap operation and store the indices of your in-order traversal to your result array. After all swap operations have been performed, return your result array for printing.

## Sample Input 1
```

11
2 3
4 -1
5 -1
6 -1
7 8
-1 9
-1 -1
10 11
-1 -1
-1 -1
-1 -1
2
2
4
```


## Sample Output 1
```

2 9 6 4 1 3 7 5 11 8 10
2 6 9 4 1 3 7 5 10 8 11
```


## Explanation 1
Here we perform swap operations at the nodes whose depth is either 2 or 4 for `k=2` and then at nodes whose depth is 4 for `k=4`.
```

         1                     1                          1             
        / \                   / \                        / \            
       /   \                 /   \                      /   \           
      2     3    [s]        2     3                    2     3          
     /      /                \     \                    \     \         
    /      /                  \     \                    \     \        
   4      5          ->        4     5          ->        4     5       
  /      / \                  /     / \                  /     / \      
 /      /   \                /     /   \                /     /   \     
6      7     8   [s]        6     7     8   [s]        6     7     8
 \          / \            /           / \              \         / \   
  \        /   \          /           /   \              \       /   \  
   9      10   11        9           11   10              9     10   11 
```

## Code

```python
 #!/bin/python3

import os
import sys

# Discussions?????? ?????? ???
sys.setrecursionlimit(15000)


class Node:
    def __init__(self, data):
        self.data = data
        self.left = -1
        self.right = -1
        self.depth = 0

    def __repr__(self):
        return str(self.data)+"Node"


class Tree:
    def __init__(self, root):
        self.root = root
        self.result = []
    
    def tree_order(self, node):
        if node == -1:
            return
        self.tree_order(node.left)
        if node.data != -1:
            self.result.append(node.data)
        self.tree_order(node.right)

#
# Complete the swapNodes function below.
#
def swapNodes(indexes, queries):
    """
    ?????? URL: http://blog.naver.com/PostView.nhn?blogId=weplayicecream&logNo=221457948572&parentCategoryNo=&categoryNo=22&viewDate=&isShowPopularPosts=false&from=postView
    [??????]
    ?????? ????????? ?????? ??? ?????? depth??? swap ?????? ??? 
    ???, ?????? k?????? ????????? ?????? n??? ??? depth??? k??? ????????? ???????????? ???????????? swap
        -> depth??? 6??? ?????? ?????? k??? 2??? ???????????? 2 depth, 4 depth, 6 depth??? ??? ???????????? swap
    
    For Example:
                            1          - depth 1
                          //  \\
                         2      3      - depth 2 <- depth 2??? ???????????? swap
                       //      //\\
                       4      5    6   - depth 3
    ?????? ????????? ?????? ?????? depth 2??? swap ?????? ?????? depth 2??? ????????????(left, right)??? swap ?????? ??????. 
    ????????? swap??? ????????? ???????????? In Order ????????????. 
                            1          - depth 1
                          //  \\
                         2      3      - depth 2 
                         \\    //\\
                          4   6    5   - depth 3
    [????????? ??????] 
    2 > 4 > 1 > 6 > 3 > 5
    [????????? ?????? ??????]
    ?????? ???????????? In Order(Left -> Root -> Right) ????????? ?????????
          1          - depth 1
        //  \\
       2      3      - depth 2 
     //\\    //\\
    [A]  4   6    5   - depth 3     

    A > 2 > 4 > 1 > 6 > 3 > 5 ?????? ????????? A??? ?????? ??????????????? A??? ???
    2 > 4 > 1 > 6 > 3 > 5 ??? ?????????.

    
    [?????? ?????? ??????]
    [1] indexes??? ?????? ????????? ????????????.
    [2] depth??? node??? ????????? ??? ?????? dict ??????(hashmap ??????) -> key??? depth(??????), value??? ????????? ??????(??????)
    [3] queries??? ?????? ????????? ???????????? ????????? depth??? ??? ????????? ????????? ??? ?????? queries??? query??? ?????? depth??? ??? ????????? ??????
        ?????? ????????? swap ??????.
    [4] ????????? ????????? ??? In Order ???????????? ???????????? ????????????.
    """
    result = []

    node_arr = []       # Node??? ????????? ?????? ??????
    depth_node_arr = {}

    # ????????? ?????? ??????
    root = Node(1)
    root.depth = 1
    depth_node_arr[root.depth] = [root]

    # ?????? ??????
    tree = Tree(root)

    # ?????? ??????
    node_arr.append(root)
    for i, nodes in enumerate(indexes):
        
        cur_node = node_arr[i]  # ?????? ??????

        left = nodes[0]
        right = nodes[1]
        # print(cur_node.data, left, right)
        
        if left != -1:
            cur_node.left = Node(left)
            cur_node.left.depth = cur_node.depth + 1
            node_arr.append(cur_node.left)  # ?????? ???????????? ????????? ????????? ??? ???????????? ????????? ?????? ????????????.
            
            # Depth??? ?????? ??????
            if cur_node.left.depth in depth_node_arr.keys():
                depth_node_arr[cur_node.left.depth].append(cur_node.left)
            else:
                depth_node_arr[cur_node.left.depth] = [cur_node.left]

        if right != -1:
            cur_node.right = Node(right)
            cur_node.right.depth = cur_node.depth + 1
            node_arr.append(cur_node.right) # ????????? ???????????? ????????? ????????? ??? ???????????? ????????? ?????? ????????????.
            
            # Depth??? ?????? ??????
            if cur_node.right.depth in depth_node_arr.keys():
                depth_node_arr[cur_node.right.depth].append(cur_node.right)
            else:
                depth_node_arr[cur_node.right.depth] = [cur_node.right]
    
    # print(depth_node_arr)

    # Swap
    for query in queries:
        for i, depth in enumerate(depth_node_arr):
            if depth % query == 0:
                for node in depth_node_arr[depth]:
                    # swap 
                    temp = node.left
                    node.right, node.left = temp, node.right
                    # print(node.left, node.right)
        
        # print("\n<<<      Inorder      >>>")
        # Inorder(????????????)????????? left -> root -> right
        
        tree.tree_order(root)
        result.append(tree.result)
        tree.result = []
            
    return result
if __name__ == '__main__':
    # fptr = open(os.environ['OUTPUT_PATH'], 'w')

    # n = int(input())

    # indexes = []

    # for _ in range(n):
    #     indexes.append(list(map(int, input().rstrip().split())))

    # queries_count = int(input())

    # queries = []

    # for _ in range(queries_count):
    #     queries_item = int(input())
    #     queries.append(queries_item)

    # result = swapNodes(indexes, queries)
    # result = swapNodes([[2,3], [-1,4], [-1, 5], [-1, -1], [-1, -1]], [2])
    result = swapNodes([[2, 3],[4 ,-1],[5 ,-1],[6 ,-1],[7 ,8],[-1, 9],[-1, -1],[10, 11],[-1, -1],[-1, -1],[-1, -1]], [2, 4])

    # fptr.write('\n'.join([' '.join(map(str, x)) for x in result]))
    # fptr.write('\n')

    # fptr.close()

```
