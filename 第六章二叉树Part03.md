## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/23  
**主题：** 第六章 二叉树 Part 03｜平衡判断 · 路径回溯 · 条件求和 · 完全二叉树  
**题目：**  
- [110. Balanced Binary Tree（后序 · 高度返回判断）](https://leetcode.com/problems/balanced-binary-tree/)  
- [257. Binary Tree Paths（前序 + 回溯 · 路径构建）](https://leetcode.com/problems/binary-tree-paths/)  
- [404. Sum of Left Leaves（前序 · 父节点判断左叶子）](https://leetcode.com/problems/sum-of-left-leaves/)  
- [222. Count Complete Tree Nodes（递归 / 数学优化 · 完全二叉树性质）](https://leetcode.com/problems/count-complete-tree-nodes/)  

**今日状态：**  
重点理解了如何在递归中“返回多层信息”，以及在函数参数中传递“上下文状态（如父节点信息、路径）”。  
还标记了完全二叉树优化做法（待研究）。

---

### 💡 110. Balanced Binary Tree（后序 · 高度返回判断）
- **核心思路：**  
  平衡二叉树的定义：每个节点左右子树高度差 ≤ 1 且左右子树均平衡。  
- **后序遍历求解：**  
  在递归返回高度的同时判断是否平衡。  
- **技巧：**  
  当子树不平衡时，返回 -1 作为信号，后续递归无需继续计算。  
- **实现：**
  ```python
  def isBalanced(root):
      def height(node):
          if not node: return 0
          left = height(node.left)
          if left == -1: return -1
          right = height(node.right)
          if right == -1: return -1
          if abs(left - right) > 1: return -1
          return 1 + max(left, right)
      return height(root) != -1
  ```
- **理解重点：**  
  使用 `-1` 作为“不平衡”信号是一种**递归剪枝技巧**，  
  将逻辑判断嵌入高度计算中，一次遍历即可完成。  

---

### 💡 257. Binary Tree Paths（前序 + 回溯 · 路径构建）
- **核心思路：**  
  输出所有从根到叶节点的路径。  
  典型的**前序遍历 + 回溯**组合题。  
- **关键点：**  
  - 当前节点入路径；
  - 若为叶节点，加入结果；
  - 否则递归左右；
  - 回溯时弹出当前节点（在同一个花括号里完成）。  
- **实现：**
  ```python
  def binaryTreePaths(root):
      res, path = [], []
      def dfs(node):
          if not node: return
          path.append(str(node.val))
          if not node.left and not node.right:
              res.append("->".join(path))
          else:
              dfs(node.left)
              dfs(node.right)
          path.pop()  # 回溯
      dfs(root)
      return res
  ```
- **思维要点：**  
  回溯不是单独的过程，而是递归函数的一部分。  
  必须在“递归结束前”撤销状态。  

---

### 💡 404. Sum of Left Leaves（前序 · 父节点判断左叶子）
- **核心思路：**  
  不能在当前节点判断自己是不是左叶子，  
  因为“左”这个信息来自于父节点。  
- **两种写法：**
  1. **通过参数传递是否为左节点：**
     ```python
     def sumOfLeftLeaves(root, isLeft=False):
         if not root: return 0
         if not root.left and not root.right:
             return root.val if isLeft else 0
         return sumOfLeftLeaves(root.left, True) + sumOfLeftLeaves(root.right, False)
     ```
  2. **通过父节点判断：**
     ```python
     if root.left and not root.left.left and not root.left.right:
         sum += root.left.val
     sum += dfs(root.left) + dfs(root.right)
     ```
- **理解重点：**  
  “左叶子”是一种**相对关系**，必须依赖父节点信息来判断。  

---

### 💡 222. Count Complete Tree Nodes（递归 / 数学优化 · 完全二叉树性质）
- **普通做法：**
  ```python
  def countNodes(root):
      if not root: return 0
      return 1 + countNodes(root.left) + countNodes(root.right)
  ```
  时间复杂度 O(n)。  

- **优化思路（未实现 · 标记研究）：**
  - 若左右子树高度相等，则左子树为满二叉树；
  - 否则右子树为满二叉树；
  - 满二叉树节点数公式：`2^h - 1`。  
  因此复杂度可优化到 **O(log² n)**。  

- **优化版伪代码：**
  ```python
  def countNodes(root):
      if not root: return 0
      left_h, right_h = height(root.left), height(root.right)
      if left_h == right_h:
          return (1 << left_h) + countNodes(root.right)
      else:
          return (1 << right_h) + countNodes(root.left)
  ```
- **标记：** ✅ 待二刷实现优化版本。  

---

### ⚡ 易错点 / 卡壳点
- 110：若不使用 `-1` 早停，会重复计算导致超时。  
- 257：回溯必须与递归写在同一作用域，否则路径不会正确撤销。  
- 404：当前节点无法判断自己是否为左叶子，必须从父节点角度判断。  
- 222：普通解法正确但效率低，优化需理解“完全二叉树高度关系”。  

---

### ✅ 改进方案
- 总结不同递归返回值类型：  
  | 返回值类型 | 示例题 | 返回信息 |
  |-------------|---------|-----------|
  | 布尔 | 110 | 是否平衡 |
  | 字符串列表 | 257 | 所有路径 |
  | 数值 | 404 / 222 | 累加值或节点数 |
- 学会通过“参数传递 + 返回值传递”结合递归上下文信息。  
- 二刷 222 时补上完全二叉树优化写法，并手动画出左右高度对比图。  

---

### 💬 一句话总结
二叉树递归的进阶阶段，不再只是“遍历”，  
而是要**让递归函数能携带信息上下传递**。  
掌握如何“传递状态”“提前终止”“回溯撤销”，  
就能写出既优雅又高效的树结构算法。
