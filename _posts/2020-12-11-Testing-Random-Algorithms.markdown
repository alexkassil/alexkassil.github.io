---
layout: post
title: "Testing Random Algorithms"
date: 2020-12-11
tags: Morris Counter Streaming Algorithms Testing
---

## Introduction

This is a followup to [part 1](Counting-with-less-Bits.html). A quick recap is we had two slightly different approximate counters, and now we will figure out which is better and learn about how to test functions that have randomness.

### Tests

I was interested in exploring how programmers write tests for code that is inherently random. One runs into a few problems immediately. What should your expected output be to test a function that approximates? What if the function fails? What should I be testing? 

I found two satisfying answers. One is to decouple the randomness from the code you are testing. One way to do this is with [Dependency Injection](https://softwareengineering.stackexchange.com/questions/356456/testing-a-function-that-uses-random-number-generator), or passing in the source of randomness, and then mocking it out during the test with any deterministic sequence you want. One way to utilize this for testing the Morris Counter is to mock the randomness and have the calls to randomness return values from 0 to 1 each time increasing by increments of 1/n, where n is the number of times you are incrementing the counter. The benefit of such a test would be complicated enough to test your Counter is correct, while also deterministic so it would have no chance of failing with a correct implementation. Another way to achieve similar results is to seed your random generator at the beginning of each test with the same seed. That way each algorithm gets the same stream of psuedorandom numbers, and results are consistent.

Below is an example of a test I wrote with the random seed being set to decouple randomness from code that is being tested. The code is all available [here](https://github.com/alexkassil/sketching-testing).


```python
def run(counterClass, kwargs, times, N):
    res = []
    for _ in range(times):
        counter = counterClass(**kwargs)
        for _ in range(N):
            counter.update()
        res.append(counter.query())
    return np.array(res)

...
def test_RedisCounter_standard_deviation(self):
    random.seed(seed)
    vals = run(RedisCounter, {}, self.times, self.N)
    within_25_percent = np.mean((self.N - self.N/4 <= vals) & \
                                (self.N + self.N/4 >= vals))
    print("\nFor RedisCounter,", str(within_25_percent * 100)+\
          "% of runs are within 25% on either side of", self.N, \
          "after", self.times, "runs calling update", self.N,"times")
    self.assertTrue(within_25_percent > .75)
...
```

The second satisfying answer I found to testing probabilistic code is [statistical tests](https://beust.com/weblog2/archives/2006_02_21.html). A simple example of a statistical test for the Morris Counter is to average the results of many different runs of the same counter. You can also utilize the theoretical bounds on failure probabilities and arbitrarily-close guarantees to know exactly how small of a chance this test fails if everything is implemented correctly. The test above is an example of that, so is the test below.



```python
def test_RedisCounter_expectation(self):
    random.seed(seed)
    average = 0
    for _ in range(self.times):
        c = RedisCounter()
        for _ in range(self.N):
            c.update()
        average += c.query()
average = average / self.times
print("\n" + type(c).__name__ + "'s average is", average,\
 "after", self.times, "runs calling update", self.N,"times")
    self.assertTrue(within(average, self.N, self.N * 1/25))
```

Finally below is the output on running the full suite of tests I wrote for the deterministic Counter, Basic Morris Counter, Alpha Morris Counter with alpha=.05 and 8 bits, and the Redis Morris Counter with alpha=10 and 8 bits. Here is the output of running `python3 -m unittest test.py`, [test file](https://github.com/alexkassil/sketching-testing/blob/main/test.py):

```
Counter's average is 10000.0 after \
100 runs calling update 10000 times
.
MorrisAlpha's average is 9838.326270536447 after \
100 runs calling update 10000 times
.
MorrisCounter's average is 9399.32 after \
100 runs calling update 10000 times
F
RedisCounter's average is 10089.4 after \
100 runs calling update 10000 times
.
For Counter, 100.0% of runs are within 25% on either \
side of 10000 after 100 runs calling update 10000 times
.
For MorrisAlpha, 90.0% of runs are within 25% on either \
side of 10000 after 100 runs calling update 10000 times
.
For MorrisCounter, 49.0% of runs are within 25% on either \
side of 10000 after 100 runs calling update 10000 times
F
For RedisCounter, 91.0% of runs are within 25% on either \
side of 10000 after 100 runs calling update 10000 times
.
======================================================================
FAIL: test_MorrisCounter_expectation (test.TestExpectation)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "test.py", line 79, in test_MorrisCounter_expectation
    self.assertTrue(within(average, self.N, self.N * 1/25))
AssertionError: False is not true

======================================================================
FAIL: test_MorrisCounter_standard_deviation (test.TestStandardDeviation)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "test.py", line 37, in test_MorrisCounter_standard_deviation
    self.assertTrue(within_25_percent > .75)
AssertionError: False is not true

----------------------------------------------------------------------
Ran 8 tests in 5.141s

FAILED (failures=2)
```

The two failures are from the Basic Morris Counter and highlight why you shouldn't use it in practice.

### Visualizations

I made some charts here to help show what the Morris Counter in all it's forms is doing and how the Alpha Morris Counter and the Redis Morris Counter relate.


## Conclusion

I was hoping to have a definitive answer to why the Redis Morris algorithm was great, but based on the testing and the visualizations it seems like the two algorithms, for the right parameter alpha, are quite similar. There might be a slight edge towards the Redis implementation, due to its more concentrated nature at large values, as shown in the last chart for 1,000,000 million insertions.
