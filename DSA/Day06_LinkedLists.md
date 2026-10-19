# 🧮 DSA — Day 06: Linked Lists
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 The Mental Model

A Linked List is a chain of nodes where each node holds:
- **data** → the value
- **next** → a pointer/reference to the next node

Unlike arrays, nodes are **scattered in memory** — there's no index. You traverse from `head` one step at a time.

```
head → [10 | *] → [20 | *] → [30 | *] → [40 | None]
```

### Python Node Definition
```python
class ListNode:
    def __init__(self, val: int = 0, next=None):
        self.val = val
        self.next = next

# Build a list: 1 → 2 → 3 → 4 → 5
def build_list(values: list[int]) -> ListNode:
    dummy = ListNode(0)
    cur = dummy
    for v in values:
        cur.next = ListNode(v)
        cur = cur.next
    return dummy.next
```

---

## 📌 Problem 1: Reverse Linked List (LeetCode #206)
> **Problem:** Reverse a singly linked list. Return the new head.

### 💡 Iterative: Three-Pointer Technique — $O(N)$ Time, $O(1)$ Space

```
Before: 1 → 2 → 3 → 4 → 5 → None
After:  5 → 4 → 3 → 2 → 1 → None
```

#### 🐍 Python 3
```python
def reverseList(head: ListNode | None) -> ListNode | None:
    prev = None
    curr = head
    while curr:
        next_node = curr.next   # save next before overwriting
        curr.next = prev        # reverse the link
        prev = curr             # advance prev
        curr = next_node        # advance curr
    return prev  # prev is now the new head
```

#### ☕ Java
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null, curr = head;
        while (curr != null) {
            ListNode nextNode = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextNode;
        }
        return prev;
    }
}
```

---

## 📌 Problem 2: Merge Two Sorted Lists (LeetCode #21)
> **Problem:** Merge two sorted linked lists. Return the merged list (sorted).

### 💡 Dummy Head Technique — $O(N + M)$ Time, $O(1)$ Space

Use a dummy node to avoid handling edge cases for empty lists.

#### 🐍 Python 3
```python
def mergeTwoLists(list1: ListNode | None, list2: ListNode | None) -> ListNode | None:
    dummy = ListNode(0)
    cur = dummy
    while list1 and list2:
        if list1.val <= list2.val:
            cur.next = list1
            list1 = list1.next
        else:
            cur.next = list2
            list2 = list2.next
        cur = cur.next
    cur.next = list1 if list1 else list2  # attach remaining
    return dummy.next
```

#### ☕ Java
```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(0), cur = dummy;
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) { cur.next = list1; list1 = list1.next; }
            else { cur.next = list2; list2 = list2.next; }
            cur = cur.next;
        }
        cur.next = (list1 != null) ? list1 : list2;
        return dummy.next;
    }
}
```

---

## 📌 Problem 3: Linked List Cycle Detection (LeetCode #141)
> **Problem:** Determine if the linked list has a cycle.

### 💡 Floyd's Tortoise and Hare Algorithm — $O(N)$ Time, $O(1)$ Space

Two pointers: `slow` moves 1 step, `fast` moves 2 steps.
If there's a cycle, fast will eventually lap slow and they'll meet.

#### 🐍 Python 3
```python
def hasCycle(head: ListNode | None) -> bool:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            return True
    return False
```

#### ☕ Java
```java
class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) return true;
        }
        return false;
    }
}
```

---

## 📌 Problem 4: Find Middle of Linked List (LeetCode #876)
> **Problem:** Return the middle node. If two middles, return the second.

#### 🐍 Python 3
```python
def middleNode(head: ListNode | None) -> ListNode | None:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow   # when fast reaches end, slow is at middle
```

#### ☕ Java
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```

---

## 📌 Problem 5: Remove Nth Node From End (LeetCode #19)
> **Problem:** Remove the nth node from the end of the list. Return head.

### 💡 Two-Pointer Gap Technique — one pass $O(N)$

Move `fast` pointer n+1 steps ahead, then move both until `fast` reaches end. `slow.next` is the node to remove.

#### 🐍 Python 3
```python
def removeNthFromEnd(head: ListNode | None, n: int) -> ListNode | None:
    dummy = ListNode(0, head)
    slow = fast = dummy
    for _ in range(n + 1):
        fast = fast.next
    while fast:
        slow = slow.next
        fast = fast.next
    slow.next = slow.next.next   # skip the target node
    return dummy.next
```

#### ☕ Java
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode slow = dummy, fast = dummy;
        for (int i = 0; i <= n; i++) fast = fast.next;
        while (fast != null) { slow = slow.next; fast = fast.next; }
        slow.next = slow.next.next;
        return dummy.next;
    }
}
```
