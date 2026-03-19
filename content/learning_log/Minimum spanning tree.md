---
title: 'Minimum Spanning Tree'
date: 2026-03-18T16:25:43+01:00
draft: false
tags: ["graph theory", "algorithms"]
math: true
---

Here we're going to cover what are Minimum Spanning Trees (MSTs), methods for finding them and use cases.


## What is a tree?
In graph theory we call graph $T=(V,E)$ a tree if it is connected and it doesn't have any cycles.

>[!info] Forrest
We call a graph with no cycles a forrest (contrary to the tree it doesn't need to be connected). In a forrest each **connected component** is a tree.

### Examples of trees:
**Binary tree**
![bin_tree](/images/bin_tree.svg)

**General tree**
![gen_tree](/images/gen_tr.svg)

### Properties of trees
- $|E|=|V|-1$
- removing any edge from it disconnects it
- a forrest with $k$ connected components and $n$ verticies has $|E|=n-k$ edges



## What is a spanning tree?
Spanning tree of a connected graph $G=(V,E)$ is a tree $ST=(V,F)$, where $F\subseteq E$ such that for every $v,w\in V$ there exists a path from $v$ to $w$ along the edges of $F$.

### Example of a spanning tree
![graph_ST](/images/graph_ST.svg)


## Minimum spanning tree
Now we're ready to cover minimum spanning trees (MSTs). 

A MST is a spanning tree of a weighted graph $G$, $MST=(V,F),w:F\to \mathbb{R}$ such that $\sum_{e\in F}w(e)=min$ meaning the total sum of weights of it's edges is minimal.

>[!warning]
There can be more than 1 MSTs of a graph.


>[!info] MST of an unweighted graph
In an unweighted graph we can treat each edge as having equal weight, because of that every spanning tree of an unweighted graph is an MST.

### Example
![MST](/images/MST.svg)


### Why?
Why would we need such a concept as a minimum spanning tree? Well as it turns out MSTs have a lot of use cases both intuitive and unituitive. Whenever you need a system to be connected but creating connections is resource intensive MSTs are your solution. Because of that they are used in:
- road network design
- computer connection
- electrical grid design


On top of that because trees and especially MSTs have such a clear structure there's a lot of graph theoretical theorems which lead to development of useful algorithms. Such as:
- Christofides algorithm - for approximating a solution to TSP
- segmentation and skeletonization in computer vision and image processing


### How?
So given a graph how do we get it's MST? There are two primary solutions:
- Prim's Algorithm, complexity $O(|V|^2)$ or $O((|E|+|V|)log|V|)$ depending on implementation
- Kruskal's Algorithm, complexity $O(|E|log|E|)$ or $O(|E|log|V|)$ depending on implementation

Both have their pros and cons. But the primary one is that Prim's algorithm is better for dense graphs, while Kruskal's is better for sparse graphs.

#### Prim's algorithm

1. Begin at any $v$ of $G$
2. Select an edge of minimum weight incident with $v$
3. Select an edge of minimum weight having exactly one of it's verticies incident with an edge already selected - don't connect two already selected verticies
4. Repeat 3 until n-1 edges have been selected

You start at a single vertex and slowly build the tree from this single vertex continuously choosing the edge with minimal weight.

Pseudocode: (O((E+V)log V) version)
```
function prim(G,w):
	Input: a Connected undirected graph G = (V,E) with edge weights we
	Output: A minimum spanning tree defined by the array prev
	
	for u in V:
		cost[u] <- inf
		prev[u] <- NIL
	explored <- empty set
	unexplored <- set(V)
	
	v_start <- random_choice(V)
	cost[v_start] = 0
	
	Q <- priority queue
	Q.push((v_start, cost[v_start]))
	
	edges <- empty list
	n <- |V|
	
	while length(edges) < n-1:
		(current_v, _) <- Q.pop() // pop vertex with minimum cost
		if current_v in explored:
			continue
		explored.add(current_v)
		unexplored.remove(current_v)
		
		if current_v /= v_start:
			edges.append((prev[current_v], current_v))
		
		for neighbor in neighbors(current_v):
			if neighbor in unexplored and weight(current_v, neighbor) < cost[neighbor]:
				cost[neighbor] <- weight(current_v, neighbor)
				prev[neighbor] <- current_v
				Q.push((neighbor, cost[neighbor]))
	return prev, edges
```

#### Kruskal's algorithm
1. Sort all edges by weight
2. Initialize a forest of single vertex trees
3. Process edges in sorted order: for each edge (u,v)
	1. if adding (u,v) cretes a cycle skip it
	2. else add it
4. Continue until you have |V| - 1 edges in MST

Pseudocode:
```

function Kruskal(G,w):
	Input: a graph G =(V,E) with edge weights w(e)
	Output: a Minimal spanning tree of the graph G
	
	for u in V:
		parent[u] <- u // every node is in it's own tree
		rank[u] <- 0 // every tree has depth 0
	
	function find(u): // find the representative of the tree with vertex v and compress path
		if parent[u] /= u:
			parent[u] <- find(parent[u])
		return parent[u]
	
	
	function union(u, v): // combine two trees whose representatives are u and v
		ru <- find(u)
		rv <- find(v)
		if ru = rv:
			return
		
		if rank[ru] < rank[rv]:
			parent[ru] <- rv
		else if rank[rv] < rank[ru]:
			parent[rv] <- ru
		else:
			parent[ru] <- rv
			rank[rv] <- rank[rv] + 1
	
	sorted_edges <- sort E by w(e) in ascending order
	
	edges <- empty list
	for (u,v) in sorted_edges:
		if find(u) /= find(v):
			union(u,v)
			edges.append((u,v))
	return edges
```