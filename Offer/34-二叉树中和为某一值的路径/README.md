# 34. 二叉树中和为某一值的路径 

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。



示例:
给定如下二叉树，以及目标和 sum = 22，

          5
         / \
        4   8
       /   / \
      11  13  4
     /  \    / \
    7    2  5   1
返回：

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

提示：

`数组长度 <= 10000`


链接：https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解析

数据结构：栈。

本质上即为树的遍历，并在遍历过程中记录当前遍历的路径值。

递归函数主要执行以下三个步骤：

1. 接受当前节点数据，数据入栈，记录当前路径数据和。
2. 是否同时满足：
   1. 当前节点为叶子结点
   2. 当前路径和与题目路径值相等
3. 继续进行子节点的递归遍历。
4. 执行以上步骤后，弹出栈顶元素。



## 题解

### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private List<List<Integer>> res;

    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        res = new ArrayList<>(); //动态数组
        findPath(root, sum, new ArrayList<>());
        return res;
    }

    private void findPath(TreeNode node, int sum, List<Integer> collector){
        //boolean isLeaf = (node.left == null && node.right == null);//判断当前节点是否为叶子节点。
        if(node == null)
            return;
        
        sum -= node.val;
        collector.add(node.val);
        
        if(sum == 0 && node.left == null && node. right == null){
            res.add(new ArrayList<>(collector));
        }else{
            findPath(node.left, sum, collector);
            findPath(node.right, sum, collector);
        }   
        collector.remove(collector.size() - 1);
    }
}
```



### C++

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> res;
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<int> path;//record the path.
        int currentSum = 0;
        FindPath(root, currentSum, path, sum);
        return res;
    }
    void FindPath(TreeNode* root, int currentSum, vector<int>& path, int TotalSum){
        if(root == NULL)
            return;

        currentSum += root -> val;
        path.push_back(root -> val);

        //decide
        bool isLeaf = (root -> right == NULL && root -> left == NULL);
        if(currentSum == TotalSum && isLeaf){
            res.push_back(path);
            path.pop_back();
            return;
        }

        //if not, continue
        if(root -> left != NULL)
            FindPath(root -> left, currentSum, path, TotalSum);
        if(root -> right != NULL)
            FindPath(root -> right, currentSum, path, TotalSum);
        
        //after all, pop the top stack
        path.pop_back();
    }
};
```



