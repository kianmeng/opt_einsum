# Optimized Einsum

[![Tests](https://github.com/dgasmith/opt_einsum/actions/workflows/Tests.yml/badge.svg)](https://github.com/dgasmith/opt_einsum/actions/workflows/Tests.yml)
[![codecov](https://codecov.io/gh/dgasmith/opt_einsum/branch/master/graph/badge.svg)](https://codecov.io/gh/dgasmith/opt_einsum)
[![Anaconda-Server Badge](https://anaconda.org/conda-forge/opt_einsum/badges/version.svg)](https://anaconda.org/conda-forge/opt_einsum)
[![PyPI](https://img.shields.io/pypi/v/opt_einsum.svg)](https://pypi.org/project/opt-einsum/#description)
[![PyPIStats](https://img.shields.io/pypi/dm/opt_einsum)](https://pypistats.org/packages/opt-einsum)
[![Documentation Status](https://github.com/dgasmith/opt_einsum/actions/workflows/Docs.yaml/badge.svg)](https://dgasmith.github.io/opt_einsum/)
[![DOI](https://joss.theoj.org/papers/10.21105/joss.00753/status.svg)](https://doi.org/10.21105/joss.00753)

## Optimized Einsum: A tensor contraction order optimizer

Optimized einsum can significantly reduce the overall execution time of einsum-like expressions (e.g.,
[`np.einsum`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.einsum.html),
[`dask.array.einsum`](https://docs.dask.org/en/latest/array-api.html#dask.array.einsum),
[`pytorch.einsum`](https://pytorch.org/docs/stable/torch.html#torch.einsum),
[`tensorflow.einsum`](https://www.tensorflow.org/api_docs/python/tf/einsum),
)
by optimizing the expression's contraction order and dispatching many
operations to canonical BLAS, cuBLAS, or other specialized routines.

Optimized
einsum is agnostic to the backend and can handle NumPy, Dask, PyTorch,
Tensorflow, CuPy, Sparse, Theano, JAX, and Autograd arrays as well as potentially
any library which conforms to a standard API. See the
[**documentation**](https://dgasmith.github.io/opt_einsum/) for more
information.

## Example usage

The [`opt_einsum.contract`](https://dgasmith.github.io/opt_einsum/api_reference.html#opt_einsumcontract)
function can often act as a drop-in replacement for `einsum`
functions without further changes to the code while providing superior performance.
Here, a tensor contraction is preformed with and without optimization:

```python
import numpy as np
from opt_einsum import contract

N = 10
C = np.random.rand(N, N)
I = np.random.rand(N, N, N, N)

%timeit np.einsum('pi,qj,ijkl,rk,sl->pqrs', C, C, I, C, C)
1 loops, best of 3: 934 ms per loop

%timeit contract('pi,qj,ijkl,rk,sl->pqrs', C, C, I, C, C)
1000 loops, best of 3: 324 us per loop
```

In this particular example, we see a ~3000x performance improvement which is
not uncommon when compared against unoptimized contractions. See the [backend
examples](https://dgasmith.github.io/opt_einsum/getting_started/backends.html)
for more information on using other backends.

## Features

The algorithms found in this repository often power the `einsum` optimizations
in many of the above projects. For example, the optimization of `np.einsum`
has been passed upstream and most of the same features that can be found in
this repository can be enabled with `np.einsum(..., optimize=True)`. However,
this repository often has more up to date algorithms for complex contractions.

The following capabilities are enabled by `opt_einsum`:

* Inspect [detailed information](https://dgasmith.github.io/opt_einsum/paths/introduction.html) about the path chosen.
* Perform contractions with [numerous backends](https://dgasmith.github.io/opt_einsum/getting_started/backends.html), including on the GPU and with libraries such as [TensorFlow](https://www.tensorflow.org) and [PyTorch](https://pytorch.org).
* Generate [reusable expressions](https://dgasmith.github.io/opt_einsum/getting_started/reusing_paths.html), potentially with [constant tensors](https://dgasmith.github.io/opt_einsum/getting_started/reusing_paths.html#specifying-constants), that can be compiled for greater performance.
* Use an arbitrary number of indices to find contractions for [hundreds or even thousands of tensors](https://dgasmith.github.io/opt_einsum/examples/large_expr_with_greedy.html).
* Share [intermediate computations](https://dgasmith.github.io/opt_einsum/getting_started/sharing_intermediates.html) among multiple contractions.
* Compute gradients of tensor contractions using [autograd](https://github.com/HIPS/autograd) or [jax](https://github.com/google/jax)

Please see the [documentation](https://dgasmith.github.io/opt_einsum/index.html) for more features!

## Installation

`opt_einsum` can either be installed via `pip install opt_einsum` or from conda `conda install opt_einsum -c conda-forge`.
See the installation [documentation](https://dgasmith.github.io/opt_einsum/getting_started/install.html) for further methods.

## Citation

If this code has benefited your research, please support us by citing:

Daniel G. A. Smith and Johnnie Gray, opt_einsum - A Python package for optimizing contraction order for einsum-like expressions. *Journal of Open Source Software*, **2018**, 3(26), 753

DOI: <https://doi.org/10.21105/joss.00753>

## Contributing

All contributions, bug reports, bug fixes, documentation improvements, enhancements, and ideas are welcome.

A detailed overview on how to contribute can be found in the [contributing guide](https://github.com/dgasmith/opt_einsum/blob/master/.github/CONTRIBUTING.md).
