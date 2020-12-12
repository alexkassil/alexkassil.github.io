---
layout: post
title: "Counting with less Bits"
date: 2020-12-11
tags: Morris Counter Streaming Algorithms Testing
---

## Introduction

Currently I am taking a class called [Sketching Algorithms](https://www.sketchingbigdata.org/fall20). This course covers how many problems can be solved probabilistically in much less space/faster. You improve space/compute time, but instead of getting back exact answers, you get a solution that is provably approximately close to the actual solution most of the time.

### Counting

There is nothing special about a simple counter. Just a variable that you increment.


```python
class Counter:
    def __init__(self):
        self.X = 0
    def update(self):
        self.X += 1
    def query(self):
        return self.X
```
_A python class for a simple counter_

What gets more interesting is if you have a probabilistic counter. Here instead of always incrementing X, we increment X with probability of 1/2 to the power of X. Wow! And instead of returning X, we return 2 to the power of X minus 1.

```python
class MorrisCounter:
    def __init__(self, bits=8):
        self.X = 0
        self.bits = bits
    def update(self):
        if self.X < (2 ** self.bits) - 1 and \
           random.random() < 1/(2 ** self.X):
            self.X += 1
    def query(self):
        return (2 ** self.X) - 1

```
_A python class for a simple probabalistic counter_

For the python code I also added a variable to simulate the counter being a certain number of bits.

This counter above is known as the [Morris Counter](https://core.ac.uk/download/pdf/208681313.pdf). This algorithm is from 1985, and to me it is exciting both how young and how old the algorithm is. First off it was invented within the last 50 years, but also it was invented long before I was born. What else is exciting is this algorithm, with a minor tweak, recently had a [lower, optimal bound](https://arxiv.org/abs/2010.02116) proven in October of 2020. Never have I been so close to the fronteir of computer science research!

The Morris Counter linked above only can count in powers of 2 minus 1, which is great in terms of conserving bits, but there is a tradeoff with if you have a few more bits you can reduce variance and get a better probabilistic counter. Instead of incrementing with probability 1/2 to the power of X, you increment with 1/(1 + alpha) to the power of X, where alpha is a small constant, and setting alpha equal to 1 gets you back the original Morris Counter. To return the approximate value of increments we return (1/alpha) * ((1 + alpha) ^ X) - 1. Another important improvement is for small values not to use an approximate counter but exact, and switch over to approximate after a certain threshhold.

```python
class MorrisAlpha:
    def __init__(self, a=.05, bits=8, default=5):
        self.X = 0
        self.a = a
        self.bits = bits
        self.default = default
    def update(self):
        if self.X < self.default:
            self.X += 1
        elif self.X < (2 ** self.bits) - 1 and \
             random.random() < 1/((1 + self.a)**self.X):
            self.X += 1
    def query(self):
        if self.X <= self.default:
            return self.X
        return 1/self.a * (((1 + self.a)**self.X) - 1)
```
_A python class for the improved Morris Counter_

Finally for my final project I did a little investigation on the variant of the Morris Counter used by [Redis](https://github.com/redis). The investigation was inspired by [Professor Jelani Nelson's github issue](https://github.com/redis/redis/issues/7943), and the TL; DR is Redis has a Morris counter, but increments X with probability 1/(1 + alpha * X), with the corresponding approximation for the number of increments being (alpha/2) * X ^ 2.

```python
class RedisCounter:
    def __init__(self, a=10, default=5, bits=8):
        self.a = a
        self.default = default
        self.X = 0
        self.bits = bits

    def update(self):
        if self.X != (2 ** self.bits) - 1:
            r = random.random()
            baseval = self.X - self.default
            if baseval < 0:
                baseval = 0
            p = 1/(baseval * self.a + 1)
            if r < p:
                self.X += 1

    def query(self):
        if self.X <= self.default:
            return self.X
        return self.a / 2 * (self.X - self.default) ** 2
```
_A python class for the Redis Morris Counter_

Is this counter better? Worse? Find out in [part 2](Testing-Random-Algorithms.html).