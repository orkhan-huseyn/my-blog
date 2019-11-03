---
title: Understanding Dijkstra's Two Stack Algorithm
date: 2019-11-02 11:56:02
---

I wrote [an article about stacks](https://medium.com/@eauditory3/what-are-stack-based-calculators-cf2dbe249264) and how to evaluate [postix expressions](http://www.cs.csi.cuny.edu/~zelikovi/csc326/data/assignment5.htm) using a single stack. Since stacks has many applications, we know what we can do with a single stack. And know let's see what two stacks are capable of.

In Computer Science two stacks has an important application called [Two Stack PDA](https://www.quora.com/What-is-two-stack-pushdown-automata) which is beyond the scope of this article but worth to look at. Else, if you just want to evaluate an infix expression without using a third party library then this article is for you.

Let's say we have an [infix](https://en.wikipedia.org/wiki/Infix_notation) expression like this where we want to evaluate the expression with also preserving presedence:

{% asset_img an-infix-exp.png "an infix expression" %}

Now, before we move to the solution think about how you can solve it. Or if you want to directly dive into the solution then let's begin. The two stack algorithm was invented by [Edsger Dijkstra](https://en.wikipedia.org/wiki/Edsger_W._Dijkstra) is a method for evaluation fully patenthesized mathematical expressions in infix notation. As the name suggests it uses two stacks, and the algorithm is simple:

- We have two stacks: a value stack and an operator stack
- Read the expression string from left to right
- If the current symbol is a value, push it to the value stack
- If the current symbol is an operator, push it to the operator stack
- If the current symbol is left parenthesis, just ignore it
- If the current symbol is right parenthesis
  - Pop an operator from operator stack and pop two values from value stack
  - Apply the operator to that values
  - Push the result to the value stack
