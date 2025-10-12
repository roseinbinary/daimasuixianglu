## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/12  
**主题：** 链表 02｜双指针 · 节点交换 · 相交与环检测  
**题目：**  
- [24. Swap Nodes in Pairs（模拟 · 指针重排）](https://leetcode.com/problems/swap-nodes-in-pairs/)  
- [19. Remove Nth Node From End of List（双指针 · 间距控制）](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)  
- [160. Intersection of Two Linked Lists（双指针 · 起点对齐）](https://leetcode.com/problems/intersection-of-two-linked-lists/)  
- [142. Linked List Cycle II（快慢指针 · 环检测）](https://leetcode.com/problems/linked-list-cycle-ii/)  

**今日状态：**  
链表的双指针部分逐渐明朗，开始体会“指针间的相对距离”是很多问题的本质。

---

### 🧠 解题思路总结

#### 24. Swap Nodes in Pairs（模拟 · 指针重排）
- **关键技巧：** 虚拟头节点 + 四个指针（cur, first, second, temp）。  
- 操作步骤：
  1. 初始化：`dummyHead -> head`, `cur = dummyHead`。  
  2. 记录节点：`first = cur.next`, `second = first.next`, `temp = second.next`。  
  3. 执行交换三步：
     ```python
     cur.next = second
     second.next = first
     first.next = temp
     ```
  4. 移动 cur 到下一对：`cur = first`。  
- **退出条件：** `cur.next == None` 或 `cur.next.next == None`。  
- **思维重点：** 每轮操作只负责局部（两节点）交换，通过 dummyHead 保证统一结构。  

---

#### 19. Remove Nth Node From End of List（双指针 · 间距控制）
- **核心思路：双指针保持固定间距 n。**
  1. 使用 `dummy -> head`，`fast` 和 `slow` 都从 dummy 开始。  
  2. `fast` 先走 `n+1` 步（保持间距为 n）。  
  3. 然后 `slow` 与 `fast` 一起走，直到 `fast == null`。  
  4. 此时 `slow.next` 即为要删除的节点。  
  5. 执行删除：`slow.next = slow.next.next`。  
- **时间复杂度：** O(n)，只遍历一次。  
- **难点：** 理解“间距为 n”的实质是让 slow 恰好停在目标前一位。  

---

#### 160. Intersection of Two Linked Lists（双指针 · 起点对齐）
- **核心思路：对齐起点，使两链表同步遍历。**
  1. 先分别计算两链表长度 `lenA`, `lenB`。  
  2. 让长的先走 `abs(lenA - lenB)` 步，使两者剩余长度相同。  
  3. 然后同步前进，第一次相等的节点即为相交点。  
- **直觉误区：** 一开始容易以为“短链表会错过相交点”。  
  实际上，**对齐起点**后两者走的路径长度相同，不会错过。  
- **本质：** 通过“差距对齐”让两个遍历者在同一个相对位置上比较节点引用。  

---

#### 142. Linked List Cycle II（快慢指针 · 环检测）
- **思路：快慢指针 + 数学推理**
  1. 快指针每次走两步，慢指针走一步。  
  2. 若有环，二者必定在环内某点相遇。  
  3. 相遇后，让一指针回到起点，两指针每次走一步，再次相遇处即环的入口。  
- **推理核心：**  
  设：
  - 起点到环入口距离为 `a`
  - 环入口到相遇点距离为 `b`
  - 环长度为 `c`
  
  当两指针相遇时：
  ```
  fast 走的路 = 2 * slow 走的路
  a + b + n*c = 2(a + b)
  => a = n*c - b
  ```
  意味着从头走 a 步，与从相遇点走 (c - b) 步将再次相遇于环入口。  
- **目前状态：** 虽然能看懂思路，但需要再多次画图推演理解“为什么相遇后再从头出发一定能到环入口”。  

---

### ⚡ 易错点 / 卡壳点
- 24 题：交换节点时 `cur = first` 忘记更新会导致死循环。  
- 19 题：`fast` 要先走 `n+1` 而不是 `n`，否则 `slow` 会停错位置。  
- 160 题：理解“对齐后同步走”需要画图才能真正领悟。  
- 142 题：难点在数学推理部分，对“a = n*c - b”的理解仍需加强。  

---

### ✅ 改进方案
- 对每个双指针题都先画图 → 标出指针间距、起点、终点。  
- 反复练习“先固定间距再同步移动”的模式。  
- 对 142 题建议手动画环路、标记相遇点与入口位置。  
- 熟悉 dummyHead 的用途（统一逻辑、简化边界处理）。  

---

### 💬 一句话总结
今天深刻体会到：  
**链表的核心不是节点，而是指针的“相对距离”和“相遇时机”。**  
当你能准确控制这些关系时，几乎所有链表题都能被拆解成快慢指针或局部反转问题。
