# Linked List Merge Patterns

This note covers three related linked list problems:

1. Merge Two Sorted Lists
2. Merge K Sorted Lists
3. Sort List (Merge Sort)

---

# 21. Merge Two Sorted Lists

## Pattern

* Linked List
* Two Pointers
* Dummy Node
* Merge

## Recognition

Use this pattern when:

* Two sorted linked lists are given.
* Need to merge while preserving sorted order.
* Asked to do it in-place.

---

## Core Idea

Maintain a dummy node.

Compare the current nodes of both lists.

Attach the smaller node to the merged list.

**Always move `curr` after attaching a node.**

---

## High Level Algorithm

1. Create dummy node.
2. Maintain `curr`.
3. Compare both lists.
4. Attach smaller node.
5. Move the chosen list.
6. **Move `curr`.**
7. Attach remaining list.

---

## Reusable Snippet

### Dummy Node

```java
ListNode dummyNode = new ListNode(-1);
ListNode curr = dummyNode;
```

---

### Merge Loop

```java
while(list1 != null && list2 != null){

    if(list1.val < list2.val){
        curr.next = list1;
        list1 = list1.next;
    }else{
        curr.next = list2;
        list2 = list2.next;
    }

    // IMPORTANT
    curr = curr.next;
}
```

---

### Attach Remaining Nodes

```java
while(list1 != null){
    curr.next = list1;
    list1 = list1.next;

    // DON'T FORGET
    curr = curr.next;
}

while(list2 != null){
    curr.next = list2;
    list2 = list2.next;

    // DON'T FORGET
    curr = curr.next;
}
```

---

## Complete Solution

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        // Dummy node simplifies edge cases
        ListNode dummyNode = new ListNode(-1);
        ListNode curr = dummyNode;

        while(list1 != null && list2 != null){

            // Attach smaller node
            if(list1.val < list2.val){
                curr.next = list1;
                list1 = list1.next;
            }else{
                curr.next = list2;
                list2 = list2.next;
            }

            // VERY IMPORTANT
            // Forgot this once in interview practice
            curr = curr.next;
        }

        while(list1 != null){
            curr.next = list1;
            list1 = list1.next;

            // Move curr
            curr = curr.next;
        }

        while(list2 != null){
            curr.next = list2;
            list2 = list2.next;

            // Move curr
            curr = curr.next;
        }

        return dummyNode.next;
    }
}
```

---

## Complexity

Time : **O(n + m)**

Space : **O(1)**

---

# 23. Merge K Sorted Lists

## Pattern

* Heap
* Priority Queue
* Linked List
* K-way Merge

---

## Recognition

Use Priority Queue when:

* Multiple sorted lists
* Need smallest element repeatedly

---

## Core Idea

Push the head of every list.

Repeatedly remove the smallest node.

Attach it.

Insert its next node.

Repeat until heap becomes empty.

---

## Reusable Snippets

### Priority Queue

```java
PriorityQueue<ListNode> pq = new PriorityQueue<>((a,b)->{
    return a.val - b.val;
});
```

---

### Push Initial Nodes

```java
for(int i=0; i<lists.length; i++){
    if(lists[i] != null){
        pq.add(lists[i]);
    }
}
```

---

### Heap Processing

```java
while(!pq.isEmpty()){

    ListNode top = pq.poll();

    curr.next = top;

    if(top.next != null){
        pq.add(top.next);
    }

    // DON'T FORGET
    curr = curr.next;
}
```

---

## Complete Solution

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

            // Attach smallest node
            curr.next = top;

            // Push next node from same list
            if(top.next != null){
                pq.add(top.next);
            }

            // VERY IMPORTANT
            // Easy to forget
            curr = curr.next;
        }

        return dummyNode.next;
    }
}
```

---

## Complexity

Time : **O(N log K)**

Space : **O(K)**

where

* N = total nodes
* K = number of lists

---

# 148. Sort List

## Pattern

* Merge Sort
* Linked List
* Slow Fast Pointer

---

## Recognition

Whenever asked

* Sort linked list
* O(n log n)
* Constant extra space

Think:

**Merge Sort**

---

## Core Idea

1. Find middle.
2. Split list.
3. Sort left.
4. Sort right.
5. Merge both.

---

## Reusable Snippets

### Find Middle

```java
public ListNode getMiddle(ListNode head){

    ListNode fast = head.next;
    ListNode slow = head;

    while(fast != null && fast.next != null){
        fast = fast.next.next;
        slow = slow.next;
    }

    return slow;
}
```

---

### Split

```java
ListNode middle = getMiddle(head);

ListNode left = head;
ListNode right = middle.next;

middle.next = null;
```

---

### Merge

```java
return merge(sortList(left), sortList(right));
```

---

## Complete Solution

```java
class Solution {

    public ListNode merge(ListNode list1, ListNode list2){

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

            // VERY IMPORTANT
            // Forgot this before
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

    public ListNode sortList(ListNode head){

        if(head == null || head.next == null){
            return head;
        }

        ListNode middle = getMiddle(head);

        ListNode left = head;
        ListNode right = middle.next;

        // Break the list into two halves
        middle.next = null;

        return merge(sortList(left), sortList(right));
    }
}
```

---

## Complexity

Time : **O(n log n)**

Space : **O(log n)** (Recursion)

---

# Interview Reminders

## Biggest Mistake I Made

I forgot

```java
curr = curr.next;
```

in all three linked list problems.

Whenever I write

```java
curr.next = someNode;
```

I must immediately think

```java
curr = curr.next;
```

Otherwise:

* The merged list is built incorrectly.
* `curr.next` keeps getting overwritten.
* Debugging becomes difficult because the list structure looks partially correct.

---

# Linked List Checklist

* Use a dummy node when constructing a new list.
* Always move `curr` after attaching a node.
* Return `dummy.next`.
* Handle `null` lists.
* Save pointers before breaking links.
* For merge sort:

  * Find middle
  * Split
  * Sort left
  * Sort right
  * Merge

---

# Related Problems

* Merge Two Sorted Lists
* Merge K Sorted Lists
* Sort List
* Merge Intervals (similar merge concept)
* Merge Sorted Array
* Merge BSTs (variation)
* External Merge Sort
