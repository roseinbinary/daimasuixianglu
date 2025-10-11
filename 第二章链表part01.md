## 🧩 LeetCode 刷题日记  
**日期：** 2025/10/11  
**主题：** 链表 01｜基础操作 · 结构设计 · 反转逻辑  
**题目：**  
- [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)  
- [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)  
- [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)  

**今日状态：**  
通过多道题逐步熟悉链表结构的节点操作，但在边界与指针理解上仍有困惑。

---

### 🧠 解题思路总结

#### 203. Remove Linked List Elements（删除链表中的节点）
- **核心思路：虚拟头节点 + 单指针遍历**  
  当删除的目标可能是头节点时，用一个 dummy node 指向 head，可统一逻辑。  
  用 `cur` 遍历链表，判断 `cur.next.val == val` 时跳过节点：  
  ```java
  cur.next = cur.next.next;
  ```
- **关键难点：**
  - 理解 Java 中对象赋值是**引用传递（浅拷贝）**：  
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
    - 若从 dummy 开始计数，循环次数应为 `index` 次；  
    - 若从 head 开始计数，循环次数应为 `index - 1`。  
  - 需注意每次增删节点后更新 `size` 的时机。

---

#### 206. Reverse Linked List（反转链表）
- **核心思路：三指针法**
  ```python
  prev = None
  cur = head
  while cur:
      nxt = cur.next
      cur.next = prev
      prev = cur
      cur = nxt
  return prev
  ```
- **关键理解：**  
  每次循环反转当前节点指针方向，退出条件是 `cur == None`。  
  反转后，`prev` 即为新链表的头。  
- **本质：**  
  指针操作的最小单位是“边”，不是“节点”——每次改一条 `next` 的指向。

---

### ⚡ 易错点 / 卡壳点
- 203 题中，若不使用虚拟头节点，删除头节点时逻辑复杂。  
- 707 题中，`index` 的循环边界与 `size` 更新时机容易出错（建议画出链表节点编号）。  
- 206 题中，若 while 条件写成 `while cur.next:` 会漏掉最后一个节点。  
- 对 Java 引用（浅拷贝/深拷贝）理解不牢固，容易误以为赋值创建了新节点。

---

### ✅ 改进方案
- 每次操作链表前，**画出节点与指针关系图**，可防止思维混乱。  
- 代码调试时多打印节点值与 next 指针，帮助直观看清引用关系。  
- 对于设计题（707），先实现单功能（如 `addAtHead` / `get`），确认正确后再组合。  
- 用伪代码或注释写出循环含义，比如：  
  ```java
  // move to node before index
  ```
  明确循环边界的逻辑。

---

### 💬 一句话总结
链表考验的是“指针关系的直觉”。  
今天的重点是：理解引用传递，习惯虚拟头节点，熟悉指针交换的顺序。  
当脑中能画出指针走向时，链表题就通了。
