# Gitbook 3: (2025-05-28)

Included file: by-date/2025/05/28-gitbook-3:_assets/istockphoto-2149038061-612x612-projects.webp

## README.md

---
icon: hand-wave
cover: >-
  https://www.microsoft.com/en-us/research/wp-content/uploads/2018/08/01_MSR_SIGCOMM_Data_Network_1400x788.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Welcome to my musing's page

On this page I keep things such as Proof-of-Concepts and Insights as we attempt to continue the perilous journey of software design. And mind you with the AI-hype upon us, many things will be tried to get a feel for and an understandig of the limits.

I most like to take things which I am wondering about and play with them, these findings either end up in a folder on one of the many machines I have, but recently I have started to do things such a T.I.L ( Things In Learned - one such example of this can be found here ( [https://github.com/fnchooft/TIL](https://github.com/fnchooft/TIL) ).



Also we are trying to use GitBook for some of our Projects at work - and you cannot implement what you do not understand, so here we go this page uses GitBook and will contain many small PoC's.


## getting-started/poc-space/README.md

---
description: Space to save Proof-of-Concepts
---

# PoC-space

Some Proof-of-Concepts will be added here.

By keep the folder self-contained it should be relatively easy to add content.



## getting-started/poc-space/examples.md

---
description: On this page we want to experiment on how to document examples in general.
---

# Examples

For instance, we have many examples in many languages, and it would be great if we could document it such a way that our engineers only have to make some minor adjustments to publish it on GitBook.

Lets see where that journey takes us.

```erlang
% Fibonacci numbers
% The Fibonacci sequence is given by 0, 1, 1, 2, 3, 5, … where subsequent values 
% are given by adding the two previous values in the sequence.
% Give a recursive definition of the function fib/1 computing the Fibonacci numbers
% and give a step-by-step evaluation of fib(4).
%
% Usage:
% 1> fibonacci:fib(5).
% 1> 8

-module(fibonacci).
-export([fib/1]).

fib(N) -> fib_iter(N, 0, 1).

fib_iter(0, Result, _Next) -> Result;
fib_iter(Iter, Result, Next) ->
fib_iter(Iter-1, Next, Result+Next).
```

This example was taken for [https://gist.github.com/chrisdoc/8169e14cb1ae3b6a64b2f35753a0979f](https://gist.github.com/chrisdoc/8169e14cb1ae3b6a64b2f35753a0979f)


