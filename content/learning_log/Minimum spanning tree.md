---
title: 'Minimum Spanning Tree'
date: 2026-03-18T16:25:43+01:00
draft: true
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
- Prim's Algorithm
- Kruskal's Algorithm

Both have their pros and cons.

