---
title: Understanding Dijkstra's Two Stack Algorithm
date: 2019-11-02 11:56:02
---

I wrote [an article about stacks](https://medium.com/@eauditory3/what-are-stack-based-calculators-cf2dbe249264) and how to evaluate postix expressions using a single stack. Since stacks has many applications, we know what we can do with a single stack. And know let's see what two stacks are capable of.

In Computer Science two stacks has an important application called [Two Stack PDA](https://www.quora.com/What-is-two-stack-pushdown-automata) which is beyond the scope of this article but worth to look at. Else, if you just want to evaluate an infix expression without using a third part library then this article is for you.

Let's say we have an infix expression like this where we want to evaluate the expression with also preserving presedence:

{% asset_img an-infix-exp.png "an infix expression" %}
