# [树](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.md)
## 种类
- 满二叉树
- 完全二叉树
- 二叉搜索树
  > 1. 如果该节点有左节点, 则左节点的值小于该节点
  > 2. 如果该节点有右节点, 则右节点的值都大于该节点
  > 3. 他的左右子树也都为二叉搜索树
- 平衡二叉搜索树
## 存储方式
- 链式存储（指针）
- 循序存储（数组）
## 遍历方式
- 深度优先（递归，指的是根节点的顺序）
  - 前序遍历
  - 中序遍历
  - 后续遍历
- 广度优先

## 二叉树结构
```java
public class TreeNode {
      int val;
      TreeNode left;
      TreeNode right;
      TreeNode() {}
      TreeNode(int val) { this.val = val; }
      TreeNode(int val, TreeNode left, TreeNode right) {
          this.val = val;
          this.left = left;
          this.right = right;
      }
}
```
## 常见算法
### 144.二叉树的前序遍历
```java
class Solution {
  public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    traverse(root, res);
    return res;
  }

  //对于递归，通常都需要拆出一个递归方法
  //确定参数和返回值
  private void traverse(TreeNode root, List<Integer> res) {
      //终止条件, 避免死归
    if (root == null) {
      return;
    }
    //确定单层递归的逻辑
    res.add(root.val);
    traverse(root.left, res);
    traverse(root.right, res);
  }

  //迭代的实现方案 模拟调用栈栈
  public List<Integer> preorderTraversal(TreeNode root) {
    ArrayList<Integer> res = new ArrayList<>();
    if (root == null) {
      return res;
    }
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    while (!stack.isEmpty()) {
      TreeNode node = stack.pop();
      res.add(node.val);
      if (node.right != null) stack.push(node.right);
      if (node.left != null) stack.push(node.left);
    }

    return res;
  }
}
```
### 94.二叉树的中序遍历
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        traverse(root, res);
        return res;
    }

    private void traverse(TreeNode node, List<Integer> res) {
        if (node == null) {
            return;
        }
        traverse(node.left, res);
        res.add(node.val);
        traverse(node.right, res);
    }

  public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    while (root != null || !stack.isEmpty()) {
      if (root != null) {//不断往左子树方向走,模拟递归,直到左子树为null
        stack.push(root);
        root = root.left;
      } else {
        //当前节点为空，说明左边走到头了，从栈中弹出节点并保存 
        // 然后转向右边节点，继续上面整个过程
        root = stack.pop();
        res.add(root.val);
        root = root.right;
      }
    }
    return res;
  }
}
```
145.二叉树的后序遍历
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        traverse(root, res);
        return res;
    }

    private void traverse(TreeNode node, List<Integer> res) {
        if (node == null) {
            return;
        }
        traverse(node.left, res);
        traverse(node.right, res);
        res.add(node.val);
    }
}
```
102.二叉树的层序遍历
> 广度优先的实现方式
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<TreeNode> queue = new LinkedList<>();
        //先加入根节点
        if (root != null) {
            queue.offer(root);
        }
        while (queue.size() >0) {
            //这个是灵魂, 需要得到当前层次的最大遍历次数
            int size = queue.size();
            ArrayList<Integer> tmp = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                tmp.add(node.val);
                //分别将子节点加入
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            res.add(tmp);
        }

        return res;
    }
}
```
### 107.二叉树的层次遍历 II
> 和102类似,只是数据从头部插入
```java
//queue 的offer,poll api
class Solution {
  public List<List<Integer>> levelOrderBottom(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    if (root != null) {
      queue.offer(root);
    }
    while(!queue.isEmpty()) {
      int size = queue.size();
      ArrayList<Integer> tmp = new ArrayList<>();
      for (int i = 0; i < size; i++) {
        TreeNode node = queue.poll();
        tmp.add(node.val);
        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
      }
      res.add(0, tmp);
    }

    return res;
  }
}
```
### 199.二叉树的右视图
```java
class Solution {
  public List<Integer> rightSideView(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    if (root != null) {
      queue.add(root);
    }
    while (queue.size() > 0) {
      int size = queue.size();
      for (int i = 1; i <= size; i++) {
        TreeNode node = queue.poll();
        //有视图即将最右边元素的结果加入
        if (i == size) {
          res.add(node.val);
        }
        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
      }
    }

    return res;
  }
}
```
### 637.二叉树的层平均值
```java
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> res = new ArrayList<>();
        LinkedList<TreeNode> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        while (queue.size() >0) {
            int size = queue.size();
            //double
            double total = 0;
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                total += node.val;
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            res.add(total/size);
        }

        return res;
    }
}
```
### N叉树的层序遍历
```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Node> queue = new LinkedList<Node>();
        if (root != null) {
            queue.offer(root);
        }
        while (queue.size() >0) {
            int size = queue.size();
            ArrayList<Integer> tmp = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                Node node = queue.poll();
                tmp.add(node.val);
                for(Node child : node.children) {
                    queue.offer(child);
                }
            }
            res.add(tmp);
        }
        return res;
    }
}

```
### 515.在每个树行中找最大值
```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
        if (root != null) {
            queue.offer(root);
        }
        while(queue.size() >0) {
            int size = queue.size();
            int max = Integer.MIN_VALUE;
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                max = Math.max(node.val, max);
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            res.add(max);
        }

        return res;
    }
}
```
### 116.填充每个节点的下一个右侧节点指针
```java
class Solution {
    public Node connect(Node root) {
        LinkedList<Node> queue = new LinkedList<Node>();
        if (root != null) {
            queue.offer(root);
        }
        while (queue.size() >0) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Node node = queue.poll();
                if (i < size-1) {
                    node.next = queue.peek();
                } else {//最后一个几点给null
                    node.next = null;
                }
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
        }

        return root;
    }
}
```
### 226. 翻转二叉树
```java
// 递归 -广度优先
class Solution {
    public TreeNode invertTree(TreeNode root) {
        recursiveInvert(root);
        return root;
    }

    private void recursiveInvert(TreeNode node) {
        if (node == null) return;
        //前序遍历翻转, 中序遍历会翻两次
        TreeNode leftNode = node.left;
        TreeNode rightNode = node.right;
        node.left = rightNode;
        node.right = leftNode;
        recursiveInvert(leftNode);
        recursiveInvert(rightNode);
    }
}
//迭代 -深度优先
class Solution {
  public TreeNode invertTree(TreeNode root) {
    LinkedList<TreeNode> queue = new LinkedList<>();
    if (root != null) {
      queue.offer(root);
    }
    while (queue.size() > 0) {
      int size = queue.size();
      for (int i = 0; i < size; i++) {
        TreeNode node = queue.poll();
        TreeNode tmp = node.left;
        node.left = node.right;
        node.right = tmp;
        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
      }
    }

    return root;
  }
}
```
### 101. 对称二叉树
```java
//递归
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return false;

        return isSame(root.left, root.right);
    }

    private boolean isSame(TreeNode left, TreeNode right) {
        if (left == null || right == null) {
            return left == null && right == null;
        }

        return left.val == right.val
                && isSame(left.right, right.left)
                && isSame(left.left, right.right);
    }

    //迭代, 每次pop两个元素出来比较
  public boolean isSymmetric(TreeNode root) {
    if (root == null) return false;
    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    queue.offer(root.left);
    queue.offer(root.right);
    while(queue.size() > 0) {
      //二叉树
      TreeNode left = queue.poll();
      TreeNode right = queue.poll();
      if (left == null && right == null) {
        continue;
      }
      if ((left != null && right ==  null )
              || (left == null && right != null)
              || left.val != right.val) {
        return false;
      }
      queue.offer(left.left);
      queue.offer(right.right);
      queue.offer(left.right);
      queue.offer(right.left);
    }

    return true;
  }
}

```
### 104.二叉树的最大深度

```java
import java.util.LinkedList;

//递归
class Solution {
  public int maxDepth(TreeNode root) {
    return recursive(root, 0);
  }

  private int recursive(TreeNode root, int depth) {
    if (root == null) {
      return depth;
    }

    return Math.max(
            recursive(root.left, depth+1),
            recursive(root.right, depth+1)
    );
  }
  //迭代 深度遍历
  public int maxDepth(TreeNode root) {
    //return recursive(root, 0);
    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    if (root != null) {
      queue.offer(root);
    }
    int depth = 0;
    while (queue.size() > 0) {
      depth++;
      int size = queue.size();
      for (int i = 0; i < size; i++) {
        TreeNode node = queue.poll();
        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
      }
    }
    return depth;
  }
}


```
### 104.多叉树的最大深度
```java
//递归
class Solution {
  public int maxDepth(Node root) {
    return recursive(root, 0);
  }

  private int recursive(Node root, int depth) {
    if (root == null) return depth;
    int res = depth+1;
    for (Node child : root.children) {
      res = Math.max(recursive(child, depth+1), res);
    }

    return res;
  }

  //深度优先遍历
  public int maxDepth(Node root) {
    if (root == null) return 0;
    LinkedList<Node> queue = new LinkedList<Node>();
    queue.offer(root);
    int depth = 0;
    while (queue.size() > 0) {
      int size = queue.size();
      depth++;
      for (int i = 0; i < size; i++) {
        Node node = queue.poll();
        for(Node child : node.children) {
          queue.offer(child);
        }
      }
    }

    return depth;
  }
}
```
### 111.二叉树的最小深度
> 不同于最大深度, 非叶子节点的深度不算
```java
class Solution {
    public int minDepth(TreeNode root) {
        return recursive(root, 0);
    }

    private int recursive(TreeNode root, int depth) {
        if (root == null) return depth;
        if (root.left == null && root.right == null){
            return depth+1;
        } else if (root.left == null) {//只有右节点
            return recursive(root.right, depth+1);
        } else if (root.right == null) {//只有左节点
            return recursive(root.left, depth+1);
        } else {//有左节点和右节点
            return Math.min(
                    recursive(root.left, depth+1),
                    recursive(root.right, depth+1)
            );
        }
    }
//迭代解法, 二叉树的层次遍历离不开队列
  public int minDepth(TreeNode root) {
    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    if (root == null) return 0;
    queue.offer(root);
    int depth = 0;
    while (queue.size() > 0) {
      int size = queue.size();
      depth++;
      for (int i = 0; i < size; i++) {
        TreeNode node = queue.poll();
        if (node.left == null && node.right == null) {
          return depth;
        }
        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
      }
    }

    return depth;
  }
}
```
### 222.完全二叉树的节点个数
```java
class Solution {
    //递归
  public int countNodes(TreeNode root) {
    if (root == null) return 0;
    int leftNum = countNodes(root.left);
    int rightNum = countNodes(root.right);
    return leftNum + rightNum +1;
  }

  //迭代
  public int countNodes(TreeNode root) {
    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    if (root == null) {
      return 0;
    }
    queue.offer(root);
    int count = 0;
    while (queue.size() > 0) {
      int size = queue.size();
      for (int i = 0; i < size; i++) {
        TreeNode node = queue.poll();
        count++;
        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
      }
    }
    return count;
  }
}
```
### 110.平衡二叉树
```java
class Solution {
  public boolean isBalanced(TreeNode root) {
    if (root == null) return true;
    //左边不平衡或者右边不平衡
    if (!isBalanced(root.left) || !isBalanced(root.right)) {
      return false;
    }

    //如果左边平衡,且右边平衡,再看左右的差距高度是是否是1
    int leftH = getDepth(root.left);
    int rightH = getDepth(root.right);

    return Math.abs(leftH - rightH) <= 1;
    
//    return isBalanced(root.left) 
//            && isBalanced(root.right) 
//            && Math.abs(getDepth(root.left) - getDepth(root.right)) <= 1;
  }

  private int getDepth(TreeNode root) {
    if (root == null) return 0;

    int depth = Math.max(getDepth(root.left), getDepth(root.right));
    return depth + 1;
  }
}
```

### [257. 二叉树的所有路径](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.md#https://mp.weixin.qq.com/s/ivLkHzWdhjQQD1rQWe6zWA)
```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        ArrayList<String> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        trarverse(root, "", res);

        return res;
    }

    private void trarverse(TreeNode node, String path, List<String> res) {
        path += node.val;
        //回溯完成
        if (node.left == null && node.right == null) {
            res.add(path);
            return;
        }

        if (node.left != null) {
            trarverse(node.left, path + "->", res);//path + "=>" 带有回溯
        }
        if (node.right != null) {
            trarverse(node.right, path + "->", res);
        }
    }
}
```
### 404.左叶子之和
```java
class Solution {

    //确定参数与返回值
    public int sumOfLeftLeaves(TreeNode root) {
        //确定终止条件
        if(root == null) return 0;
        /*
        *       （root）
        *       /      \
        *      (L)     (R)
        *              / \
        *         (R.left)    
        */

        //存储根节点的左叶子的值。
        int rootVal = 0;
        if(root.left != null && root.left.left == null && root.left.right == null){
            rootVal = root.left.val;
           
        }
        //存储根节点的L子树的左叶子节点（L.left）的值
        int left = sumOfLeftLeaves(root.left);

        //存储根节点的R子树的左叶子节点（R.left）的值
        int right = sumOfLeftLeaves(root.right);

        int sum = rootVal + left + right;
        return sum;
    }
}
```

### 513.找树左下角的值
```java
class Solution {
    int maxDepth = Integer.MIN_VALUE;
    int maxValue = Integer.MIN_VALUE;
    public int findBottomLeftValue(TreeNode root) {
        getMax(root, 0);
        return maxValue;
    }

    private void getMax(TreeNode root, int depth) {
        //终止条件
        if (root.left == null && root.right == null) {
            if (depth > maxDepth) {
                maxDepth = depth;
                maxValue = root.val;
            }
        }
        //递归左右子树的高度
        if (root.left != null) {
            getMax(root.left, depth+1);
        }
        if (root.right != null) {
            getMax(root.right, depth+1);
        }
    }
}
```
### 112.路径总和
> 对于需要递归所有节点的, 递归函数不能有返回值, 否则需要有返回值
```java
class Solution {
  public boolean hasPathSum(TreeNode root, int targetSum) {
    return traverse(root, targetSum);
  }

  private boolean traverse(TreeNode node, int targetSum) {
      //说明该父节点只有一个子节点, 且不满足targetSum
    if (node == null) {
      return false;
    }
    if (node.left == null && node.right == null) {
      return node.val == targetSum;
    }
    boolean leftHasPath = traverse(node.left, targetSum-node.val);
    boolean rightHasPath = traverse(node.right, targetSum-node.val);

    return  leftHasPath || rightHasPath;
  }
}
```
### 113.路径总和II
```java
class Solution {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
       if (root == null) {
           return res;
       }
        traverse(root, targetSum, new ArrayList<Integer>());
        return res;
    }

    private void traverse(TreeNode node, int targetSum, List<Integer> path) {
        path.add(node.val);
        if (node.left == null && node.right == null && node.val == targetSum) {
            //拷贝结果
            List<Integer> thisPath = new ArrayList<Integer>(path);
            res.add(thisPath);
        }
        if (node.left == null && node.right == null) {
            return;
        }
        if (node.left != null) {
            traverse(node.left, targetSum-node.val, path);
            //回溯
            path.remove(path.size()-1);
        }

        if (node.right != null) {
            traverse(node.right, targetSum-node.val, path);
            //回溯
            path.remove(path.size()-1);
        }
    }
}
```
### 617.合并二叉树
```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        //root1 为空, 则返回root2的节点
        if (root1 == null) {
            return root2;
        }
        if (root2 == null) {
            return root1;
        }
        //此时root1和root2都不为空
        root1.val += root2.val;
        root1.left = mergeTrees(root1.left, root2.left);
        root1.right = mergeTrees(root1.right, root2.right);
        return root1;
    }
}
```
### 700.二叉搜索树中的搜索
```java
class Solution {
    //递归实现
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) {
            return null;
        }
        if (root.val == val) {
            return root;
        }
        if (root.val > val) {
            return searchBST(root.left, val);
        } else {
            return searchBST(root.right, val);
        }
    }
}

class Solution {
    //迭代实现
  public TreeNode searchBST(TreeNode root, int val) {
    while(root != null) {
      if (root.val == val) {
        return root;
      } else if (root.val > val) {
        root = root.left;
      } else{
        root = root.right;
      }
    }
    return null;
  }
}
```
### 98.验证二叉搜索树
```java
//递归
class Solution {
    public boolean isValidBST(TreeNode root) {
        return helper(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean helper(TreeNode root, long left, long right) {
        if (root == null) return true;
        //不能等于
        if (root.val <= left || root.val >= right ) return false;

        //因为节点的值要大于他的所有左子树的值, 且小于他所有右子树的值
        //构建一个区间范围
        return helper(root.left, left, root.val) && helper(root.right, root.val, right);
    }
}

//中序遍历
class Solution {
  public boolean isValidBST(TreeNode root) {
    ArrayList<Integer> res = new ArrayList<Integer>();
    convertToList(root, res);
    for (int i = 0; i < res.size() -1 ; i++) {
      if (res.get(i) >= res.get(i+1)) return false;
    }
    return true;
  }

  //中序遍历后, 该链表是有序的
  private void convertToList(TreeNode root, List<Integer> list) {
    if (root.left != null) convertToList(root.left, list);
    list.add(root.val);
    if (root.right != null) convertToList(root.right, list);
  }
}
```
### 530.二叉搜索树的最小绝对差
> 对于二叉搜索树要想到中序遍历
```java
class Solution {
    private int res = Integer.MAX_VALUE;
    private TreeNode pre;
    public int getMinimumDifference(TreeNode root) {
        if (root == null) return 0;
        helper(root);
        return res;
    }

    //二叉搜索树的中序遍历是有序的, 找出两两相邻的最小值即可
    private void helper(TreeNode root) {
        if (root == null) return;
        helper(root.left);
        if (pre != null) {
            int diff = root.val - pre.val;
            if (diff < res) res = diff;
        }
        pre=root;
        helper(root.right);
    }
}
```
### 501.二叉搜索树中的众数
```java
class Solution {
    TreeNode pre;
    int count;
    int maxCount;
    List<Integer> res = new LinkedList<>();
    public int[] findMode(TreeNode root) {
        dfs(root);
        int[] arr = new int[res.size()];
        for (int i = 0; i < res.size(); i++) {
            arr[i] = res.get(i);
        }

        return arr;
    }

    private void dfs(TreeNode root) {
        //中序遍历解题
        if (root == null) return;
        dfs(root.left);
        if (pre != null) {
            if (pre.val == root.val) {
                count++;
            }  else {
                count=1;
            }
        } else {
            count=1;
        }
        //相等也需要放入
        if (count == maxCount) {
            res.add(root.val);
        }
        //大于需要将之前的结果都清除
        if (count > maxCount) {
            res.clear();
            res.add(root.val);
            maxCount = count;
        }
        pre = root;
        dfs(root.right);
    }
}
```
### [236. 二叉树的最近公共祖先](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.md)
```java
class Solution {
    //从下往上处理, 采用后续遍历
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //递归终止的条件是该节点为p,q节点,或者为null节点
        if (root == p || root == q || root == null) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        //有左右子树存在pq,则该节点是最近祖先
        if (left != null && right != null) {
            return root;
        } else if (left != null) {
            return left;
        } else {
            return right;
        }
    }
}
```
### 235. 二叉搜索树的最近公共祖先
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return null;
        TreeNode smallNodel, bigNode;
        if (p.val < q.val) {
            smallNodel = p;
            bigNode=q;
        } else {
            smallNodel = q;
            bigNode=p;
        }
        if (root.val >= smallNodel.val && root.val <= bigNode.val) {
            return root;
        } else if (root.val < smallNodel.val) {
            return lowestCommonAncestor(root.right,p,q);
        } else {
            return lowestCommonAncestor(root.left, p,q);
        }
    }
}

```
### 701.二叉搜索树中的插入操作
```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }
        helper(root, val);
        return root;
    }

    private void helper(TreeNode root, int val) {
        if (root.val > val) {
            if (root.left == null) {
                root.left = new TreeNode(val);
            } else {
                insertIntoBST(root.left, val);
            }
        } else {
            if (root.right == null) {
                root.right = new TreeNode(val);
            } else {
                insertIntoBST(root.right, val);
            }
        }
    }
}
```
### 450.删除二叉搜索树中的节点
```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return root;
        if (root.val == key) {
            if (root.right == null) {
                return root.left;
            } else if (root.left == null) {
                return root.right;
            } else {
                //将该节点右子树的最左节点放左子树, 并返回右子树
                TreeNode cur = root.right;
                while (cur.left != null) {
                    cur = cur.left;
                }
                cur.left = root.left;
                return root.right;
            }
        }
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
        } else  {
            root.right = deleteNode(root.right, key);
        }

        return root;
    }
}
```
### [669. 修剪二叉搜索树](https://gitee.com/ntu_sam_chensenbo/leetcode-master/blob/master/problems/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.md)
```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) return null;

        if (root.val < low) {
            return trimBST(root.right, low, high);
        }
        if (root.val > high) {
            return trimBST(root.left, low, high);
        }

        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);

        return root;
    }
}
```
### 108.将有序数组转换为二叉搜索树
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        if (nums.length < 1) {
            return null;
        }
        if (nums.length == 1) {
            return new TreeNode(nums[0]);
        }
        int mid = nums.length >> 1;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = sortedArrayToBST(Arrays.copyOfRange(nums,0, mid));
        root.right = sortedArrayToBST(Arrays.copyOfRange(nums, mid+1, nums.length));

        return root;
    }
}
```
### 538.把二叉搜索树转换为累加树
```java
class Solution {
    private int preVal = 0;
    public TreeNode convertBST(TreeNode root) {
        //使每个节点 node 的新值等于原树中
        // <strong>大于或等于node.val 的值之和</strong>

        //使用中序遍历, 因为搜索树是有序的
        //先遍历右节点, 再遍历根节点, 再遍历左节点
        if (root == null) return null;
        convertBST(root.right);
        root.val = root.val + preVal;
        preVal = root.val;
        convertBST(root.left);

        return root;
    }
}
```

