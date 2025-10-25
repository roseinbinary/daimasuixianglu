## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/21  
**主题：** 第六章 二叉树 Part 01｜前序 · 中序 · 后序 · 层序遍历  
**题目：**  
- [144. Binary Tree Preorder Traversal（递归遍历 · 前序）](https://leetcode.com/problems/binary-tree-preorder-traversal/)  
- [94. Binary Tree Inorder Traversal（递归遍历 · 中序）](https://leetcode.com/problems/binary-tree-inorder-traversal/)  
- [145. Binary Tree Postorder Traversal（递归遍历 · 后序）](https://leetcode.com/problems/binary-tree-postorder-traversal/)  
- [102. Binary Tree Level Order Traversal（层序遍历 · 队列）](https://leetcode.com/problems/binary-tree-level-order-traversal/)  

**今日状态：**  
掌握了二叉树的递归与层序遍历基础，对“树的遍历顺序与输出结构”有了初步理解。  
迭代与统一迭代的写法尚未深入，先留作后续补充学习。

---

### 🌲 理论回顾：树的遍历方式

1. **深度优先遍历（DFS）**  
   - 前序遍历：根 → 左 → 右  
   - 中序遍历：左 → 根 → 右  
   - 后序遍历：左 → 右 → 根  
   - 一般通过 **递归** 或 **栈（迭代）** 实现。  

2. **广度优先遍历（BFS）**  
   - 按层访问，常通过 **队列** 实现。  
   - 典型应用：层次遍历（Level Order）、最短路径、树的右视图等。

---

### 💡 144. Binary Tree Preorder Traversal（递归遍历 · 前序）
- **核心思路：** 根节点 → 左子树 → 右子树。  
- **实现方式：**
  ```python
  def preorderTraversal(root):
      res = []
      def dfs(node):
          if not node: return
          res.append(node.val)
          dfs(node.left)
          dfs(node.right)
      dfs(root)
      return res
  ```
- **特点：** 根节点最先访问，适合序列化、复制等操作。  

---

### 💡 94. Binary Tree Inorder Traversal（递归遍历 · 中序）
- **核心思路：** 左子树 → 根节点 → 右子树。  
- **实现方式：**
  ```python
  def inorderTraversal(root):
      res = []
      def dfs(node):
          if not node: return
          dfs(node.left)
          res.append(node.val)
          dfs(node.right)
      dfs(root)
      return res
  ```
- **特点：** 若二叉树为二叉搜索树（BST），中序遍历结果即为有序序列。  

---

### 💡 145. Binary Tree Postorder Traversal（递归遍历 · 后序）
- **核心思路：** 左子树 → 右子树 → 根节点。  
- **实现方式：**
  ```python
  def postorderTraversal(root):
      res = []
      def dfs(node):
          if not node: return
          dfs(node.left)
          dfs(node.right)
          res.append(node.val)
      dfs(root)
      return res
  ```
- **特点：** 后序遍历常用于“删除树”、“计算子树值”等需要从下至上的问题。  

---

### 💡 102. Binary Tree Level Order Traversal（层序遍历 · 队列）
- **核心思路：**  
  按层访问节点，利用队列先进先出的特性。  
- **实现方式：**
  ```python
  from collections import deque

  def levelOrder(root):
      if not root: return []
      res, queue = [], deque([root])
      while queue:
          level = []
          for _ in range(len(queue)):
              node = queue.popleft()
              level.append(node.val)
              if node.left: queue.append(node.left)
              if node.right: queue.append(node.right)
          res.append(level)
      return res
  ```
- **特点：**  
  典型的 BFS 应用，能自然分层收集节点。

---

### ⚙️ 统一迭代法（尚未学习 · 待补）
- **思路简介：**  
  使用栈模拟递归过程，通过添加 “访问标记” 或 “空节点” 的方式，统一处理前中后序。  
- **优点：**  
  - 同一模板即可实现不同顺序；
  - 更易转为非递归形式；
  - 有助于理解栈与函数调用栈之间的对应关系。  
- **状态：** Mark ✅，待深入。

---

### ⚡ 易错点 / 卡壳点
- 忘记在递归中 return 或遗漏空节点判断；  
- 层序遍历中忘记控制每层元素数量（会导致错层输出）；  
- 递归顺序写错（尤其是前序/后序中 append 的位置）。

---

### ✅ 改进方案
- 画图辅助理解递归顺序（在纸上标记入栈、出栈时机）。  
- 练习将递归改写为迭代（栈实现）版本。  
- 之后补充学习“统一迭代法”，为后续二叉树遍历题做铺垫。

---

### 💬 一句话总结
树的遍历是“递归思想”的第一课：  
**理解递归的本质 = 理解树的遍历。**  
前序关注“根在前”，中序关注“根在中”，后序关注“根在后”；  
层序遍历则让你第一次真正看见“队列”在算法中的威力。
