## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/10  
**主题：** 数组 02｜滑动窗口 · 模拟 · 前缀和思维拓展  
**题目：**  
- [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)  
- [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)  
- 自编题①（前缀和版区间求和） → 类似题：[303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)  
- 自编题②（矩阵划分最小差） → 类似题：[304. Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)  

**今日状态：** 学到了如何从暴力解法过渡到双指针/滑动窗口，并意识到前缀和是处理区间和问题的核心工具。

---

### 🧠 解题思路总结

#### 209. Minimum Size Subarray Sum  
- **核心思路：滑动窗口（双指针）**  
  从暴力法（枚举所有子数组）优化为动态维护一个窗口 `[left, right)`。  
  - 右指针不断扩展窗口，累加当前和。  
  - 当 `sum >= target` 时，说明窗口已满足条件 → 记录长度并尝试收缩左边界以获取更短长度。  
  - 左指针收缩时要减去离开的值。  
- 本质上是一种“贪心式双指针”：每次尽量压缩区间以获得最短满足条件的窗口。  
- 时间复杂度降至 **O(n)**。  

#### 59. Spiral Matrix II  
- **思路类型：模拟 + 边界控制**  
  - 定义四个边界：`top`, `bottom`, `left`, `right`。  
  - 顺序依次为：→ 右 → 下 → 左 → 上，填充数字并收缩边界。  
  - 循环终止条件：`left > right` 或 `top > bottom`。  
- **难点：**
  - 每次更新边界顺序要正确（top++, right--, bottom--, left++）。  
  - 需要注意行列交替时的方向变化（按螺旋顺序切换）。  
- **技巧：**  
  用 `dirs = [(0,1),(1,0),(0,-1),(-1,0)]` 来表示方向数组，可通过方向索引循环控制转向。  

---

#### 自编题①：区间求和（Prefix Sum 一维版）  
**题意：** 给定数组和若干查询，每次求 `[a,b]` 区间的和。  
**关键思路：前缀和数组**  
- 构建 `prefix[i] = prefix[i-1] + nums[i]`。  
- 区间和可在 O(1) 时间获得：`sum(a,b) = prefix[b] - prefix[a-1]`。  
**类似题：** [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

---

#### 自编题②：二维划分最小差（Prefix Sum 二维版）  
**题意：** 将 n×m 矩阵横或纵划分为两部分，使两部分的和差最小。  
**关键思路：二维前缀和 + 枚举分割线**  
- 构建二维前缀和矩阵 `preSum[i][j]` 表示从 (0,0) 到 (i,j) 的矩形和。  
- 然后：
  - 枚举行方向划分线：计算上方和下方总和差。  
  - 枚举列方向划分线：计算左方和右方总和差。  
- 取两种划分中的最小差。  
**类似题：** [304. Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)  
**延伸思路：** 这类问题与「最大矩形和」「区域划分 DP」问题思路相通。

---

### ⚡ 易错点 / 卡壳点
- 209 题中忘记在 sum ≥ target 时立刻更新结果并收缩左边界。  
- 59 题模拟时容易漏掉边界判断或顺序错误（比如右下左上循环中忘记更新对应边界）。  
- 自编题① 初始 prefix 数组要多开一位（prefix[0]=0）。  
- 自编题② 容易混淆划分时使用的下标范围（二维前缀和下标偏移较多）。  

---

### ✅ 改进方案
- 遇到滑动窗口题，先思考“如何动态维护一个可行区间”，再考虑边界条件。  
- 模拟题建议先画图 → 明确边界收缩顺序后再写代码。  
- 前缀和题统一遵循模板：**preSum[0]=0**，`preSum[i]=preSum[i-1]+nums[i-1]`，并多测试边界区间。  
- 练习二维前缀和时先从 1D 到 2D 逐步推演，体会公式转化。  

---

### 💬 一句话总结
今天进一步掌握了从暴力到高效的“思维跃迁”：双指针让搜索更精确，前缀和让求和更快捷，而模拟题考验的是逻辑严谨性与边界控制力。  
