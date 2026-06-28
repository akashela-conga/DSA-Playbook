# Reverse Linked List Pattern

## Problems Covered

* LC 206 - Reverse Linked List
* LC 92 - Reverse Linked List II
* LC 25 - Reverse Nodes in k-Group

---

# Problem Recognition

Use when:

* Reverse entire list
* Reverse a sublist
* Reverse every K nodes

---

# Core Idea

1. Find the portion to reverse.
2. Cut it.
3. Reverse.
4. Reconnect.

---

# Reverse Template

```java
ListNode reverse(ListNode head){

    ListNode prev = null;

    while(head != null){

        // Save next node before changing pointers
        ListNode next = head.next;

        // Reverse current pointer
        head.next = prev;

        // Move prev forward
        prev = head;

        // Continue traversal
        head = next;
    }

    // prev is the new head
    return prev;
}
```

---

# Get Kth Node

```java
ListNode getKthNode(ListNode head,int k){

    // Move k-1 times
    while(head != null && --k > 0){
        head = head.next;
    }

    // Returns null if k nodes don't exist
    return head;
}
```

---

# Reverse Between (LC 92)

Steps

```java
// Save remaining list
ListNode next = rightNode.next;

// Disconnect sublist
leftPrev.next = null;
rightNode.next = null;

// Reverse sublist
ListNode newHead = reverse(leftNode);

// Connect left half -> reversed list
leftPrev.next = newHead;

// Original head becomes new tail
leftNode.next = next;
```

---

# Reverse K Group (LC 25)

Algorithm

```java
// Find kth node
ListNode kth = getKthNode(curr, k);

if(kth == null){
    tail.next = curr;
    break;
}

// Save next group
ListNode next = kth.next;

// Disconnect current group
kth.next = null;

// Reverse current group
ListNode newHead = reverse(curr);

// Connect previous group
tail.next = newHead;

// Current head becomes new tail
tail = curr;

// Move to next group
curr = next;
```

---

# Things To Remember

* Reverse returns **new head**.
* Original head becomes **new tail**.
* Dummy node avoids head edge cases.
* Always cut before reversing.
* Always reconnect after reversing.

---

# Representative Problems

* LC 206
* LC 92
* LC 25
