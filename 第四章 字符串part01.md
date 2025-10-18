## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/16  
**主题：** 字符串 01｜反转 · 局部翻转 · 原地替换  
**题目：**  
- [344. Reverse String（双指针 · 原地交换）](https://leetcode.com/problems/reverse-string/)  
- [541. Reverse String II（分组反转 · 区间控制）](https://leetcode.com/problems/reverse-string-ii/)  
- 卡码网 54. 替换数字（模拟 · 原地扩容） → 类似题：[443. String Compression（双指针 · 原地修改）](https://leetcode.com/problems/string-compression/)  

**今日状态：**  
前两题较为直接，但最后一题“替换数字”难度上升，真正体会到双指针在原地修改字符串中的威力。

---

### 🧠 解题思路总结

#### 344. Reverse String（双指针 · 原地交换）
- **核心思路：**  
  用左右指针从两端向中间移动，依次交换字符。  
  ```python
  left, right = 0, len(s) - 1
  while left < right:
      s[left], s[right] = s[right], s[left]
      left += 1
      right -= 1
  ```
- **关键点：**
  - 注意循环条件为 `<` 而非 `<=`。  
  - 字
