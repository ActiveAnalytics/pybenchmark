pybenchmark
===========

Scripts that carry out benchmarking in Python similar to the microbenchmarks package in R

# Description

The `bench` function is the workhorse function for benchmarking the details are given below

```
bench(sExpr, neval=100, units = "ms", ndigits = 5)
```

`sExpr`: This is a string expression
`units`: These are units, defaults to `"ms"`: milliseconds, but `"us"`: microseconds,
        `"ns"`: nanoseconds, `"s"`: time in seconds, `"raw"`: raw time in seconds are available
`ndigits`: These are number of decimal places

```
benchmark(*args, **kwargs)
```
`*args`: are comma separated string arguments to be evaluated in the benchmark
`**kwargs`: are named arguments passed to the `bench` function

```
lbenchmark(lExpr, **kwargs)
```
`lExpr`: This is a list of expressions
`**kwargs`: are names arguments that are passed to the `bench` function


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
np.random.uniform(size=1000)                 37.9085  38.8622   39.1007   41.008    70.0951      100
np.random.normal(size=1000)                  52.9289  57.9357   61.9888  134.945   178.099       100
np.random.binomial(size=1000, n=10, p = .5)  56.982   59.1278   64.1346   65.0883   88.2149      100
np.random.poisson(size=1000, lam=1)          61.9888  64.075    66.0419   73.6117  119.925       100
```

## A list of expressions

```
>>> evalStrings = ["np.random.uniform(size=1000)", "np.random.normal(size=1000)",
               "np.random.binomial(size=1000, n=10, p = .5)", "np.random.poisson(size=1000, lam=1)"]

>>> lbenchmark(evalStrings, units = "us")
expr                                             min       lq    median        uq       max    neval
-------------------------------------------  -------  -------  --------  --------  --------  -------
np.random.uniform(size=1000)                 24.7955  38.8622   39.1007   41.008   100.851       100
np.random.normal(size=1000)                  56.0284  57.9357  131.965   135.362   169.039       100
np.random.binomial(size=1000, n=10, p = .5)  59.1278  64.075    65.0883   66.0419   94.8906      100
np.random.poisson(size=1000, lam=1)          63.8962  66.0419   69.8566   72.2408  114.918       100
```
