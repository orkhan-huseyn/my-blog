---
title: Understanding Dijkstra's Two Stack Algorithm
date: 2019-11-02 11:56:02
---

## Inroduction

I wrote [an article about stacks](https://medium.com/@eauditory3/what-are-stack-based-calculators-cf2dbe249264) and how to evaluate [postix expressions](http://www.cs.csi.cuny.edu/~zelikovi/csc326/data/assignment5.htm) using a single stack. Since stacks has many applications, we know what we can do with a single stack. And now let's see what we can do with two stacks.

In Computer Science two stacks has an important application called [Two Stack PDA](https://www.quora.com/What-is-two-stack-pushdown-automata) which is beyond the scope of this article but worth to look at. Fortunately or unfortunately, we're not going to talk a lot about Computer Science here, so if you just want to evaluate a mathematical expression without using a third party library then this article is for you.

## Evaluating an expression

Let's say we have an infix expression (a mathematical expression where operators are between operands) like this where we want to evaluate the expression with also preserving presedence. How would you do it?

`( 1 + ( ( 2 + 3 ) * ( 4 * 5 ) ) )`

Now, before we move to the solution think a little bit about how you can solve it on your own. Or if you want to directly dive into the solution then let's begin.
The two stack algorithm, which was invented by [Edsger Dijkstra](https://en.wikipedia.org/wiki/Edsger_W._Dijkstra), is a method for evaluating fully patenthesized mathematical expressions. As the name suggests it uses two stacks, and the algorithm is simple:

- We have two stacks: a value stack and an operator stack
- We read the expression string from left to right
- If the current symbol is a value, push it to the value stack
- If the current symbol is an operator, push it to the operator stack
- If the current symbol is left (opening) parenthesis, just ignore it
- If the current symbol is right (closing) parenthesis
  - Pop an operator from operator stack and pop two values from value stack
  - Apply the operator to that values
  - Push the result to the value stack

To see it visually I would recommend to take a look at [this video](https://www.coursera.org/lecture/algorithms-part1/stack-and-queue-applications-optional-2Mbvz) on Coursera. If we write JavaScript code for this algorithm it would look something like this:

```javascript
function evaluate(str) {
  const ops = [];
  const vals = [];
  const arr = str.split(" ");
  for (let i = 0; i < arr.length; i++) {
    const s = arr[i];
    if (s === "(") continue;
    else if (s === "+") ops.push(s);
    else if (s === "-") ops.push(s);
    else if (s === "*") ops.push(s);
    else if (s === "/") ops.push(s);
    else if (s === ")") {
      const op = ops.pop();
      let v = vals.pop();
      if (op === "+") v = vals.pop() + v;
      else if (op === "-") v = vals.pop() - v;
      else if (op === "*") v = vals.pop() * v;
      else if (op === "/") v = vals.pop() / v;
      vals.push(v);
    } else vals.push(parseInt(s, 10));
  }
  return vals.pop();
}
```

I believe the code is clear to you since it is fairly simple algorithm, so let's test it with the expression that we considered earlier:

```javascript
evaluate("( 1 + ( ( 2 + 3 ) * ( 4 * 5 ) ) )"); // output is 101
```

Surprisingly, if you put every operator after operands, the algorithm will also output the correct answer:

```javascript
evaluate("( 1 ( ( 2 3 + ) ( 4 5 * ) * ) + )"); // output is 101
```

In this kind of expressions parentheses are redundant and removing them will give us a postfix expression (a mathematical expression where operators follow operands).
However, this expression will not be evaluated by our function since there is no any closing parenthesis to stop and make us calculate.

## Conslusion

Dijkstra is popular with his [shortest path algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) but it too was an interesting, simple and powerful algorithm that we learnt. I believe, getting familiar with a lot of such algorithms will make you a better problem solver. Since problem solving is a very important aspect of a software engineer, this kind of topics are worth to learn.

The content of this article is based on [Algorithms Part I](https://www.coursera.org/learn/algorithms-part1/) course on Coursera which is presented by Princeton University and is completely free. I strongly recommend to take a look at that course.

Thanks for reading.
