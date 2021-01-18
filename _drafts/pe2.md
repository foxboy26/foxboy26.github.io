---
layout: post
title:  "Project Euler P2 - Even Fibonacci numbers"
date:   2021-01-16 18:00:00
categories: project_euler
tags: simple Fibonacci
---
The easiest way is probably writing a program to do this, as $4\times 10^6$ is not a big number and Fibonacci number grows expotenially. The following Python snippet does the job:

```python
a = 1
b = 1
c = a + b
s = 0
while c < 4000000:
    if c % 2 == 0:
        s += c
    a = b
    b = c
    c = a + b
print s
```

The time-complexity of this algorithm is $O(n)$. The program above only took 30ms to run on my 2017 iMac, so this is probably good enough.

## A Mathematical Approach

However, I am a bit curious on if we can do better, by finding a closed form for this summation.

Fibonacci numbers are defined recursively as $F_1=1, F_2=1$, and $F_n=F_{n-1}+F_{n-2}$ 

First let's write down the first a few Fibonacci numbers:
$$
1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, ...
$$
Notice that all the even Fibonacci numbers lie on $F_3$, ${F_6}$ , $F_9$, ... In other words, all $F_{3n}$ are even Fibonacci numbers. This result comes from a simple fact that odd+odd=even, and even+odd=odd. Now the problem becomes to find the closed form for:
$$
S_n=\sum_{i=1}^nF_{3i}
$$
Again, let's try writing down a few terms first:

| $F_n$ | 1    | 1    | 2    | 3    | 5    | 8    | 13   | 21   | 34   | 55   | 89   | 144  | 233  | 377  | 610  |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| $S_n$ |      |      | 2    |      |      | 10   |      |      | 44   |      |      | 188  |      |      | 798  |

If you look carefullly, you might notice that $2S_n= F_{3n+2} - 1$ holds for all these terms we check. Let's try to proof this!

The simplest way is probably by induction. Here I will show a way that without induction. Notice that
$$
2F_{3n} = F_{3n} + F_{3n-1} + F_{3n-2}
$$
This means
$$
2S_n=\sum_{i=1}^nF_{3i} = \sum_{i=1}^n(F_{3i} + F_{3i-1} + F_{3i-2}) = \sum_{k=1}^{3n}F_k = F_{3n+2} - 1
$$
The last step comes from a well-known result for the sum of first $k$ Fibonacci numbers [1].

Now that we have the closed-form, there are two things left, first to find the largest term that is smaller than 4,000,000, and to compute $F_{3n+2}$. 

Notice that $F_n = \frac{1}{\sqrt{5}}(\phi^n+(1-\phi))^n$ , where $\phi=(1+\sqrt{5})/2$ is the golden-ratio. Since $(1-\phi) \approx -0.618 < 1$, this $(1-\phi)^n$ term vanishes pretty quickly when $n$ becomes large. Now we have ${F_n \approx \frac{1}{\sqrt{5}}\phi^n}$, which is a pretty good approximation to compute $n$-th Fibonacci number.  Plugin $\frac{1}{\sqrt{5}}\phi^n < 4000000$ , we found the $F_{33} = 3524578$ be the largest one that is smaller than 4,000,000. Therefore, with the help of a calculator, we have
$$
S_{11} = (F_{33+2} - 1) / 2 = \fbox{4613732}.
$$
 [1] https://en.wikipedia.org/wiki/Fibonacci_number#Combinatorial_identities

