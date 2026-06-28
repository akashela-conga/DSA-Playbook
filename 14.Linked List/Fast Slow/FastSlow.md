# Fast & Slow Pointer (Floyd's Cycle Detection)

## When to Think of This Pattern

* Detect cycle in Linked List
* Find cycle entry
* Find middle node
* Repeated transformations that may form a cycle (Happy Number)

---

# LC 141 - Linked List Cycle

**Pattern:** Fast & Slow Pointer

**Algorithm:** Floyd's Cycle Detection (Tortoise & Hare)

### Idea

* Slow moves 1 step.
* Fast moves 2 steps.
* If they meet → cycle exists.

```java
while(fast != null && fast.next != null){
    slow = slow.next;
    fast = fast.next.next;

    if(slow == fast){
        return true;
    }
}
```

**Time:** O(n)

**Space:** O(1)

---

# LC 142 - Linked List Cycle II

**Pattern:** Fast & Slow Pointer

**Algorithm:** Floyd's Cycle Detection + Cycle Entry

### Idea

After slow and fast meet:

* Keep one pointer at meeting point.
* Start another from head.
* Move both one step.
* They meet at the cycle start.

```java
private ListNode findCycleStart(ListNode head, ListNode meet){
    ListNode curr = head;

    while(curr != meet){
        curr = curr.next;
        meet = meet.next;
    }

    return meet;
}
```

```java
if(fast == slow){
    return findCycleStart(head, fast);
}
```

**Time:** O(n)

**Space:** O(1)

---

# LC 876 - Middle of the Linked List

**Pattern:** Fast & Slow Pointer

**Algorithm:** Tortoise & Hare

### Idea

When fast reaches the end, slow is at the middle.

```java
private ListNode getMiddle(ListNode head){
    ListNode slow = head;
    ListNode fast = head;

    while(fast != null && fast.next != null){
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;
}
```

```java
return getMiddle(head);
```

**Time:** O(n)

**Space:** O(1)

---

# LC 202 - Happy Number

**Pattern:** Fast & Slow Pointer

**Algorithm:** Floyd's Cycle Detection

### Idea

Treat every transformed number as the next node.

Example:

```
19
↓
82
↓
68
↓
100
↓
1
```

If a cycle doesn't reach **1**, it is **not** a happy number.

### Helper

Generates the next number by taking the sum of squares of its digits.

```java
private int nextNumber(int n){
    int sum = 0;

    while(n > 0){
        int digit = n % 10;
        sum += digit * digit;
        n /= 10;
    }

    return sum;
}
```

### Floyd's Cycle Detection

```java
int slow = n;
int fast = n;

do{
    slow = nextNumber(slow);                  // 1 step
    fast = nextNumber(nextNumber(fast));      // 2 steps
}while(slow != fast);

return slow == 1;
```

**Time:** O(log n)

**Space:** O(1)

---

# Interview Reminders

* **Cycle?** → Floyd's Cycle Detection
* **Cycle Entry?** → Move one pointer to head after first meeting
* **Middle?** → Fast = 2x Slow
* **Repeated transformation?** → Model as a linked list and apply Floyd's algorithm
