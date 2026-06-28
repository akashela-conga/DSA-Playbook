# LRU Cache (LC 146)

**Pattern:** HashMap + Doubly Linked List

**Algorithm:** Doubly Linked List + HashMap

## Recognition

* O(1) `get()`
* O(1) `put()`
* Remove least recently used item

---

## Data Structures

```text
HashMap<Key, Node>

Node
- key
- value
- prev
- next
```

The DLL stores nodes in **Most Recently Used → Least Recently Used** order.

```text
Head <-> MRU .... LRU <-> Tail
```

---

## Operations

### Insert at Head (Most Recently Used)

```java
insertAtStart(key, value);
```

### Remove Any Node

```java
removeNode(node);
```

### get(key)

```java
if(key not present)
    return -1

remove(node)
insertAtStart(node)

return value
```

Accessing a node makes it the **Most Recently Used**.

---

### put(key,value)

* Key exists

  * Remove old node
  * Insert updated node at head

* Key doesn't exist

  * Cache full

    * Remove tail (LRU)
  * Insert new node at head

---

## Complexity

* **Time:** O(1)
* **Space:** O(capacity)

---

## Interview Reminder

Whenever a node is accessed or updated, move it to the **head**.

---

# LFU Cache (LC 460)

**Pattern:** HashMap + HashMap + Doubly Linked Lists

**Algorithm:** Frequency Buckets

## Recognition

Need O(1):

* get()
* put()
* Remove **Least Frequently Used**
* If frequencies tie → remove **Least Recently Used**

---

# Core Idea

Instead of one DLL (LRU),

Maintain **one DLL per frequency**.

```text
Frequency 1

Head
 ↓
A <-> B <-> C
 ↑         ↑
MRU       LRU

Frequency 2

Head
 ↓
D <-> E

Frequency 3

Head
 ↓
F
```

Each DLL behaves exactly like an LRU list.

---

## Data Structures

### Node

```java
key
value
freq
prev
next
```

---

### keyMap

```java
HashMap<Integer, Node>
```

Find node in O(1).

---

### freqMap

```java
HashMap<Integer, DLL>
```

Maps

```text
frequency
      ↓
 Doubly Linked List
```

Example

```text
1 -> DLL(A,B,C)

2 -> DLL(D,E)

3 -> DLL(F)
```

---

### minFreq

Tracks the smallest frequency currently present.

This lets us immediately know which list to evict from.

---

# Flow

## put()

### New Node

Always starts with

```text
freq = 1
```

Insert into

```text
freqMap[1]
```

Update

```text
minFreq = 1
```

---

### Existing Node

Update value

Increase frequency

Move node to new DLL

---

### Cache Full

Evict from

```text
freqMap[minFreq]
```

Specifically,

```text
removeLast()
```

because inside the same frequency,

Tail is the **Least Recently Used**.

---

# get()

```text
Node found

↓

Increase frequency

↓

Move node

↓

Return value
```

---

# update(node)

This is the heart of the problem.

## Step 1

Remove node from current frequency list.

```java
oldList.remove(node);
```

---

## Step 2

If this was the only node having minimum frequency,

Increase

```java
minFreq++;
```

```java
if(oldFreq == minFreq && oldList.size == 0)
    minFreq++;
```

Example

Before

```text
Freq 1 : A

Freq 2 : B C

minFreq = 1
```

Access A

After

```text
Freq 1 : empty

Freq 2 : A B C

minFreq = 2
```

---

## Step 3

Increase frequency.

```java
node.freq++;
```

---

## Step 4

Insert into new frequency list.

```java
newList.addFirst(node);
```

Node becomes

**Most Recently Used** among nodes having the same frequency.

---

# Why Doubly Linked List?

Need O(1)

* Remove arbitrary node
* Insert at front
* Remove LRU from back

Singly Linked List cannot remove arbitrary nodes in O(1).

---

# Mental Model

```text
keyMap

1 → Node
2 → Node
3 → Node
```

```text
freqMap

1 →
Head
A <-> B

2 →
Head
C <-> D

3 →
Head
E
```

Every node belongs to exactly **one** frequency list.

---

# Complete Flow

```text
get(key)

↓

Find node

↓

Remove from old frequency list

↓

Increase frequency

↓

Insert into new frequency list

↓

Update minFreq if needed
```

---

# Complexity

| Operation | Time |
| --------- | ---- |
| get       | O(1) |
| put       | O(1) |

**Space:** O(capacity)

---

# Things to Remember

* `keyMap` → key → node
* `freqMap` → frequency → DLL
* New node always starts with **freq = 1**
* `minFreq` always points to the smallest frequency in cache.
* When frequencies tie, evict the **Least Recently Used** node from that frequency (`removeLast()`).
* `update(node)` does **4 things**:

  1. Remove from old DLL
  2. Update `minFreq` if needed
  3. Increase frequency
  4. Insert into new DLL

---

# Mnemonic

**LRU**

> One DLL.

**LFU**

> Many DLLs (one per frequency) + `minFreq`.
