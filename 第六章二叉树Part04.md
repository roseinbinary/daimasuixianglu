
## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/24  
**主题：** 第六章 二叉树 Part 04｜路径判断 · 树重建 · 遍历反推结构  
**题目：**  
- [513. Find Bottom Left Tree Value（层序 · 队列遍历）](https://leetcode.com/problems/find-bottom-left-tree-value/)  
- [112. Path Sum（递归 · 判断路径和）](https://leetcode.com/problems/path-sum/)  
- [113. Path Sum II（递归 + 回溯 · 路径收集）](https://leetcode.com/problems/path-sum-ii/)  
- [106. Construct Binary Tree from Inorder and Postorder Traversal（递归构建 · 分治）](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)  
- [105. Construct Binary Tree from Preorder and Inorder Traversal（递归构建 · 分治）](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)  

**今日状态：**  
这几题是二叉树递归的关键练习。  
既巩固了“递归的返回值设计”，也重新理解了“前中后序遍历能否唯一确定树”的规律。

---

### 💡 513. Find Bottom Left Tree Value（层序 · 队列遍历）
- **核心思路：**  
  找出树的最底层、最左侧节点的值。  
- **层序遍历法（推荐）：**
  ```python
  from collections import deque

  def findBottomLeftValue(root):
      queue = deque([root])
      while queue:
          node = queue.popleft()
          # 先右后左，这样最后访问的就是最左下角节点
          if node.right:
              queue.append(node.right)
          if node.left:
              queue.append(node.left)
      return node.val
  ```
- **递归法（可选）：**  
  也可通过递归记录深度和最左节点，但层序更直观。  
- **要点：**  
  在层序遍历中，通过调整入队顺序（右→左）即可让最后一个出队节点为“最左下角”。

---

### 💡 112. Path Sum（递归 · 判断路径和）
- **核心思路：**  
  判断是否存在从根到叶子的路径，其节点值之和等于 targetSum。  
- **递归结构：**
  ```python
  def hasPathSum(root, targetSum):
      if not root:
          return False
      if not root.left and not root.right:
          return root.val == targetSum
      targetSum -= root.val
      return (hasPathSum(root.left, targetSum) or
              hasPathSum(root.right, targetSum))
  ```
- **思维重点：**  
  本题递归返回的是 **布尔值**，表示“是否存在符合条件的路径”。  
  每层递归向上传递的是**是否成功**，而非路径或和。

---

### 💡 113. Path Sum II（递归 + 回溯 · 路径收集）
- **核心思路：**  
  与 112 类似，但需记录所有路径。  
- **递归 + 回溯实现：**
  ```python
  def pathSum(root, targetSum):
      res, path = [], []
      def dfs(node, remain):
          if not node:
              return
          path.append(node.val)
          if not node.left and not node.right and remain == node.val:
              res.append(list(path))
          dfs(node.left, remain - node.val)
          dfs(node.right, remain - node.val)
          path.pop()  # 回溯
      dfs(root, targetSum)
      return res
  ```
- **递归返回值设计：**  
  与 112 不同，这里递归不返回布尔值，而是通过**外部变量 res**收集结果。  
  这是“递归返回值 vs 外部状态”的典型区别。

---

### 💡 106. Construct Binary Tree from Inorder and Postorder（递归构建 · 分治）
- **核心思路：**  
  - **后序遍历**的最后一个元素是根节点；
  - 根据根节点在中序遍历中的位置，可划分左右子树。  
- **实现：**
  ```python
  def buildTree(inorder, postorder):
      if not inorder or not postorder:
          return None
      root_val = postorder.pop()
      root = TreeNode(root_val)
      idx = inorder.index(root_val)
      # 先构造右子树，再构造左子树（因为 postorder 从后往前）
      root.right = buildTree(inorder[idx+1:], postorder)
      root.left = buildTree(inorder[:idx], postorder)
      return root
  ```
- **技巧：**  
  因为 `postorder.pop()` 从末尾取元素，必须**先递归右子树**再左子树。  
- **复杂度：** O(n²)（可通过哈希表优化 index 查找到 O(n)）。

---

### 💡 105. Construct Binary Tree from Preorder and Inorder（递归构建 · 分治）
- **核心思路：**  
  - **前序遍历**的第一个元素是根节点；
  - 根据根节点在中序遍历中的位置，划分左右子树；  
  - 按中序分割区间递归构建。  
- **实现：**
  ```python
  def buildTree(preorder, inorder):
      if not preorder or not inorder:
          return None
      root_val = preorder.pop(0)
      root = TreeNode(root_val)
      idx = inorder.index(root_val)
      root.left = buildTree(preorder, inorder[:idx])
      root.right = buildTree(preorder, inorder[idx+1:])
      return root
  ```
- **注意点：**
  - 由于 `pop(0)` 为 O(n)，建议改用索引范围或 deque 提高效率；
  - **只有中序 + 前序 / 后序** 能唯一确定二叉树；
  - **中序 + 中序 / 前序 + 后序** 无法确定唯一结构。

---

### ⚡ 易错点 / 卡壳点
- 513：若不控制入队顺序（应右→左），结果会错误。  
- 112 vs 113：递归返回值不同；前者是布尔判断，后者是路径集合。  
- 106：构造顺序必须“先右后左”；否则 postorder 元素被错误使用。  
- 105：理解“前序第一个是根，中序定位左右区间”是关键。  

---

### ✅ 改进方案
- 理解递归函数的三种返回模式：  
  | 类型 | 示例 | 含义 |
  |------|------|------|
  | 布尔值 | 112 | 判断条件是否成立 |
  | 集合/路径 | 113 | 收集所有结果 |
  | 树节点 | 105 / 106 | 构建并返回新结构 |
- 手画出前序 / 中序 / 后序 的遍历序列切割线，理解根与左右子树的对应区间。  
- 二刷 105 / 106 时，尝试使用哈希表优化查找索引。  

---

### 💬 一句话总结
这几题让递归真正“活”了起来：  
有的递归返回真假（逻辑判断），有的返回结构（树），有的只是过程（回溯）。  
掌握何时“让递归返回值”，何时“用外部变量收集结果”，  
就掌握了写出清晰、高效树题的核心能力。
