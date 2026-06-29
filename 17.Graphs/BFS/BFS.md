# BFS Revision Notes (LeetCode)

---

# 994. Rotting Oranges

## Pattern
- Multi-Source BFS

## Problem Summary
All rotten oranges simultaneously rot adjacent fresh oranges every minute.

Find the minimum time required to rot all oranges.

## Recognition Cues
- Spread simultaneously
- Multiple starting points
- Minimum time
- Grid traversal
- BFS on matrix

## Core Idea
- Push every rotten orange into the queue initially.
- Count fresh oranges.
- Process BFS level by level.
- Each level represents one minute.
- When fresh oranges become zero, return current time.

## Initialization

```java
Queue<int[]> q = new LinkedList<>();

if(grid[i][j] == 2)
    q.add(new int[]{i, j});

if(grid[i][j] == 1)
    freshOrange++;
```

## Directions

```java
int[][] dirs = {
    {-1,0},
    {1,0},
    {0,-1},
    {0,1}
};
```

## Rot Neighbor

```java
grid[i][j] = 2;
freshOrange--;
q.add(new int[]{i, j});
```

## Algorithm

1. Count fresh oranges.
2. Push all rotten oranges.
3. Perform level-order BFS.
4. Rot adjacent fresh oranges.
5. Reduce fresh count.
6. Last completed level gives answer.

## Complexity

- Time : **O(n × m)**
- Space : **O(n × m)**

## Interview Reminder

- Multi-Source BFS.
- Initial queue contains every rotten orange.
- Mark orange rotten before pushing.
- One BFS level = one minute.

## Related Problems

- Walls and Gates
- 01 Matrix
- As Far from Land as Possible
- Escape the Spreading Fire

---

# 752. Open the Lock

## Pattern
- BFS
- State Space Search

## Problem Summary
Starting from `"0000"`, rotate one wheel at a time to reach the target while avoiding deadends.

## Recognition Cues

- Minimum moves
- Equal cost operations
- State graph
- Dead states

## Core Idea

- Each lock configuration is a graph node.
- Every node has 8 neighbors.
- Rotate one wheel forward or backward.
- BFS guarantees minimum rotations.

## State Representation

```java
String prepare(int a, int b, int c, int d){
    return a + "" + b + "" + c + "" + d;
}
```

## Initialization

```java
Queue<int[]> q = new LinkedList<>();

if(!hs.contains("0000")){
    q.add(new int[]{0,0,0,0});
    hs.add("0000");
}
```

## Neighbor Generation

For every wheel:

- Rotate +1
- Rotate -1

Wrap around:

```java
9 -> 0
0 -> 9
```

## Algorithm

1. Store deadends in HashSet.
2. Start BFS from `"0000"`.
3. Generate 8 neighbors.
4. Ignore visited/dead states.
5. First time reaching target is the answer.

## Complexity

- Time : **O(10000)**
- Space : **O(10000)**

## Interview Reminder

- Mark visited while pushing.
- Deadends are treated as visited.
- Maximum possible states = 10⁴.

## Related Problems

- Word Ladder
- Snakes and Ladders
- Sliding Puzzle

---

# 909. Snakes and Ladders

## Pattern

- BFS
- Shortest Path in Unweighted Graph

## Problem Summary

Find the minimum number of dice throws required to reach square `n²`.

Landing on a snake or ladder immediately moves to its destination.

## Recognition Cues

- Minimum moves
- Dice gives 6 choices
- Equal edge weight
- Zig-zag board numbering

## Core Idea

- Every square is a graph node.
- Explore next 1–6 positions.
- Convert square number to board coordinates.
- Apply snake/ladder before pushing.
- First time reaching last square is the answer.

## Board Index Conversion

```java
int j = next - 1;

int row = n - 1 - (j / n);

int col;

if((j / n) % 2 == 0)
    col = j % n;
else
    col = n - 1 - (j % n);
```

## BFS Initialization

```java
Queue<Integer> q = new LinkedList<>();
HashSet<Integer> vis = new HashSet<>();

q.offer(1);
vis.add(1);
```

## Algorithm

1. Start from square 1.
2. Explore next six positions.
3. Convert index to board cell.
4. Apply snake or ladder.
5. Push unseen destination.
6. BFS level gives dice throws.

## Complexity

- Time : **O(n²)**
- Space : **O(n²)**

## Interview Reminder

- Snake/Ladder is applied once after dice roll.
- Never revisit a square.
- BFS level represents number of dice throws.

## Related Problems

- Open the Lock
- Word Ladder
- Minimum Genetic Mutation

---

# BFS Recognition Cheat Sheet

Use BFS whenever you see:

- Minimum moves
- Minimum operations
- Minimum transformations
- Equal edge weights
- Shortest path in an unweighted graph
- Level-order traversal
- Spread over time
- Simultaneous expansion
- Multiple starting points

## Standard BFS Template

```java
Queue<Node> q = new LinkedList<>();
HashSet<Node> vis = new HashSet<>();

q.offer(start);
vis.add(start);

int level = 0;

while(!q.isEmpty()){

    int size = q.size();

    while(size-- > 0){

        Node curr = q.poll();

        if(goal)
            return level;

        for(Node next : neighbors){

            if(!vis.contains(next)){
                vis.add(next);
                q.offer(next);
            }
        }
    }

    level++;
}
```