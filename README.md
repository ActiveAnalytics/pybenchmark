pybenchmark
===========

Scripts that carry out benchmarking in Python similar to the microbenchmarks package in R

# Description

## The `bench` function
The `bench` function is the workhorse function for benchmarking the details are given below

```
bench(sExpr, neval=100, units = "ms", ndigits = 5)


sExpr: This is a string expression
units: These are units, defaults to "ms": milliseconds, but "us": microseconds,
        "ns": nanoseconds, "s": time in seconds, "raw": raw time in seconds are available
ndigits: the are number of decimal places.
```

## The `benchmark` function

The `benchmark` function is a convenience function for the programmer and the details 
are given below

```
benchmark(*args, **kwargs)

*args: are comma separated string arguments to be evaluated in the benchmark
**kwargs: are named arguments passed to the bench function
```

## The `lbenchmark` function

The `lbenchmark` function is a convenience function for the programmer and the details 
are given below


```
lbenchmark(lExpr, **kwargs)

lExpr: This is a list of expressions
**kwargs: are names arguments that are passed to the bench function
```

# Usage Examples

## Loaded libraries:

```
import collections as col
import numpy as np
import time as tt
import tabulate as tab
```

## Evaluating a single expression

```
>>> bench("np.random.uniform(size=1000)")
expr                              min       lq    median       uq      max    neval
----------------------------  -------  -------  --------  -------  -------  -------
np.random.uniform(size=1000)  0.02289  0.02313   0.02384  0.02408  0.05507      100

>>> benchmark("np.random.uniform(size=1000)")
expr                              min       lq    median       uq      max    neval
----------------------------  -------  -------  --------  -------  -------  -------
np.random.uniform(size=1000)  0.01597  0.01693   0.01717  0.01812  0.05198      100

>>> lbenchmark(["np.random.uniform(size=1000)"])
expr                              min       lq    median       uq      max    neval
----------------------------  -------  -------  --------  -------  -------  -------
np.random.uniform(size=1000)  0.01597  0.01693   0.01693  0.01812  0.05293      100
```

## Entering multiple string expressions

```
>>> print benchmark("np.random.uniform(size=1000)", "np.random.normal(size=1000)",
        "np.random.binomial(size=1000, n=10, p = .5)", "np.random.poisson(size=1000, lam=1)", units="us")
expr                                             min       lq    median        uq       max    neval
-------------------------------------------  -------  -------  --------  --------  --------  -------
np.random.uniform(size=1000)                 15.974   16.9277   16.9277   17.1661   28.8486      100
np.random.normal(size=1000)                  56.982   78.7377   82.016   112.236   325.918       100
np.random.binomial(size=1000, n=10, p = .5)  57.9357  59.8431   60.7967   61.9888  191.212       100
np.random.poisson(size=1000, lam=1)          62.9425  68.6646   71.0487   80.8835  120.878       100
```

## A list of expressions

```
>>> evalStrings = ["np.random.uniform(size=1000)", "np.random.normal(size=1000)",
               "np.random.binomial(size=1000, n=10, p = .5)", "np.random.poisson(size=1000, lam=1)"]

>>> lbenchmark(evalStrings, units = "us")
expr                                             min       lq    median       uq       max    neval
-------------------------------------------  -------  -------  --------  -------  --------  -------
np.random.uniform(size=1000)                 24.7955  25.9876   25.9876  28.1334   56.0284      100
np.random.normal(size=1000)                  76.0555  78.9166   85.1154  86.0691  168.085       100
np.random.binomial(size=1000, n=10, p = .5)  59.1278  64.8499   66.5188  96.0827  120.163       100
np.random.poisson(size=1000, lam=1)          64.8499  69.6778   70.0951  71.0487   97.99        100
```
