## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/22  
**主题：** 第六章 二叉树 Part 02｜翻转 · 对称 · 深度计算  
**题目：**  
- [226. Invert Binary Tree（递归 · 前序翻转）](https://leetcode.com/problems/invert-binary-tree/)  
- [101. Symmetric Tree（递归 · 镜像判断）](https://leetcode.com/problems/symmetric-tree/)  
- [104. Maximum Depth of Binary Tree（递归 · 求最大深度）](https://leetcode.com/problems/maximum-depth-of-binary-tree/)  
- [111. Minimum Depth of Binary Tree（递归 · 求最小深度）](https://leetcode.com/problems/minimum-depth-of-binary-tree/)  

**今日状态：**  
理解了二叉树的“结构性递归”思想，每个题都在问同一个问题：  
**如何让左右子树通过递归返回信息并组合成父节点的结果。**

---

### 💡 226. 翻转二叉树（递归 · 前序翻转）
- **核心思路：** 交换每个节点的左右子树。  
- **关键点：**  
  - 只能使用 **前/后序遍历（root → left → right）**；  
  - 若使用中序遍历，在递归访问右子树前，左右已被翻转，逻辑会被反复反转。  
- **实现：**
  ```python
  def invertTree(root):
      if not root:
          return None
      root.left, root.right = root.right, root.left
      invertTree(root.left)
      invertTree(root.right)
      return root
  ```
- **理解技巧：**  
  翻转是“自上而下”的操作，先翻父节点，再翻子节点。  
  画图帮助直观理解每次翻转对指针的影响。  

---

### 💡 101. 对称二叉树（递归 · 镜像判断）
- **核心思路：** 判断树是否为“自身的镜像”。  
  → 即判断左子树与右子树是否对称。  
- **递归定义：**
  - 若两节点都为空 → 对称；  
  - 若一个为空另一个不为空 → 不对称；  
  - 若值不同 → 不对称；  
  - 否则递归判断：  
    ```python
    isMirror(left.left, right.right) and isMirror(left.right, right.left)
    ```
- **代码结构：**
  ```python
  def isSymmetric(root):
      def isMirror(left, right):
          if not left and not right: return True
          if not left or not right: return False
          return (left.val == right.val and
                  isMirror(left.left, right.right) and
                  isMirror(left.right, right.left))
      return isMirror(root.left, root.right)
  ```
- **理解重点：**  
  第一层调用是 `isMirror(root.left, root.right)`，  
  后续每层递归都是左右“交叉对称”比较。  

---

### 💡 104. 二叉树的最大深度（递归 · 高度计算）
- **核心思路：**  
  节点的最大深度 = `1 + max(左子树深度, 右子树深度)`。  
- **实现：**
  ```python
  def maxDepth(root):
      if not root:
          return 0
      return 1 + max(maxDepth(root.left), maxDepth(root.right))
  ```
- **本质：**  
  深度问题是典型的 **后序遍历（左 → 右 → 根）**，  
  因为要先得到子树的深度，再计算当前节点的深度。  
- **重点理解：**  
  深度的含义是“从根到叶的最长路径节点数”。  

---

### 💡 111. 二叉树的最小深度（递归 · 特殊边界处理）
- **核心思路：**  
  节点的最小深度 = `1 + min(左子树深度, 右子树深度)`。  
  ⚠️ 但只有当 **左右子树都存在** 时才能取 min。  
- **陷阱：**  
  若当前节点缺少一侧子树，直接取 `min()` 会返回 0，导致错误。  
- **正确写法：**
  ```python
  def minDepth(root):
      if not root:
          return 0
      if not root.left and not root.right:
          return 1
      if not root.left:
          return 1 + minDepth(root.right)
      if not root.right:
          return 1 + minDepth(root.left)
      return 1 + min(minDepth(root.left), minDepth(root.right))
  ```
- **关键理解：**  
  只有“通向叶子”的路径才算深度，  
  缺失子树的节点不能作为叶节点返回。  

---

### ⚡ 易错点 / 卡壳点
- 226：中序遍历会导致二次翻转（先右后左的顺序被破坏）。  
- 101：递归判断需“交叉比较”，不是左右各自独立比较。  
- 104：搞清“深度”与“高度”的定义，递归是自底向上。  
- 111：最小深度判断需排除只有一侧子树的节点。  

---

### ✅ 改进方案
- 画出每道题的递归树，观察递归返回的含义（尤其是 226 与 111）。  
- 总结“前序 vs 后序”的使用规律：  
  | 遍历类型 | 应用场景 | 原因 |
  |-----------|-----------|------|
  | 前序 | 修改结构（如翻转） | 先处理当前节点再递归 |
  | 后序 | 汇总信息（如深度、路径和） | 先得出子结果再组合 |
- 在 101、104、111 题中体会“递归函数返回值就是子问题答案”的思想。  

---

### 💬 一句话总结
二叉树的递归，本质是让每个节点**独立思考自己要返回给父节点的信息**。  
翻转是“修改结构”，对称是“比较结构”，深度是“汇总结构”——  
不同题目考察的，其实都是你能否**从局部视角看全局关系**。
