---
title: 'Groups Abstract Algebra'
date: 2026-03-05T17:25:33+01:00
draft: false
tags: ["university", "abstract algebra"]
math: true
---

Here we consider definition of a group and some simple properties, operations, examples.


## Fromal definition

### Group
A pair $(X, +)$ is called a group if $X$ is a set, $+$ is a function $+:X\times X\to X$ and:
- operations are associative: $a + (b + c) = (a + b) + c$
- X has an identity element 0 such that $\forall x\in X$ $x + 0 = 0 + x = x$
- each element $x$ has an inverse $x^{-1}$: $x+ x^{-1} = x^{-1}+ x = 0$


### Semigrups
Semigroup is a wekaer structure than the group it does not require:
- an identity element
- inverses
It does require:
- closure
- associativity



### Abellian groups
A group $G = (X,+)$ is called abellian if the operation $+$ is commutative



## Properties
- identity element is unique
- inverses are unique
- cancelation law: if $a + b = a + c$ then $b = c$
- if $a + x = b$ then $x = a^{-1} + b$

## Examples of Groups

### Integers under addition

$$
(\mathbb{Z}, +)
$$

- identity: $0$
- inverse of $a$: $-a$

This is an **Abelian group**.

---

### Nonzero real numbers under multiplication

$$
(\mathbb{R} \setminus \{0\}, \cdot)
$$

- identity: $1$
- inverse of $a$: $1/a$

Also an **Abelian group**.

---

### Integers modulo $n$

$$
(\mathbb{Z}_n, +)
$$

Elements:

$$
\{0,1,2,\dots,n-1\}
$$

Operation: addition modulo $n$.

Example for $n=5$:

$$
3 + 4 \equiv 2 \pmod{5}
$$

---


### Matrix groups

Invertible matrices form groups under multiplication:

$$
GL(n, \mathbb{R})
$$

These groups are **not commutative**.

---

### Permutation groups

A **permutation** is a rearrangement of elements in a set.

The set of all permutations of $n$ elements forms the **symmetric group**

$$
S_n
$$

The group operation is **composition of permutations**.

Example: permutations of the set $\{1,2,3\}$.

One permutation might send:

$$
1 \rightarrow 2,\quad 2 \rightarrow 3,\quad 3 \rightarrow 1
$$

Applying two permutations one after another gives another permutation.

Properties:

- identity permutation leaves all elements unchanged
- every permutation has an inverse
- generally **not commutative**

For $n$ elements the group contains

$$
n!
$$

permutations.

Example:

$$
|S_3| = 6
$$

Permutation groups are extremely important because **many groups can be represented as permutation groups**.

---