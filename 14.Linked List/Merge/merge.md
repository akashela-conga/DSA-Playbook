# Linked List Merge Patterns (Merge 2 Lists, Merge K Lists, Sort List)

## ⭐ Important Interview Points

### 1. Dummy Node Pattern

```java
ListNode dummyNode = new ListNode(-1);
ListNode curr = dummyNode;
```

* Avoids handling the head separately.
* Return `dummyNode.next`.

---

### 2. 🚨 Most Common Mistake (I forgot this in all 3 problems)

After attaching any node:

```java
curr.next = list1;   // or list2 / top
curr = curr.next;    // DON'T FORGET THIS
```

If you forget

```java
curr = curr.next;
```

then the next assignment overwrites `curr.next` and the merged list becomes incorrect.

This applies to:

* Merge Two Sorted Lists
* Merge K Sorted Lists
* Sort List (merge function)

---

### 3. Remaining Nodes

Instead of another merge loop, you can directly attach the remaining list.

```java
if(list1 != null)
    curr.next = list1;

if(list2 != null)
    curr.next = list2;
```

---

### 4. Merge K Lists

Only put the first node of every list into the PriorityQueue.

Whenever a node is removed:

* attach it
* push its next node (if present)

---

### 5. Sort List

Remember the sequence:

```
Find Middle
↓
Split
↓
Sort Left
↓
Sort Right
↓
Merge
```

---

# Merge Two Sorted Lists

### Complexity

* Time : **O(n + m)**
* Space : **O(1)**

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummyNode = new ListNode(-1);
        ListNode curr = dummyNode;

        while(list1 != null && list2 != null){
            if(list1.val < list2.val){
                curr.next = list1;
                list1 = list1.next;
            }else{
                curr.next = list2;
                list2 = list2.next;
            }
            curr = curr.next;
        }

        while(list1 != null){
            curr.next = list1;
            list1 = list1.next;
            curr = curr.next;
        }

        while(list2 != null){
            curr.next = list2;
            list2 = list2.next;
            curr = curr.next;
        }

        return dummyNode.next;
    }
}
```

---

# Merge K Sorted Lists (Priority Queue)

### Complexity

* Time : **O(N log K)**
* Space : **O(K)**

where

* N = total nodes
* K = number of lists

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<ListNode> pq = new PriorityQueue<>((a,b)->{
            return a.val - b.val;
        });

        for(int i=0; i<lists.length; i++){
            if(lists[i] != null){
                pq.add(lists[i]);
            }
        }

        ListNode dummyNode = new ListNode(-1);
        ListNode curr = dummyNode;

        while(!pq.isEmpty()){
            ListNode top = pq.poll();

            curr.next = top;

            if(top.next != null){
                pq.add(top.next);
            }

            curr = curr.next;
        }

        return dummyNode.next;
    }
}
```

---

# Sort List (Merge Sort on Linked List)

### Complexity

* Time : **O(n log n)**
* Space :

  * Recursive stack **O(log n)**
  * Extra merge space **O(1)**

```java
class Solution {

    public ListNode merge(ListNode list1, ListNode list2) {

        ListNode dummyNode = new ListNode(-1);
        ListNode curr = dummyNode;

        while(list1 != null && list2 != null){

            if(list1.val < list2.val){
                curr.next = list1;
                list1 = list1.next;
            }else{
                curr.next = list2;
                list2 = list2.next;
            }

            curr = curr.next;
        }

        if(list1 != null){
            curr.next = list1;
        }

        if(list2 != null){
            curr.next = list2;
        }

        return dummyNode.next;
    }

    public ListNode getMiddle(ListNode head){

        ListNode fast = head.next;
        ListNode slow = head;

        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
        }

        return slow;
    }

    public ListNode sortList(ListNode head) {

        if(head == null || head.next == null){
            return head;
        }

        ListNode middle = getMiddle(head);

        ListNode left = head;
        ListNode right = middle.next;

        middle.next = null;

        return merge(sortList(left), sortList(right));
    }
}
```

---

# Recognition Cues

Use these patterns when you see:

* Merge two sorted linked lists
* Merge K sorted linked lists
* Sort a linked list in O(n log n)
* Merge sorted streams
* Multiple sorted sequences

---

# Interview Reminder

✅ Dummy node

✅ Always move `curr`

```java
curr = curr.next;
```

✅ Attach remaining list

```java
curr.next = list1;
```

or

```java
curr.next = list2;
```

✅ Merge Sort Steps

```
Find Middle
Split
Sort Left
Sort Right
Merge
```
