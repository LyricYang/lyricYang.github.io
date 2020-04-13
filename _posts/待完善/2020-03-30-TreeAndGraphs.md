[TOC]

---

## 树
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\b7648a10-4667-4b49-9724-cd0ca7891d5d.jpg)

### 树的基本术语

- **结点的度:**一个结点拥有子树(或后继结点)的个数称为度.度是结点分支树的表示.
- **树的度:**树中所有结点的度的最大值称为树的度.
- **子结点:**一个结点的子树的根节点(或直接后继结点)称为该结点的子结点.
- **父结点:**一棵子树根结点的前继结点称为父结点.除根结点以外的任何结点有且仅有一个父结点.父结点也称双亲结点.
- **兄弟结点:**属于同一个父结点的若干子结点之间互称兄弟结点.
- **树的深度:**树的最大层次数称为树的深度,也称树的高度.

### 树的类别

**完全二叉树：**若设二叉树的高度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第h层有叶子结点，并且叶子结点都是从左到右依次排布。

**满二叉树: **除了叶结点外每一个结点都有左右子叶且叶子结点都处在最底层的二叉树。

**平衡二叉树:**平衡二叉树又被称为AVL树（区别于AVL算法），它是一棵二叉排序树，且具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\95599e91-03e9-4b50-8141-3e7fc78c16c4.png)


**多路搜索二叉树**
- B树：
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\4a9a5005-e073-4a57-8c81-543f54f22c04.jpg)


- B+树
 1. 有n棵子树的结点中含有n个关键字，每个关键字不保存数据，只用来索引，所有数据都保存在叶子节点。
 2. 所有的叶子结点中包含了全部关键字的信息，及指向含这些关键字记录的指针，且叶子结点本身依关键字的大小自小而大顺序链接。
 3. 所有的非终端结点可以看成是索引部分，结点中仅含其子树（根结点）中的最大（或最小）关键字。通常在B+树上有两个头指针，一个指向根结点，一个指向关键字最小的叶子结点。
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\8eb1c67c-7e77-4c56-8ff8-8abda104ba94.jpg)


- B\*树
 1. 在B+树的非根和非叶子结点再增加指向兄弟的指针；
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\3e0752cd-ac3a-4467-b84d-e55ce0a2866b.jpg)


### 树的遍历
#### 二叉树的前序遍历
>给定一棵二叉树，返回其节点值的前序遍历。
>例如：
>给定二叉树`[1,null,2,3]`，
><pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">   1
>\
>2
>/
>3
></pre>
>返回 `[1,2,3]`。

- 递归方式

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> order = new ArrayList<>();
        preorder(root,order);
        return order;
    }
    
    public void preorder(TreeNode root,List<Integer> res){
        if(root==null) return;
        res.add(root.val);
        preorder(root.left,res);
        preorder(root.right,res);
    }
}
```

- 非递归方式

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> order = new ArrayList<>();
        if(root==null) return order;
        Stack<TreeNode> traversal = new Stack<>();
        traversal.push(root);
        while(!traversal.isEmpty()){
            TreeNode temp = traversal.pop();
            order.add(temp.val);
            if(temp.left!=null) traversal.push(temp.left);
            if(temp.right!=null) traversal.push(temp.right);
        }
        return order;
    }
}
```

#### 二叉树的中序遍历
>给定一个二叉树，返回其中序遍历。
>例如：
>给定二叉树 `[1,null,2,3]`,
><pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">   1
>\
>2
>/
>3
></pre>
>返回 `[1,3,2]`.

- 递归方式

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> order = new ArrayList<>();
        inorder(root,order);
        return order;
    }
    public void inorder(TreeNode root,List<Integer> res){
        if(root==null) return;
        inorder(root.left,res);
        res.add(root.val);
        inorder(root.right,res);
    }
}
```

- 非递归方式

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> order = new ArrayList<>();
        TreeNode cur = root;
        Stack<TreeNode> traversal = new Stack<>();
        while(cur!=null||!traversal.isEmpty()){
            if(cur!=null){
                traversal.push(cur);
                cur=cur.left;
            }else{
                TreeNode temp = traversal.pop();
                cur=temp.right;
                order.add(temp.val);
            }
        }
        return order;
    }
}
```

#### 二叉树的后序遍历
>给定一棵二叉树，返回其节点值的后序遍历。
>例如：
>给定二叉树 `[1,null,2,3]`，
><pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">   1
>\
>2
>/
>3
></pre>
>返回 `[3,2,1]`。

- 递归方式

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> order = new ArrayList<>();
        postorder(root,order);
        return order;
    }
    public void postorder(TreeNode root,List<Integer> res){
        if(root==null) return;
        postorder(root.left,res);
        postorder(root.right,res);
        res.add(root.val);
    }
}
```

- 非递归方式

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> order = new ArrayList<>();
        Stack<TreeNode> s1 = new Stack<>();
        Stack<Integer> s2 = new Stack<>();
        if(root==null) return order;
        s1.push(root);
        while(!s1.isEmpty()){
            TreeNode temp = s1.pop();
            s2.push(temp.val);
            if(temp.left!=null)
                s1.push(temp.left);
            if(temp.right!=null)
                s1.push(temp.right);
        }
        while(!s2.isEmpty()){
            order.add(s2.pop());
        }
        return order;
    }
}
```

#### 二叉树层次遍历
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\62378ecd-5889-4b51-8416-86d9431ef523.jpg)
>给定一个二叉树，返回其按层次遍历的节点值。 （即zhu'ceng'de，从左到右访问）。
>例如:
>给定二叉树: `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
返回其层次遍历结果为：
```
[
  [3],
  [9,20],
  [15,7]
]
```

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res  = new ArrayList<>();
        if(root==null) return res;
        List<Integer> cl = new ArrayList<Integer>();
        LinkedList<TreeNode> q  = new LinkedList<TreeNode>();
        q.add(root);
        int curNum=1, nextNum=0;
        while(!q.isEmpty()){
            TreeNode n = q.poll();
            curNum--;
            cl.add(n.val);
            if(n.left!=null){
                q.add(n.left);
                nextNum++;
            }
            if(n.right!=null){
                q.add(n.right);
                nextNum++;
            }
            if(curNum==0){
                res.add(cl);
                cl = new ArrayList<Integer>();
                curNum  =nextNum;
                nextNum = 0;
            }
        }
        return res;
    }
}
```

---

---

### 树的递归

#### “自顶向下” 的解决方案
>“自顶向下” 意味着在每个递归层级，我们将首先访问节点来计算一些值，并在递归调用函数时将这些值传递到子节点。 所以 “自顶向下” 的解决方案可以被认为是一种前序遍历。 具体来说，递归函数 top_down(root, params) 的原理是这样的：
```java
return specific value for null node
update the answer if needed                     // anwer <-- params
left_ans = top_down(root.left, left_params)     // left_params <-- root.val, params
right_ans = top_down(root.right, right_params)  // right_params <-- root.val, params
return the answer if needed                     // answer <-- left_ans, right_ans
```

#### “自底向上” 的解决方案
>“自底向上” 是另一种递归方法。 在每个递归层次上，我们首先对所有子节点递归地调用函数，然后根据返回值和根节点本身的值得到答案。 这个过程可以看作是后序遍历的一种。 通常， “自底向上” 的递归函数 bottom_up(root) 为如下所示：
```java
return specific value for null node
left_ans = bottom_up(root.left)          // call function recursively for left child
right_ans = bottom_up(root.right)        // call function recursively for right child
return answers                           // answer <-- left_ans, right_ans, root.val
```


#### Maximum Depth of Binary Tree
>Given a binary tree, find its maximum depth.
>The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
>For example:
>Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its depth = 3.

```java
class Solution {
    public int maxDepth(TreeNode root) {
        maximum_depth(root,1);
        return answer;
    }
    private int answer;
    private void maximum_depth(TreeNode root, int depth) {
        if (root == null) {
            return;
        }
        if (root.left == null && root.right == null) {
            answer = Math.max(answer, depth);
        }
        maximum_depth(root.left, depth + 1);
        maximum_depth(root.right, depth + 1);
    }
}
```

#### Symmetric Tree
>Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).
>For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
But the following `[1,2,2,null,3,null,3]` is not:
```
    1
   / \
  2   2
   \   \
   3    3
```

```java
class Solution{
    public boolean isSymmetric(TreeNode root) {  
        if(root==null)  
            return true;  
        return isSymmetric(root.left,root.right);  
    }  
    public boolean isSymmetric(TreeNode l,TreeNode r){  
        if(l==null && r==null)  
            return true;  
        if(l==null || r==null)  
            return false;  
        if(l.val==r.val)  
            return isSymmetric(l.left,r.right) && isSymmetric(l.right,r.left);  
        return false;  
    } 
}
```

#### Path Sum
>Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.
>For example:
>Given the below binary tree and `sum = 22`,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```
return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.

```java
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null) return false;
        if(root.left == null && root.right == null && sum - root.val == 0) return true;
        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
    }
}
```

### 二叉查找树

- 若左子树不空，则左子树上所有结点的值均小于或等于它的根结点的值
- 若右子树不空，则右子树上所有结点的值均大于或等于它的根结点的值
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\05b6ba46-e107-4d5f-b1d3-09985d4ce5b7.jpg)

#### Serialize and Deserialize BST
>Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.
>Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.
>**The encoded string should be as compact as possible.**
>**Note:** Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        String data="";
        BST_preorder(root,data);
        return data;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data==null||data.length()<=0) return null;
        List<TreeNode> node_list = new ArrayList<>();
        int val = 0;
        for(int i=0;i<data.length();i++){
            if(data.charAt(i)=='#'){
                node_list.add(new TreeNode(val));
                val=0;
            }
            else{
                val=val*10+data.charAt(i)-'0';
            }
        }
        
        for(int i=1;i<node_list.size();i++){
            BST_insert(node_list.get(0),node_list.get(i));
        }
        return node_list.get(0);
    }
    
    private void BST_insert(TreeNode root,TreeNode insert){
        if(root==null) return;
        if(insert.val<root.val){
            if(root.left!=null){
                BST_insert(root.left,insert);
            }
            else{
                root.left=insert;
            }
        }
        else{
            if(root.right!=null){
                BST_insert(root.right,insert);
            }
            else{
                root.right=insert;
            }
        }
    }
    
    private void BST_preorder(TreeNode root,String data){
        if(root == null){
            return;
        }
        String str_val="";
        change_int_to_string(root.val,str_val);
        data+=str_val;
        BST_preorder(root.left,data);
        BST_preorder(root.right,data);
    }
    
    private void change_int_to_string(int val,String s){
        String tmp ="";
        if(val==0) tmp="0";
        while(val>0){
            tmp=val%10+tmp;
            val = val/10;
        }
        s+=tmp;
        s+="#";
    }
}
```

#### Count of Smaller Numbers After Self

>You are given an integer array _nums_ and you have to return a new _counts_ array. The _counts_ array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.
>**Example:**
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">Given _nums_ = [5, 2, 6, 1]
To the right of 5 there are **2** smaller elements (2 and 1).
To the right of 2 there is only **1** smaller element (1).
To the right of 6 there is **1** smaller element (1).
To the right of 1 there is **0** smaller element.
</pre>
>Return the array `[2, 1, 1, 0]`.


```java
class Solution {
    class Node {
        Node left, right;
        int val, sum, dup = 1;
        public Node(int v, int s) {
            val = v;
            sum = s;
        }
    }
    public List<Integer> countSmaller(int[] nums) {
        Integer[] ans = new Integer[nums.length];
        Node root = null;
        for (int i = nums.length - 1; i >= 0; i--) {
            root = insert(nums[i], root, ans, i, 0);
        }
        return Arrays.asList(ans);
    }
    private Node insert(int num, Node node, Integer[] ans, int i, int preSum) {
        if (node == null) {
            node = new Node(num, 0);
            ans[i] = preSum;
        } else if (node.val == num) {
            node.dup++;
            ans[i] = preSum + node.sum;
        } else if (node.val > num) {
            node.sum++;
            node.left = insert(num, node.left, ans, i, preSum);
        } else {
            node.right = insert(num, node.right, ans, i, preSum + node.dup + node.sum);
        }
        return node;
    }
}
```

### 树的题目
#### Path Sum II
>Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.
>For example:
>Given the below binary tree and `sum = 22`,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
return
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\5d0ef331-1b97-4130-b880-d5d6231058bd.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\d9233553-dda9-42ce-9d58-7573773809c0.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\9ed33726-ba1e-4152-9b43-b0305918481d.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\6cd68f1e-8343-4c28-89a9-132937c2e4d7.jpg)

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
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        int path_value = 0;
        preorder(root,res,path,path_value,sum);
        return res;
    }
    private void preorder(TreeNode root,List<List<Integer>> res,List<Integer> path,int path_value,int sum){
        if(root==null){
            return;
        }
        path_value += root.val;
        path.add(root.val);
        if(path_value==sum&&root.left==null&&root.right==null){
            res.add(new ArrayList<Integer>(path));
        }
        preorder(root.left,res,path,path_value,sum);
        preorder(root.right,res,path,path_value,sum);
        path_value -= root.val;
        path.remove(path.size()-1);
    }
    
}
```

#### Lowest Common Ancestor of a Binary Tree
>Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.
>According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow **a node to be a descendant of itself**).”
```
        ___3___
       /       \
     _5__    ___1__
    /    \  /      \
   6     2  0       8
        / \
       7   4
```
>For example, the lowest common ancestor (LCA) of nodes `5` and `1` is `3`. Another example is LCA of nodes `5` and `4` is `5`, since a node can be a descendant of itself according to the LCA definition.

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\3961aac2-a7ff-4ed1-866f-0392ee1165c5.jpg)
- 方法一
```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        LinkedList<TreeNode> path = new LinkedList<>();
        LinkedList<TreeNode> res_p = new LinkedList<>();
        LinkedList<TreeNode> res_q = new LinkedList<>();
        preorder(root,p,path,res_p,0);
        path.clear();
        preorder(root,q,path,res_q,0);
        int path_len=0;
        if(res_p.size()<res_q.size()){
            path_len=res_p.size();
        }
        else{
            path_len=res_q.size();
        }
        TreeNode result = null;
        for(int i=0;i<path_len;i++){
            if(res_q.get(i)==res_p.get(i)){
                result = res_p.get(i);
            }
        }
        return result;
        
     }
    
    private void preorder(TreeNode root,TreeNode search,LinkedList<TreeNode> path,LinkedList<TreeNode> res,int finish){
        if(root==null||finish==1){
            return;
        }
        path.push(root);
        if(root==search){
            finish = 1;
            res = new LinkedList<TreeNode>(path);
        }
        preorder(root.left,search,path,res,finish);
        preorder(root.right,search,path,res,finish);
        path.pop();
    }
```
- 方法二
```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        return left == null ? right : right == null ? left : root;
    }
```

#### Flatten Binary Tree to Linked List
>Given a binary tree, flatten it to a linked list in-place.
>For example,
>Given
```
         1
        / \
       2   5
      / \   \
     3   4   6
```
The flattened tree should look like:
```
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\11052a52-2250-4030-99cd-47cac0720523.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\fa222f0f-82c0-4983-b562-7b09751d1df1.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\be057edf-6afe-4e36-82f5-dc5b3b3e032b.jpg)

- 方法一
```java
class Solution {
    public void flatten(TreeNode root) {
        TreeNode last = null;
        preorder(root,last);
    }
    
    private TreeNode preorder(TreeNode root,TreeNode last){
        if(root==null){
            return null;
        }
        if(root.left==null&&root.right==null){
            return root;
        }
        TreeNode left = root.left;
        TreeNode right = root.right;
        TreeNode left_last = null;
        TreeNode right_last = null;
        if(left!=null){
            left_last = preorder(left,left_last);
            root.left = null;
            root.right=left;
            last = left_last;
        }
        if(right!=null){
            right_last = preorder(right,right_last);
            if(left_last!=null){
                left_last.right = right;
            }
            last = right_last;
        }
        return last;
    }
}
```

- 方法二
```java
public class Solution {
    public void flatten(TreeNode root) {
        if(root==null)
            return;
        flatten(root.left);
        flatten(root.right);
        TreeNode left  = root.left;
        TreeNode right = root.right;
        root.left  = null;
        root.right = left; 
        while(root.right!=null)
            root = root.right;
        root.right = right;
    }
}
```


## 图
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\5853b9b0-2df9-4965-9581-6cec8e9d7486.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\二叉树和图\05a2f1fc-8a17-4695-9aba-ba3915de4767.jpg)