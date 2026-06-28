# LC 138 - Copy List with Random Pointer

**Pattern:** Linked List

**Algorithm:** Interweaving (Weaving) Linked List

## Idea

1. Insert copied nodes after every original node.
2. Assign random pointers.
3. Separate the two lists.

---

## Step 1: Interweave Copy Nodes

```text
1 -> 2 -> 3

↓

1 -> 1' -> 2 -> 2' -> 3 -> 3'
```

```java
private void copyList(Node head){
    Node curr = head;

    while(curr != null){
        Node copy = new Node(curr.val);

        copy.next = curr.next;
        curr.next = copy;

        curr = copy.next;
    }
}
```

---

## Step 2: Copy Random Pointers

Since every copied node is immediately after its original,

```text
copy.random = original.random.next
```

```java
private void copyRandomPointers(Node head){
    Node curr = head;

    while(curr != null){
        if(curr.random != null){
            curr.next.random = curr.random.next;
        }

        curr = curr.next.next;
    }
}
```

---

## Step 3: Separate Both Lists

Restore the original list while extracting the copied list.

```java
private Node separateLists(Node head){
    Node copyHead = head.next;

    Node original = head;
    Node copy = copyHead;

    while(original != null){
        original.next = copy.next;
        original = original.next;

        if(original != null){
            copy.next = original.next;
            copy = copy.next;
        }
    }

    return copyHead;
}
```

---

## Main

```java
public Node copyRandomList(Node head) {
    if(head == null){
        return null;
    }

    copyList(head);
    copyRandomPointers(head);

    return separateLists(head);
}
```

---

## Complexity

* **Time:** O(n)
* **Space:** O(1)

---

## Interview Reminders

* Interweave copied nodes with originals.
* `copy.random = original.random.next`
* Restore the original list while extracting the copied list.
* No HashMap required.
