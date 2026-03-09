---
title: 'Introduction to Turing Machines'
date: 2026-03-06T20:00:47+01:00
draft: false
tags: ["university", "theoretical basics of informatics", "computation theory"]
math: true
---

Here we're going to cover the very basics of what are turing machines and what they are used for. To understand this post it's best to be familiar with finite automata DFAs, NFAs, regular languages and context free grammars.

## Motivation
We as programmers want to use mathematics to study algorithms efficiently, but for that we need a formal mathematical notion of what an algorithm is. Additionally we want a way to figure out if a problem is computable - does there exist an algorithm solving a problem. Because of that we need some sort of mathematical entity where the notion of algorithms makes formal sense and which allows us to analyze those algorithms. This mathematical entity is the **Turing Machine (TM)**. 

## Definition
TM is a quadruple (Q, $q_0$, $\Sigma$, $\delta$) such that:
1. Q - a finite set of states
2. $q_0$ - initial state
3. $\Sigma$ - finite slphabet of symbols (with additional character $\sqcup$ - blank for empty spaces)
4. $\delta$ - transition function $\delta:Q\times (\Sigma\cup\\{\sqcup\\})\to (Q\cup\\{yes,no,h\\})\times(\Sigma\cup \\{\sqcup\\})\times\\{\rightarrow,\leftarrow,-\\}$

Where:
- $\sqcup$ - is a blank sumbol
- yes, no, h - are special states signifying **stopping** the machine, yes - accept, no - reject, h - return the sequence currently on the tape as output
- $\leftarrow,\rightarrow,-$ - are special symbolls signifying moving right, left, or staying in a cell when going through the machine tape

Depending on a source TM can be defined differently for examply by adding additional element signifying the tape alphabet which is the set of characters originally on the tape + blank symbol etc. Those definitions are equivalent with the one above.

The TM works on an infinite tape (depending on the source uni or bi directionally infinite). Originally on the tape there is a finite input sequence with the **head** of the machine pointing at it's first element. All fields before the first element of the input sequence and after the last are originally **blank**. While operating the machine moves the head by at most 1 space (depending on the symbol $\leftarrow, \rightarrow,-$) and has the ability to write symbols from the alphabet to the fields it's currently pointing at.

>[!info] 
The TM can have more than one tape, but here we're going to only consider turing machines with only one tape.


Field and head of TM can be visualized as such:

<img src="/images/turing_tape_copy.svg" width="700">.


## Example
Say we want to check if a word is of a form $ w=a^nb^nc^n $ for some natural number n. We want to create a TM which would accept words in this form and reject all others.

>[!info] Idea:
Idea for the algorithm is as such:
1. delete first a
2. skip all a and b until you reach last b
3. replace last b with d
4. go back to the first a
Repeat steps 1-4 until either unexpected behaviour for example wrong symbol then reject, or you run out as or bs
if you removed all as but there are still bs, or you replaced all bs but there are still as - reject, otherwise go to step 5
5. delete first d
6. skip all d and c until you reach last c
7. delete last c
8. go back to first d
Repeat steps 5-8 until unexpected behaviour - or you run out of ds or cs
if you removed all ds but there are still cs - reject, otherwise accept 



following transition function table describes such a machine:

| $\delta$ | a                          | b                     | c                            | d                          | $\sqcup$                   |
| -------- | -------------------------- | --------------------- | ---------------------------- | -------------------------- | -------------------------- |
| $q_0$    | $$q_1,\sqcup,\rightarrow$$ | no                    | no                           | $$q_6,d,-$$                | yes                        |
| $q_1$    | $$q_1,a,\rightarrow$$      | $$q_2,b,-$$           | no                           | no                         | no                         |
| $q_2$    | no                         | $$q_2,b,\rightarrow$$ | $$q_3,c,\leftarrow$$         | $$q_3,d,\leftarrow$$       | no                         |
| $q_3$    | no                         | $$q_4,d,\leftarrow$$  | no                           | no                         | no                         |
| $q_4$    | $$q_5,a,\leftarrow$$       | $$q_4,b,\leftarrow$$  | no                           | no                         | $$q_0,\sqcup,\rightarrow$$ |
| $q_5$    | $$q_5,a,\leftarrow$$       | no                    | no                           | no                         | $$q_0,\sqcup,\rightarrow$$ |
| $q_6$    | no                         | no                    | no                           | $$q_7,\sqcup,\rightarrow$$ | yes                        |
| $q_7$    | no                         | no                    | $$q_8,c,\rightarrow$$        | $$q_7,d,\rightarrow$$      | no                         |
| $q_8$    | no                         | no                    | $$q_8,c,\rightarrow$$        | no                         | $$q_9,\sqcup,\leftarrow$$  |
| $q_9$    | no                         | no                    | $$q_{10},\sqcup,\leftarrow$$ | no                         | no                         |
| $q_{10}$ | no                         | no                    | $$q_{10},c,\leftarrow$$      | $$q_{11},d,\leftarrow$$    | $$q_6,\sqcup,\rightarrow$$ |
| $q_{11}$ | no                         | no                    | no                           | $$q_{11},d,\leftarrow$$    | $$q_6,\sqcup,\rightarrow$$ |



## Configuration of a TM
Sometimes we want to represent TM at a given point in time concisely. For that we use **configurations** also **nstantaneous description**.

A configuration of a TM is a triplet: $(q,\alpha,\Beta)$ such that:
- $q $ is the current state
- $\alpha$ - is a string of symbols before the head of the TM
- $\Beta$ - is a string of symbols after the head of the TM

Such triplet contains all the information needed for the TM to continue computation.

For example for a machine in this state:
```
... ⊔ ⊔ 1 0 1 1 0 ⊔ ⊔ ...
            ^
            |
           head
```
we would have:
- $\alpha = [1,0,1]$
- $\Beta = [1,0]$


### COnfiguration transitions
We can view TM computation as a sequence of configurations.

We write:
$C_1 \vdash C_2$
when after 1 step from configuration $C_1$ we go to configuration $C_2$.

We write:
$C_1 \vdash^i C_2$
If after i steps from configuration $C_1$ we go to configuration $C_2$.

We write:
$C_1 \vdash^* C_2$
If after any number of steps from configuration $C_1$ we go to configuration $C_2$.


For a machine M and input sequence X we can notice some properties:
- $M(X) = yes \iff (q_0,\epsilon,X)\vdash^* (yes,-,-)$ - Machine M ends up accepting input sequence X if after some number of steps we reach the "yes" state
- $M(X) = Y \iff (q_0,\epsilon,X)\vdash^* (h,\alpha,\Beta)\wedge Y=\alpha\Beta$ - Machine M returns sequence Y if after some number of steps we reach state h with Y being the sequence on tape