## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/27  
**主题：** 动态规划 Part 03｜0/1 背包基础 · 二维 DP · 一维滚动数组优化  
**题目：**  
- 0/1 背包（课内经典题，无对应 LeetCode 原题）  
- 明日目标：416. Partition Equal Subset Sum（0/1 背包应用）

---

# 🎒 0/1 背包基础题（纯净版）

## 📘 题意
有 `n` 件物品，一个容量为 `W` 的背包。  
第 `i` 件物品重量 `weight[i]`、价值 `value[i]`。  
每件物品只能用一次，问能获得的最大价值。

这是最标准的 **0/1 背包模型**。

---

# 🧩 1. 状态定义

### 二维 DP（最容易理解）
`dp[i][j]` 表示：

> **从前 i 件物品中任选，放入容量 j 的背包，能获得的最大价值。**

其中：

- i 范围：0 ~ n-1
- j 范围：0 ~ W

---

# 🔁 2. 状态转移方程（核心）

物品 i 只有两种选择：**不放** 或 **放入背包**。

### ❌ 不放物品 i：
```
dp[i][j] = dp[i - 1][j]
```

### ✔ 放物品 i（前提 j ≥ weight[i]）：
```
dp[i][j] = dp[i - 1][j - weight[i]] + value[i]
```

### ✨ 综合成递推式（关键）
```
dp[i][j] = max( dp[i - 1][j], 
                dp[i - 1][j - weight[i]] + value[i] )
```

---

# 🟦 3. 初始化（关键）

### ✔ 第一行：只考虑 0 号物品
- 若 j < weight[0] → 放不进去 → dp[0][j] = 0  
- 若 j ≥ weight[0] → 可放 → dp[0][j] = value[0]

### ✔ 第一列：容量为 0 → 永远放不下任何物品
```
dp[i][0] = 0
```

### 其他位置？
全填 0 即可（都会在迭代中被覆盖）。

---

# 🔄 4. 遍历顺序

二维 dp 时顺序不影响正确性，但为了理解：

```
for i in range(n):      # 遍历物品
    for j in range(W+1):# 遍历背包容量
```

物品 → 容量（好理解）  
容量 → 物品（也可以）

---

# 🌟 5. 完整二维 DP 代码模板

```python
def knapSack(W, weight, value):
    n = len(weight)
    dp = [[0] * (W + 1) for _ in range(n)]

    # 初始化第一行
    for j in range(weight[0], W + 1):
        dp[0][j] = value[0]

    for i in range(1, n):
        for j in range(0, W + 1):
            if j < weight[i]:
                dp[i][j] = dp[i - 1][j]
            else:
                dp[i][j] = max(dp[i - 1][j],
                               dp[i - 1][j - weight[i]] + value[i])

    return dp[n - 1][W]
```

---

# 🚀 6. 一维 DP（状态压缩）——重点重点重点！

二维转为一维之原因：

```
dp[i][j] 只依赖 dp[i-1][j] 和 dp[i-1][j-weight[i]]
```

因此可以：

```
dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
```

### ❗ 但压缩必须 **倒序遍历 j**
```
for i in range(n):
    for j in range(W, weight[i] - 1, -1):
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
```

### 为什么必须倒序？
你总结得非常准确：

- 正序时：`dp[j - weight[i]]` 可能已经被“本轮 i”更新过 → 相当于物品可以用无限次 → 变成完全背包 ❌
- 倒序时：`dp[j - weight[i]]` 一定来自上一轮 (i-1) → 正确满足 0/1 背包条件 ✔

### 一维完整版模板

```python
def knapSack(W, weight, value):
    n = len(weight)
    dp = [0] * (W + 1)

    for i in range(n):
        for j in range(W, weight[i] - 1, -1):
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])

    return dp[W]
```

---

# 🎯 7. 今日收获总结（理解非常关键）

- 0/1 背包的关键在于：**物品只能选或不选一次**
- 二维 DP 易理解，一维 DP 提高效率  
- **状态压缩必须使用倒序遍历**（避免变成完全背包）
- 初始化非常重要，尤其第 0 行和第 0 列
- 递推方向：物品 → 背包容量

---

# 🔥 8. 明日任务预告：416. Partition Equal Subset Sum

这是 **01 背包在 LeetCode 上最经典的应用题之一**：

目标是判断：
```
是否可以从数组中选 SOME 元素，使它们的和 = total_sum / 2
```

✔ 本质：  
**转化为 0/1 背包、容量为 target、重量为 nums[i]、价值等于重量**

明天做这一题将极大巩固你今天学到的核心内容。

