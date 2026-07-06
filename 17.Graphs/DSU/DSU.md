# Disjoint Set Union (DSU) / Union Find

## What is DSU?

**Disjoint Set Union (DSU)**, also known as **Union Find**, is a data structure used to efficiently maintain a collection of **disjoint (non-overlapping) sets**.

It supports two main operations:

1. **Find** → Determine which set (component) a node belongs to.
2. **Union (Merge)** → Merge two different sets into one.

DSU is commonly used to solve **connectivity problems** in graphs.

---

## When to Think of DSU?

Use DSU whenever the problem involves:

* Connected Components
* Dynamic Connectivity
* Merging Groups
* Cycle Detection in Undirected Graphs
* Equivalence Relations
* Kruskal's Minimum Spanning Tree

Typical keywords:

* "Connect"
* "Merge"
* "Friend Circles"
* "Network"
* "Groups"
* "Equivalent"
* "Connected Components"

---

# Representation

Suppose we have 5 nodes.

Initially, every node is its own component.

```
0    1    2    3    4
```

Parent array:

```java
parent = [0,1,2,3,4]
```

Meaning:

```
parent[0] = 0
parent[1] = 1
parent[2] = 2
parent[3] = 3
parent[4] = 4
```

Each node is its own leader.

---

# The `find()` Operation

## Purpose

Returns the **leader (representative/root)** of the component containing a node.

Two nodes belong to the same component **if and only if** they have the same leader.

---

## Code

```java
public int findPar(int u){
    if(parent[u] == u){
        return u;
    }
    return parent[u] = findPar(parent[u]);
}
```

---

## How `find()` Works

Suppose the parent array represents:

```
0
|
1
|
2
|
3
```

```
parent

0 -> 0
1 -> 0
2 -> 1
3 -> 2
```

Calling

```java
findPar(3)
```

Traversal:

```
3 → 2 → 1 → 0
```

Since `0` is its own parent,

```
Leader = 0
```

So,

```java
findPar(3) = 0
```

Similarly,

```java
findPar(2) = 0
findPar(1) = 0
findPar(0) = 0
```

All four nodes belong to the same component.

---

# Why is `find()` Important?

Whenever we want to merge two nodes:

```java
u
v
```

we **never compare the nodes directly**.

Instead,

```java
int p1 = findPar(u);
int p2 = findPar(v);
```

If

```java
p1 == p2
```

both nodes already belong to the same component.

Otherwise,

```java
p1 != p2
```

the components are different and can be merged.

---

# Time Complexity

| Operation                         | Complexity     |
| --------------------------------- | -------------- |
| `find()` without Path Compression | O(N)           |
| `find()` with Path Compression    | O(α(N)) ≈ O(1) |

where **α(N)** is the **Inverse Ackermann Function**, which grows extremely slowly (less than 5 for any practical input size).

Therefore, in interviews, it is acceptable to say:

* **Amortized O(α(N))**
* **Approximately O(1)**

---

# Key Points to Remember

* Every component has exactly one **leader (root)**.
* `find()` returns the leader of a node.
* Nodes with the same leader belong to the same connected component.
* Always call `find()` before performing a union.
* Path Compression makes future `find()` operations nearly constant time.

---

# Problem-Specific Core Logic

## 1. Number of Provinces (LC 547)

### Core Logic

* Treat every city as an individual component.
* Traverse only the lower (or upper) triangle of the adjacency matrix.
* If two cities are directly connected, merge their components.
* Count how many nodes remain as leaders (`parent[i] == i`).

**Complexity**

* Time: `O(N² · α(N)) ≈ O(N²)`
* Space: `O(N)`

---

## 2. Number of Operations to Make Network Connected (LC 1319)

### Core Logic

* Initially, every computer is its own component.
* For every cable:

  * Different leaders → merge components.
  * Same leader → this cable is an extra (redundant) edge.
* Let:

  * `components` = number of connected components.
  * `extraEdges` = redundant cables.
* Need `components - 1` cables to connect all components.
* If `extraEdges < components - 1`, return `-1`; otherwise return `components - 1`.

**Complexity**

* Time: `O(E · α(N))`
* Space: `O(N)`

---

## 3. Lexicographically Smallest Equivalent String (LC 1061)

### Core Logic

* Each character (`a`–`z`) is a node.
* Merge equivalent characters.
* Always make the **lexicographically smaller** character the parent.
* For every character in `baseStr`, replace it with its component leader.

**Complexity**

* Time: `O((|s1| + |baseStr|) · α(26)) ≈ O(|s1| + |baseStr|)`
* Space: `O(26) ≈ O(1)`
