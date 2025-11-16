## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/28  
**主题：** 第九章 动态规划 Part 07｜打家劫舍 Ⅰ·Ⅱ·Ⅲ  
**题目：**  
- [198. House Robber（线性 DP · 相邻不可偷）](https://leetcode.com/problems/house-robber/)  
- [213. House Robber II（环形 DP · 头尾冲突）](https://leetcode.com/problems/house-robber-ii/)  
- [337. House Robber III（二叉树 DP · 二维状态）](https://leetcode.com/problems/house-robber-iii/)  

---

# 🎯 今日主题：打家劫舍三部曲

三个题目难度从线性 → 环形 → 树形  
关键点从：

- **偷 or 不偷** 1 维状态  
- 到 **头尾不能同时偷** 的区间 DP  
- 再到需要 **二维状态记录树节点状态** 的递归 DP

你已经抓住了今天的重点：  
> **337 必须用二维状态（偷 or 不偷）记录每个节点的最佳结果。**

---

## 💡 198. House Robber（线性 DP · 偷 or 不偷）

### 🧩 状态定义  
`dp[i]` = 偷到第 i 间房子可获得的最大金额。

### 🔁 状态转移  
```
dp[i] = max(
    dp[i-1],           # 不偷第 i 间
    dp[i-2] + nums[i]  # 偷第 i 间
)
```

### 🔧 关键思想  
- 相邻不能偷  
- 只依赖前两个状态 → 可用 O(1) 空间优化  

### ✔ 优化版本  
```
prev2, prev1 = 0, 0
for x in nums:
    prev2, prev1 = prev1, max(prev1, prev2 + x)
return prev1
```

---

## 💡 213. House Robber II（环形 DP · 不能同时偷第一和最后一家）

### 🧩 本质  
房子围成一圈：  
- 偷第一家 → 不能偷最后一家  
- 偷最后一家 → 不能偷第一家  

因此拆成两次 198：  
```
case1 = rob(nums[0 : n-1])
case2 = rob(nums[1 : n])
return max(case1, case2)
```

### 🚩 特别注意  
- 若数组长度为 1：直接返回 nums[0]  
- 其它情况必须分成两个区间

这是典型的 **区间动态规划**。

---

## 💡 337. House Robber III（树形 DP · 二维状态）

### 🌳 本题核心：  
**树结构没有线性相邻关系，因此不能用一维 dp。  
必须同时记录“偷当前节点”与“不偷当前节点”的最大收益。**

### 🧩 状态定义（关键）  
对每个节点 `node`，定义：

- `dp[node][0]`：不偷当前节点的最大金额  
- `dp[node][1]`：偷当前节点的最大金额  

这就是你说的 **“2维数组记录状态”** —— 非常关键。

### 🔁 状态转移  

1. **偷当前节点**  
   → 左右子节点都不能偷  
```
dp[node][1] = node.val + dp[left][0] + dp[right][0]
```

2. **不偷当前节点**  
   → 左右子节点可偷可不偷，选最大  
```
dp[node][0] = max(dp[left][0], dp[left][1]) 
            + max(dp[right][0], dp[right][1])
```

### 🔧 后序遍历（左右 → 根）  
因为 dp[node] 依赖左右子树，因此必须后序：

```
def dfs(node):
    if not node:
        return (0, 0)
    left = dfs(node.left)
    right = dfs(node.right)
    rob = node.val + left[0] + right[0]
    not_rob = max(left) + max(right)
    return (not_rob, rob)
```

### ⭐ 最终答案
```
not_rob, rob = dfs(root)
return max(not_rob, rob)
```

---

# ⚡ 今日易错点 / 难点

- **198**：不要把 `dp[i-1]` 和 `dp[i-2] + nums[i]` 逻辑搞混  
- **213**：忘记拆成两个区间就会错成“偷了头又偷尾”  
- **337（重点）**：  
  - 必须用二元组 `(不偷, 偷)`  
  - 必须后序遍历  
  - 不能用 198 或 213 的线性思维硬套  

---

# ✅ 今日总结

今天掌握了 3 种 DP 的“结构变化”：

| 题目 | 结构 | 状态 | 关键点 |
|------|------|--------|--------|
| 198 | 线性 | 1 维 dp | 偷 or 不偷 |
| 213 | 环形 | 1 维 dp，两段区间 | 头尾冲突 |
| 337 | 树形 | **二维 dp** | 左右子树依赖 |

你今天已经完全掌握了树形 DP 的核心思想：  
> **节点状态必须用二维记录，否则表达不完整。**  

---

# 💬 一句话总结  
线性偷家 → 区间偷家 → 树上偷家  
变化的只是结构，**不变的是“选择 + 最优”**。  
动态规划的本质始终是：  
**每个状态必须包含做决策所需的全部信息。**

