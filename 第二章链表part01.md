## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/11  
**主题：** 链表 01｜基础操作 · 结构设计 · 反转逻辑  
**题目：**  
- [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)  
- [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)  
- [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)  

**今日状态：** 通过多道题逐步熟悉链表结构的节点操作，但在边界与指针理解上仍有困惑。

---

### 🧠 解题思路总结

#### 203. Remove Linked List Elements（删除链表中的节点）
- **核心思路：虚拟头节点 + 单指针遍历**  
  - 当删除的目标可能是头节点时，用一个 dummy node 指向 head，可统一逻辑。  
  - 用 `cur` 遍历链表，判断 `cur.next.val == val` 时跳过节点：`cur.next = cur.next.next`。  
- **关键难点：**
  - 理解 Java 中对象赋值是“引用传递”（浅拷贝）：  
    `ListNode a = head` 并不会复制链表，只是创建了对同一对象的引用。  
  - 若想复制链表，需要新建节点并手动拷贝值（深拷贝）。  

---

#### 707. Design Linked List（链表设计）
- **思路要点：**
  - 需要支持四种操作：`get(index)`, `addAtHead`, `addAtTail`, `addAtIndex`, `deleteAtIndex`。  
  - 使用虚拟头节点 `dummy` 可简化头部插入/删除逻辑。  
  - 同时维护 `size` 变量，用于快速判断合法索引范围。  
- **边界困惑点：**
  - for loop 的循环次数与实际 index 容易混淆：  
    - 若从 dummy 开始计数，循环次数应为 index 次；  
    - 若从 head 开始计数，循环次数应为 index - 1。  
  - 需注意每次增删节点后更新 size 的时机。  

---

#### 206. Reverse Linked List（反转链表）
- **核心思路：三指针法**  
