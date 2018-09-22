# Binary Tree

## 1.先序遍历

非递归：
```
public List<Integer> preorder(TreeNode root){
    Stack<TreeNode> stack = new Stack<TreeNode>();
    List<Integer> res = new ArrayList<Integer>();
    if(root == null){
        return res;
    }
    stack.push(root);
    while(!stack.isEmpty()){
        TreeNode tmp = stack.pop();
        res.add(tmp.val);
        if(tmp.right != null){
            stack.push(tmp.left);
        }
        if(tmp.right != null){
            stack.push(tmp.right);
        }
    }
    return res;
}
```
递归：
```
public List<Integer> preorder(TreeNode root){
    List<Integer> res = new ArrayList<Integer>();
    preorder(root,res);
    return res;
}
private void preorder(TreeNode root, List<Integer> res){
    if(root == null){
        return res;
    }
    res.add(root.val);
    preorder(root.left);
    preorder(root.right);
}
```
分治：
```
public ArrayList<Integer> preorder(TreeNode root){
    ArrayList<Integer> res = new ArrayList<Integer>();
    if(root == null){
        return res;
    }
    
    ArrayList<Integer> left = preorder(root.left);
    ArrayList<Integer> right = preorder(root.right);
    
    res.add(root.val);
    res.addAll(left);
    res.addAll(right);
    return res;
}
```


## DFS
通常递归实现，分为遍历和分治两种，一下为模板：

**遍历：**
```
public void dfs(TreeNode root){
    if(root == null){
        return;
    }
    //preorder: do something with root
    dfs(root.left);
    //inorder:do something with root
    dfs(root.right);
    //lastorder:do something with root
}
```
**分治：**
```
public ResultType dfs(TreeNode root){
    //null or leaf
    if(root == null){
        //do something and return
        return root;
    }
    
    //Divide
    ResultType left = dfs(root.left);
    ResultType right = dfs(root.right);
    
    //Conquer
    ResultType res = Merge from left to right
    return res;
}
```

## BFS
用到队列，很简单，下面是模板：
```
public ArrayLisy<ArrayList<Integer>> leverOrder(TreeNode root){
    ArrayList res = new ArrayList();
    if(root == null){
        return res;
    }
    
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.offer(root);
    
    while(!queue.isEmpty(){
        ArrayList<Integer> level = new ArrayList<Integer>();
        int size = queue.size();
        for(int i = 0; i < size; i++){
            TreeNode head = queue.poll();
            level.add(head.val);
            if(head.left != null){
                queue.offer(head.left);
            }
            if(head.right != null){
                queue.offer(head.right);
            }
        }
        res.add(level);
    }
    return res;
}
```
## Binary Search Tree
特点是左子树的节点值比根节点的小，右子树的节点的值比根节点的大。中序遍历是有序的。