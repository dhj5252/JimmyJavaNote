

### 二叉树的重要性

ex: 对于快速排序和归并排序两个算法，请问你有什么理解：*快速排序就是二叉树的前序遍历，归并排序就是二叉树的后序遍历*

**写递归算法的关键是要明确函数的“定义”是什么，然后相信这个定义，利用这个定义推导最终的结果，绝不要跳入到递归的细节**

**写树相关的算法，简单的说，必须要搞清楚当前的root节点应该做什么，然后根据函数定义递归的调用子节点**

```java
// 定义：count(root) 返回以 root 为根的树有多少节点
int count(TreeNode root) {
    // base case
    if (root == null) return 0;
    // 自己加上子树的节点数就是整棵树的节点数
    return 1 + count(root.left) + count(root.right);
}
```

# 226 翻转二叉树

## question:

将整颗树镜像反转

## jim result:

```java
// 将整棵树的节点翻转
TreeNode invertTree(TreeNode root) {
    // base case
    if (root == null) {
        return null;
    }

    /**** 前序遍历位置 ****/
    // root 节点需要交换它的左右子节点
    TreeNode tmp = root.left;
    root.left = root.right;
    root.right = tmp;

    // 让左右子节点继续翻转它们的子节点
    invertTree(root.left);
    invertTree(root.right);

    return root;
}
```

# 116 填充二叉树节点右侧指针

## question:

填充每个节点下一个右侧节点的指针

## jim result:

```java
// 主函数
Node connect(Node root) {
    if (root == null) return null;
    connectTwoNode(root.left, root.right);
    return root;
}

// 辅助函数
void connectTwoNode(Node node1, Node node2) {
    if (node1 == null || node2 == null) {
        return;
    }
    /**** 前序遍历位置 ****/
    // 将传入的两个节点连接
    node1.next = node2;

    // 连接相同父节点的两个子节点
    connectTwoNode(node1.left, node1.right);
    connectTwoNode(node2.left, node2.right);
    // 连接跨越父节点的两个子节点
    connectTwoNode(node1.right, node2.left);
}
```

# 114 二叉树展开为链表：

## question:

给定二叉树，原地将它展开为一个单链表

## jim result:

```java
// 定义：将以 root 为根的树拉平为链表
void flatten(TreeNode root) {
    // base case
    if (root == null) return;

    flatten(root.left);
    flatten(root.right);

    /**** 后序遍历位置 ****/
    // 1、左右子树已经被拉平成一条链表
    TreeNode left = root.left;
    TreeNode right = root.right;

    // 2、将左子树作为右子树
    root.left = null;
    root.right = left;

    // 3、将原先的右子树接到当前右子树的末端
    TreeNode p = root;
    while (p.right != null) {
        p = p.right;
    }
    p.right = right;
}
```

# 654 最大二叉树

## question:

给定一个不含重复元素的整数数组。一个以此数组构建的最大二叉树定义如下：

1 二叉树的根是数组重最大的元素

2 左子树是通过数组重最大值的左边部分构造出的最大二叉树

3 右子树是通过数组重最大值右边部分构造除的最大二叉树

## jim result:

```java
/* 主函数 */
TreeNode constructMaximumBinaryTree(int[] nums) {
    return build(nums, 0, nums.length - 1);
}

/* 将 nums[lo..hi] 构造成符合条件的树，返回根节点 */
TreeNode build(int[] nums, int lo, int hi) {
    // base case
    if (lo > hi) {
        return null;
    }

    // 找到数组中的最大值和对应的索引
    int index = -1, maxVal = Integer.MIN_VALUE;
    for (int i = lo; i <= hi; i++) {
        if (maxVal < nums[i]) {
            index = i;
            maxVal = nums[i];
        }
    }

    TreeNode root = new TreeNode(maxVal);
    // 递归调用构造左右子树
    root.left = build(nums, lo, index - 1);
    root.right = build(nums, index + 1, hi);

    return root;
}
```

# 652 寻找重复的子树

## question:

给定一颗二叉树，范围所有重复的子树。对于同一类的重复子树，你只需要返回其中任意一颗的根结点即可。

## jim result:

```java
class Solution {
    Map<String, Integer> count;
    List<TreeNode> ans;
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        count = new HashMap();
        ans = new ArrayList();
        collect(root);
        return ans;
    }

    public String collect(TreeNode node) {
        if (node == null) return "#";
        String serial = node.val + "," + collect(node.left) + "," + collect(node.right);
        count.put(serial, count.getOrDefault(serial, 0) + 1);
        if (count.get(serial) == 2)
            ans.add(node);
        return serial;
    }
}
```

# 538 把二叉搜索树转为累加树

## question：

给出二叉搜素树的根节点，该树的节点值各不相同，请你将其转化为累加树，使得每个节点node的新值大于等于原树种大于或等于node.val的值之和

## jim result:

```java
class Solution {
    int sum = 0;

    public TreeNode convertBST(TreeNode root) {
        if (root != null) {
            convertBST(root.right);
            sum += root.val;
            root.val = sum;
            convertBST(root.left);
        }
        return root;
    }
}
```

# 230 二叉搜索树中第K小的元素

## question：

给定一个二叉搜索树的根节点root，和一个整数k，请你设计一个算法查找第k个最小元素

**BST的中序遍历是升序序列**

## jim result:

```java
class Solution {
  public ArrayList<Integer> inorder(TreeNode root, ArrayList<Integer> arr) {
    if (root == null) return arr;
    inorder(root.left, arr);
    arr.add(root.val);
    inorder(root.right, arr);
    return arr;
  }

  public int kthSmallest(TreeNode root, int k) {
    ArrayList<Integer> nums = inorder(root, new ArrayList<Integer>());
    return nums.get(k - 1);
  }
}
```

# 230 二叉搜索树中第K小的元素

## question：

给定一个二叉搜索树的根节点root，和一个整数k，请你设计一个算法查找第k个最小元素

## jim result:

```java
a
```

