## 12.1 分治算法
分治（divide and conquer），全称分而治之，是一种非常重要且常见的算法策略。分治通常基于递归实现，包括“分”和“治”两个步骤：
- 分（划分阶段）：递归地将原问题分解为两个或多个子问题，直至到达最小子问题时终止。
- 治（合并阶段）：从已知解的最小子问题开始，从底至顶地将子问题的解进行合并，从而构建出原问题的解。

### 12.1.1 如何判断分治问题
一个问题是否适合使用分治解决，通常可以参考以下几个判断依据：
- 1 **问题可以分解**：原问题可以分解成规模更小、类似的子问题，以及能够以相同方式递归地进行划分。
- 2 **子问题是独立的**：子问题之间没有重叠，互不依赖，可以独立解决。
- 3 **子问题的解可以合并**：原问题的解通过合并子问题的解得来。

显然，归并排序满足上面条件

### 12.1.2 通过分治提升效率
分治不仅可以有效地解决算法问题，往往还可以提升算法效率。在排序算法中，快速排序、归并排序、堆排序相较于选择、冒泡、插入排序更快，就是因为它们应用了分治策略。

#### 1.操作数量优化
- 这意味着当`n > 4`时，划分后的操作数量更少，排序效率应该更高。

#### 2.并行运算优化
- 并行优化在多核或多处理器的环境中尤其有效，因为系统可以同时处理多个子问题，更加充分地利用计算资源，从而显著减少总体的运行时间。（如桶排序就适合多线程并行）

### 12.1.3 分治常见应用
- 分治是一种“润物细无声”的算法思想，隐含在各种算法与数据结构之中


## 12.2 分治搜索策略
- 时间复杂度为` O(logn)` 的搜索算法通常是基于分治策略实现的，例如二分查找和树。

### 1. 基于分治实现二分查找
```
给定一个长度为 n 的有序数组 nums ，其中所有元素都是唯一的，请查找元素 target 。
```
```C++
/* 二分查找：问题 f(i, j) */
int dfs(vector<int> &nums, int target, int i, int j) {
    // 若区间为空，代表无目标元素，则返回 -1
    if (i > j) {
        return -1;
    }
    // 计算中点索引 m
    int m = (i + j) / 2;
    if (nums[m] < target) {
        // 递归子问题 f(m+1, j)
        return dfs(nums, target, m + 1, j);
    } else if (nums[m] > target) {
        // 递归子问题 f(i, m-1)
        return dfs(nums, target, i, m - 1);
    } else {
        // 找到目标元素，返回其索引
        return m;
    }
}

/* 二分查找 */
int binarySearch(vector<int> &nums, int target) {
    int n = nums.size();
    // 求解问题 f(0, n-1)
    return dfs(nums, target, 0, n - 1);
}
``` 

## 12.3 构建树问题
```
给定一棵二叉树的前序遍历 preorder 和中序遍历 inorder ，请从中构建二叉树，返回二叉树的根节点。假设二叉树中没有值重复的节点
```
```C++
/* 构建二叉树：分治 */
TreeNode *dfs(vector<int> &preorder, unordered_map<int, int> &inorderMap, int i, int l, int r) {
    // 子树区间为空时终止
    if (r - l < 0)
        return NULL;
    // 初始化根节点
    TreeNode *root = new TreeNode(preorder[i]);
    // 查询 m ，从而划分左右子树
    int m = inorderMap[preorder[i]];
    // 子问题：构建左子树
    root->left = dfs(preorder, inorderMap, i + 1, l, m - 1);
    // 子问题：构建右子树
    root->right = dfs(preorder, inorderMap, i + 1 + m - l, m + 1, r);
    // 返回根节点
    return root;
}

/* 构建二叉树 */
TreeNode *buildTree(vector<int> &preorder, vector<int> &inorder) {
    // 初始化哈希表，存储 inorder 元素到索引的映射
    unordered_map<int, int> inorderMap;
    for (int i = 0; i < inorder.size(); i++) {
        inorderMap[inorder[i]] = i;
    }
    TreeNode *root = dfs(preorder, inorderMap, 0, 0, inorder.size() - 1);
    return root;
}
```


## 12.4 汉诺塔问题


## 12.5 小结
