---
title: 'Local search'
date: 2026-03-19T19:04:21+01:00
draft: false
tags: ["approximation algorithms", "algorithms"]
math: true
---


Local search is a simple approximation algorithm, which doesn't necessarirly lead to the best result, instead it has a tendency to get stuck in a local minimum. Still if combined with other techniques like for example use of heuristics it can cheaply with low overhead improve the quality of the found solution.



pseudocode:
```
function local_search:
INPUT: graph of the state G = (V,E), cost function f
OUTPUT: found state

s <- generate_initial_state()

while (true): // we might want iteration amount limit in case the state space is infinite and the optimum unreachable
    s' <- s
    for n in get_neighbors(s):
        if f(n) < f(s'):
            s' <- n
    if (s' /= s):
        s <- s'
    else:
        break
return s
```

This pseudocode was written for a graph but can easily be generalized to any general solution space. Simply instead of looking at neighbours of a state we at states we can possibly "go to".
