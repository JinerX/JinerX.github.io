---
date: '2026-03-04T17:54:13+01:00'
draft: false
title: 'Algorithm Complexity Basics'
tags: ["algorithm", "university", "Theoretical basics of informatics"]
---


Here we're going to consider the basics of complexity analysis - a critical part of algorithm analysis.

## Motivation
When running algorithms we can see that some programs end almost  immediately and some take a long time. Complexity analysis aims to formalize this notion and give us tools to predict how long a program is going to run. It does so by looking at how much does the time for a program to terminate increase based on the increase in the size of the input. We do it by creating functions (mathematical) based on the algorithms which take in as input the size and return the "time". 


## Common notations
Some notations used during complexity analysis, formal definition, limit definition:
- Big $O$ - $f(n) = O(g(n)) \iff \exists\ C>0\ \exists\ n_0\in \mathbb{N}\ \forall\ n>n_0\ |f(n)| \leq |g(n)|\iff lim_{n\to \infty}\frac{|f(n)|}{|g(n)|} < \infty$
- Big $\Omega$ - $f(n) = \Omega(g(n)) \iff \exists\ C>0\ \exists\ n_0\in \mathbb{N}\ \forall\ n>n_0\ |f(n)| \geq |g(n)|\iff lim_{n\to \infty}\frac{|f(n)|}{|g(n)|} > 0$
- Big $\Theta$ - $f(n) = \Theta(g(n)) \iff \exists\ C_1, C_2>0\ \exists\ n_0\in \mathbb{N}\ \forall\ n>n_0\ C_1|g(n)| \leq |f(n)| \leq C_2|g(n)|\iff lim_{n\to \infty}\frac{|f(n)|}{|g(n)|} = c,\ c>0$
- Small $o$ - $f(n) = o(g(n)) \iff \forall\ C>0\ \exists\ n_0\in \mathbb{N}\ \forall\ n>n_0\ |f(n)| < C|g(n)|\iff lim_{n\to \infty}\frac{|f(n)|}{|g(n)|} = 0$
- Small $\omega$ - $f(n) = \omega(g(n)) \iff \forall\ C>0\ \exists\ n_0\in \mathbb{N}\ \forall\ n>n_0\ |f(n)| > C|g(n)|\iff lim_{n\to \infty}\frac{|f(n)|}{|g(n)|} = \infty$


### Intuitively:
- f(n) = O(g(n)) means that in infinity f(n) increases **no slower** than some constant times g(n)
- f(n) = $\Omega(g(n))$ means that in infinity f(n) increases **no faster** than some constant times g(n)
- f(n) = $\Theta(g(n))$ means that f(n) = O(g(n)) and f(n) = $\Omega(g(n))$
- f(n) = o(g(n)) means that in infinity f(n) increases **strictly slower** than g(n)
- f(n) = $\omega(g(n))$ means that in infinity f(n) increases **strictly faster** than g(n)

### some properties
1. $f(n) = O(g(n)) \iff g(n) = \Omega(f(n))$
2. $f(n) = o(g(n)) \iff g(n) = \omega(f(n))$
3. $f(n) = \Theta(g(n)) \iff f(n)= O(g(n)) \land f(n) = \Omega(g(n))$
4. Transitivity if $f(n) = O(g(n))$ and $g(n) = O(h(n))$ then $f(n) = O(h(n))$, same for other notations
5. closure under addition if $f = O(g)$ and $h = O(g)$ then $f + h = O(g)$
6. multiplication rule: if $f = O(g)$ and $h = O(k)$ then $fh = O(gk)$ 




## Simple examples
- $f(n) = n^2, g(n) = n^3$ then $f(n) = o(g(n)), g(n) = \omega(f(n))$
- $f(n) = n^2, g(n) = \frac{1}{2}n^2$ then $f(n) = \Theta(g(n))$
- $f(n) = n^2 + 290n + 1902, g(n) = n^2$ then $f = \Theta(g)$
- $f(n) = log(n), g(n)=n^{0.001}$ then $f=O(g)$
