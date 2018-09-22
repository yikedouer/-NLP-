*Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.*

# 整体思路如下：
1. 对树s来说，判断当前状态能否匹配到树t，能的话直接返回true。具体匹配由方法由helper函数实现
2. 如果s的当前状态不能匹配，则分别遍历左子树和右子树，二者有一个返回真即可。边界判断如下;
s和t有一个为空就返回false
s和t同时为空返回true
3. helper方法：
- 逐一判断s和t的节点，只有当量节点的val值相等，才继续分别往下走。
- 往下走时，只有左右子树同事为真菜完全匹配到了
- 如果两者都为空，说明完全匹配了
只要有一个为空，就失败
```
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
    public boolean isSubtree(TreeNode s, TreeNode t) {
        if (s == null && t == null) return true;
        if (s == null || t == null) return false;
        if(helper(s,t)){//判断当前状态是否满足子树匹配
            return true;
        }
        return isSubtree(s.left,t) || isSubtree(s.right,t); //上一个状态没找到，分别遍历左子树和右子树
    }
    private boolean helper(TreeNode sRoot,TreeNode tRoot){
        //只有当s和t的当前节点值相等才会往下走
        //两个数都走空了，说明完全匹配
        if(sRoot == null && tRoot == null){
            return true;
        }
        if(sRoot == null || tRoot == null){
            return false;
        }
        if(sRoot.val != tRoot.val){
            return false;
        }
        //当前节点匹配，继续分别匹配左子树和右子树，只有二者都完全匹配才返回真
        return helper(sRoot.left,tRoot.left) && helper(sRoot.right, tRoot.right);
    }
}
```