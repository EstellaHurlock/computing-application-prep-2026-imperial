cd "/Users/estella/git-practice/imperial-app-prep/computing-application-prep-2026-imperial"

mkdir -p docs algorithms maths/evidence

# Week -1 summary 
cat > docs/WEEK-1-README.md <<'EOF'
# Week −1 Summary (2025-09-16 → 2025-09-21)

**Focus:** get set up, refresh core linear algebra, start discrete maths, and touch core DSA.

## Highlights
- Installed **Python 3.13**, VS Code, Git; verified terminal setup, ran first scripts.
- Linear Algebra (Coursera/Imperial):  
  - **Module 1:** vectors, parameter spaces, simultaneous equations, vector ops.  
  - **Module 2:** modulus & inner product; cosine & dot; projection; changing basis; span & linear independence.  
  - **Module 3:** matrices & simultaneous equations; transformation composition; **Gaussian elimination**; inverses & determinants.
- Discrete Mathematics (MIT 6.1200 OCW):  
  - **Lecture 1:** predicates, sets, proofs (implication, truth tables).  
  - **Started Lecture 2:** contradiction & induction.
- Data Structures & Algorithms (Python):  
  - Build familiarity of **arrays vs linked lists**, **stack/queue**, **binary tree traversals**, **BFS**, **merge/quick sort** (see `algorithms/dsa_playground.py`).

## Produced this week
- `algorithms/dsa_playground.py` – tiny, runnable snippets for core DSA.
- Course exercises worked by hand (Gaussian elimination, transformations).
- Notes were kept on paper; plan to scan to `maths/evidence/`.

**Time:** ~20–25 hours (setup + study + coding).
EOF

# Append a dated entry to STUDY_LOG.md
cat >> STUDY_LOG.md <<'EOF'

## 2025-09-19 (Week −1 snapshot)
**Focus:** setup + linear algebra modules 1–3; MIT discrete maths L1; DSA basics.

**Topics covered**
- Vectors & spans; inner product & projection; change of basis; Gaussian elimination; inverses/determinants.
- Predicates/sets/proofs (implication, truth tables).
- Arrays vs linked lists; stack/queue; tree traversals; BFS; merge/quick sort.

**Work produced**
- Handwritten notes to scan into `maths/evidence/`.

**ChatGPT worked through coding examples**
- `algorithms/dsa_playground.py` runnable examples.

**Time:** ~4–6 hours (day total)
EOF

# DSA playground (runnable)
cat > algorithms/dsa_playground.py <<'EOF'
"""
DSA Playground — small, runnable Python snippets for core structures/algos, Building familiarity.

Run:
  python3 algorithms/dsa_playground.py
"""

from collections import deque

# 1) Linked list --------------------------------------------------------------

class Node:
    def __init__(self, val, nxt=None):
        self.val, self.next = val, nxt

def insert_head(head, val):
    return Node(val, head)

def ll_find(head, target):
    cur = head
    while cur:
        if cur.val == target:
            return True
        cur = cur.next
    return False

# 2) Stack & Queue ------------------------------------------------------------

class Stack:
    def __init__(self):
        self._a = []
    def push(self, x): self._a.append(x)
    def pop(self):     return self._a.pop()
    def __repr__(self): return f"Stack({self._a})"

class Queue:
    def __init__(self):
        self._q = deque()
    def enqueue(self, x): self._q.append(x)
    def dequeue(self):    return self._q.popleft()
    def __repr__(self):   return f"Queue({list(self._q)})"

# 3) Binary tree + traversals + BFS ------------------------------------------

class T:
    def __init__(self, val, left=None, right=None):
        self.val, self.left, self.right = val, left, right

def inorder(r):
    return inorder(r.left) + [r.val] + inorder(r.right) if r else []

def preorder(r):
    return [r.val] + preorder(r.left) + preorder(r.right) if r else []

def postorder(r):
    return postorder(r.left) + postorder(r.right) + [r.val] if r else []

def tree_bfs(r):
    if not r: return []
    q, out = deque([r]), []
    while q:
        n = q.popleft()
        out.append(n.val)
        if n.left:  q.append(n.left)
        if n.right: q.append(n.right)
    return out

# 4) Graph BFS/DFS (adjacency list) ------------------------------------------

def graph_bfs(adj, start):
    q, seen, order = deque([start]), {start}, []
    while q:
        u = q.popleft()
        order.append(u)
        for v in adj.get(u, []):
            if v not in seen:
                seen.add(v); q.append(v)
    return order

def graph_dfs(adj, start):
    order, seen = [], set()
    def go(u):
        seen.add(u); order.append(u)
        for v in adj.get(u, []):
            if v not in seen: go(v)
    go(start)
    return order

# 5) Sorting: merge vs quick --------------------------------------------------

def mergesort(a):
    if len(a) <= 1: return a
    m = len(a)//2
    L, R = mergesort(a[:m]), mergesort(a[m:])
    i = j = 0; out = []
    while i < len(L) and j < len(R):
        if L[i] <= R[j]:
            out.append(L[i]); i += 1
        else:
            out.append(R[j]); j += 1
    out.extend(L[i:]); out.extend(R[j:])
    return out

def quicksort(a):
    if len(a) <= 1: return a
    pivot = a[len(a)//2]
    lt = [x for x in a if x < pivot]
    eq = [x for x in a if x == pivot]
    gt = [x for x in a if x > pivot]
    return quicksort(lt) + eq + quicksort(gt)

# Demo runner -----------------------------------------------------------------

def _demo():
    print("\n[LINKED LIST]")
    head = None
    for v in [1,2,3][::-1]:
        head = insert_head(head, v)
    print("find 2?", ll_find(head, 2), "| find 99?", ll_find(head, 99))

    print("\n[STACK & QUEUE]")
    s = Stack(); s.push("A"); s.push("B"); s.push("C")
    print("stack:", s, "| pop ->", s.pop(), "| now:", s)
    q = Queue(); q.enqueue("A"); q.enqueue("B"); q.enqueue("C")
    print("queue:", q, "| dequeue ->", q.dequeue(), "| now:", q)

    print("\n[BINARY TREE]")
    r = T(2, T(1), T(3))
    print("inorder  :", inorder(r))
    print("preorder :", preorder(r))
    print("postorder:", postorder(r))
    print("BFS      :", tree_bfs(r))

    print("\n[GRAPH BFS/DFS]")
    adj = {"A":["B","C"], "B":["A","D","C"], "C":["A","B"], "D":["B"]}
    print("BFS A:", graph_bfs(adj, "A"))
    print("DFS A:", graph_dfs(adj, "A"))

    print("\n[SORTING]")
    data = [5,1,4,2,8,0,2]
    print("orig:", data)
    print("merge:", mergesort(list(data)))
    print("quick:", quicksort(list(data)))

if __name__ == "__main__":
    _demo()
EOF
