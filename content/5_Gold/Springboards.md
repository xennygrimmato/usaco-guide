---
id: springboards
title: "Max Suffix Query with Insertions Only"
author: Benjamin Qi
prerequisites: 
 - Silver - More with Maps & Sets
 - Silver - Amortized Analysis
description: "Solving USACO Gold - Springboards."
---

To solve USACO Gold [Springboards](http://www.usaco.org/index.php?page=viewproblem2&cpid=995), we need a data structure that supports operations similar to the following:

  1. Add a pair $(a,b)$.
  2. For any $x$, query the maximum value of $b$ over all pairs satisfying $a\ge x$.

This can be solved with a **segment tree** (see "Point Update Range Sum"), but a simpler option is to use a map. We rely on the fact
that if there exist pairs $(a,b)$ and $(c,d)$ in the map such that $a\le c$ and $b\le d$, we can simply ignore $(a,b)$ (and the answers to future queries will not be affected). So at every point in time, the pairs $(a_1,b_1),(a_2,b_2),\ldots,(a_k,b_k)$ that we store in the map will satisfy $a_1 < a_2 < \cdots < a_k$ and $b_1 > b_2 > \cdots > b_k$. 

  - Querying for a certain $x$ can be done with a single `lower_bound` operation, as we just want the minimum $i$ such that $a_i\ge x$.
  - When adding a pair $(a',b')$, first check if there exists $(a,b)$ already in the map such that $a\ge a', b\ge b'$. 
    - If so, then do nothing.
    - Otherwise, insert $(a',b')$ into the map and repeatedly delete pairs $(a,b)$ such that $a\le a', b\le b'$ from the map until none remain.

If there are $N$ insertions, then each query takes $O(\log N)$ time and adding a pair takes $O(\log N)$ time amortized.

```cpp
#define f first
#define s second
#define lb lower_bound

map<int,ll> m;
void ins(int a, ll b) {
	auto it = m.lb(a); if (it != end(m) && it->s >= b) return;
	it = m.insert(it,{a,b}); it->s = b;
	while (it != begin(m) && prev(it)->s <= b) m.erase(prev(it));
}
ll query(int x) { auto it = m.lb(x); 
	return it == end(m) ? 0 : it->s; } 
// it = end(m) means that no pair satisfies a >= x
```

## Problems

(easier examples?)

<problems-list>
    <problem name="Karen & Cards" cf="contest/815/problem/D" difficulty="Very Hard" tags={[]}>
      - For each $a$ from $p\to 1$, calculate the number of possible cards with that value of $a$.
    </problem>
    <problem name="GP of Korea 19 - Interesting Drug" cf="gym/102059/problem/K" difficulty="Very Hard" tags={[]}>
      - Genfuncs not required but possibly helpful
    </problem>
 </problems-list>