<!--

@license Apache-2.0

Copyright (c) 2024 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# dgemv

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Perform one of the matrix-vector operations `y = α*A*x + β*y` or `y = α*A^T*x + β*y`.

<section class="installation">

## Installation

```bash
npm install @stdlib/blas-base-dgemv
```

Alternatively,

-   To load the package in a website via a `script` tag without installation and bundlers, use the [ES Module][es-module] available on the [`esm`][esm-url] branch (see [README][esm-readme]).
-   If you are using Deno, visit the [`deno`][deno-url] branch (see [README][deno-readme] for usage intructions).
-   For use in Observable, or in browser/node environments, use the [Universal Module Definition (UMD)][umd] build available on the [`umd`][umd-url] branch (see [README][umd-readme]).

The [branches.md][branches-url] file summarizes the available branches and displays a diagram illustrating their relationships.

To view installation and usage instructions specific to each branch build, be sure to explicitly navigate to the respective README files on each branch, as linked to above.

</section>

<section class="usage">

## Usage

```javascript
var dgemv = require( '@stdlib/blas-base-dgemv' );
```

#### dgemv( order, trans, M, N, α, A, LDA, x, sx, β, y, sy )

Performs one of the matrix-vector operations `y = α*A*x + β*y` or `y = α*A^T*x + β*y`, where `α` and `β` are scalars, `x` and `y` are vectors, and `A` is an `M` by `N` matrix.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var A = new Float64Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );
var x = new Float64Array( [ 1.0, 1.0, 1.0 ] );
var y = new Float64Array( [ 1.0, 1.0 ] );

dgemv( 'row-major', 'no-transpose', 2, 3, 1.0, A, 3, x, 1, 1.0, y, 1 );
// y => <Float64Array>[ 7.0, 16.0 ]
```

The function has the following parameters:

-   **order**: storage layout.
-   **trans**: specifies whether `A` should be transposed, conjugate-transposed, or not transposed.
-   **M**: number of rows in the matrix `A`.
-   **N**: number of columns in the matrix `A`.
-   **α**: scalar constant.
-   **A**: input matrix stored in linear memory as a [`Float64Array`][mdn-float64array].
-   **LDA**: stride of the first dimension of `A` (a.k.a., leading dimension of the matrix `A`).
-   **x**: input [`Float64Array`][mdn-float64array].
-   **sx**: stride length for `x`.
-   **β**: scalar constant.
-   **y**: output [`Float64Array`][mdn-float64array].
-   **sy**: stride length for `y`.

The stride parameters determine how operations are performed. For example, to iterate over every other element in `x` and `y`,

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var A = new Float64Array( [ 1.0, 2.0, 3.0, 4.0 ] );
var x = new Float64Array( [ 1.0, 0.0, 1.0, 0.0 ] );
var y = new Float64Array( [ 1.0, 0.0, 1.0, 0.0 ] );

dgemv( 'row-major', 'no-transpose', 2, 2, 1.0, A, 2, x, 2, 1.0, y, 2 );
// y => <Float64Array>[ 4.0, 0.0, 8.0, 0.0 ]
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

<!-- eslint-disable stdlib/capitalized-comments -->

```javascript
var Float64Array = require( '@stdlib/array-float64' );

// Initial arrays...
var x0 = new Float64Array( [ 0.0, 1.0, 1.0 ] );
var y0 = new Float64Array( [ 0.0, 1.0, 1.0 ] );
var A = new Float64Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );

// Create offset views...
var x1 = new Float64Array( x0.buffer, x0.BYTES_PER_ELEMENT*1 ); // start at 2nd element
var y1 = new Float64Array( y0.buffer, y0.BYTES_PER_ELEMENT*1 ); // start at 2nd element

dgemv( 'row-major', 'no-transpose', 2, 2, 1.0, A, 2, x1, -1, 1.0, y1, -1 );
// y0 => <Float64Array>[ 0.0, 8.0, 4.0 ]
```

<!-- lint disable maximum-heading-length -->

#### dgemv.ndarray( trans, M, N, α, A, sa1, sa2, oa, x, sx, ox, β, y, sy, oy )

Performs one of the matrix-vector operations `y = α*A*x + β*y` or `y = α*A^T*x + β*y`, using alternative indexing semantics and where `α` and `β` are scalars, `x` and `y` are vectors, and `A` is an `M` by `N` matrix.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var A = new Float64Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );
var x = new Float64Array( [ 1.0, 1.0, 1.0 ] );
var y = new Float64Array( [ 1.0, 1.0 ] );

dgemv.ndarray( 'no-transpose', 2, 3, 1.0, A, 3, 1, 0, x, 1, 0, 1.0, y, 1, 0 );
// y => <Float64Array>[ 7.0, 16.0 ]
```

The function has the following additional parameters:

-   **sa1**: stride of the first dimension of `A`.
-   **sa2**: stride of the second dimension of `A`.
-   **oa**: starting index for `A`.
-   **ox**: starting index for `x`.
-   **oy**: starting index for `y`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying buffer, the offset parameters support indexing semantics based on starting indices. For example,

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var A = new Float64Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );
var x = new Float64Array( [ 0.0, 1.0, 2.0, 3.0 ] );
var y = new Float64Array( [ 7.0, 8.0, 9.0, 10.0 ] );

dgemv.ndarray( 'no-transpose', 2, 3, 1.0, A, 3, 1, 0, x, 1, 1, 1.0, y, -2, 2 );
// y => <Float64Array>[ 39, 8, 23, 10 ]
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   `dgemv()` corresponds to the [BLAS][blas] level 2 function [`dgemv`][blas-dgemv].

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
var discreteUniform = require( '@stdlib/random-array-discrete-uniform' );
var dgemv = require( '@stdlib/blas-base-dgemv' );

var opts = {
    'dtype': 'float64'
};

var M = 3;
var N = 3;

var A = discreteUniform( M*N, 0, 255, opts );
var x = discreteUniform( N, 0, 255, opts );
var y = discreteUniform( M, 0, 255, opts );

dgemv( 'row-major', 'no-transpose', M, N, 1.0, A, N, x, -1, 1.0, y, -1 );
console.log( y );

```

</section>

<!-- /.examples -->

<!-- C interface documentation. -->

* * *

<section class="c">

## C APIs

<!-- Section to include introductory text. Make sure to keep an empty line after the intro `section` element and another before the `/section` close. -->

<section class="intro">

</section>

<!-- /.intro -->

<!-- C usage documentation. -->

<section class="usage">

### Usage

```c
#include "stdlib/blas/base/dgemv.h"
```

#### c_dgemv( layout, trans, M, N, alpha, \*A, LDA, \*X, strideX, beta, \*Y, strideY )

Performs one of the matrix-vector operations `y = α*A*x + β*y` or `y = α*A^T*x + β*y`, where `α` and `β` are scalars, `x` and `y` are vectors, and `A` is an `M` by `N` matrix.

```c
#include "stdlib/blas/base/shared.h"

const double A[] = { 1.0, 0.0, 0.0, 2.0, 1.0, 0.0, 3.0, 2.0, 1.0 };
const double x[] = { 1.0, 2.0, 3.0 };
double y[] = { 1.0, 2.0, 3.0 };

c_dgemv( CblasColMajor, CblasNoTrans, 3, 3, 1.0, A, 3, x, 1, 1.0, y, 1 );
```

The function accepts the following arguments:

-   **layout**: `[in] CBLAS_LAYOUT` storage layout.
-   **trans**: `[in] CBLAS_TRANSPOSE` specifies whether `A` should be transposed, conjugate-transposed, or not transposed.
-   **M**: `[in] CBLAS_INT` number of rows in the matrix `A`.
-   **N**: `[in] CBLAS_INT` number of columns in the matrix `A`.
-   **alpha**: `[in] double` scalar constant.
-   **A**: `[in] double*` input matrix.
-   **LDA**: `[in] CBLAS_INT` stride of the first dimension of `A` (a.k.a., leading dimension of the matrix `A`).
-   **X**: `[in] double*` first input vector.
-   **strideX**: `[in] CBLAS_INT` stride length for `X`.
-   **beta**: `[in] double` scalar constant.
-   **Y**: `[inout] double*` second input vector.
-   **strideY**: `[in] CBLAS_INT` stride length for `Y`.

```c
void c_dgemv( const CBLAS_LAYOUT layout, const CBLAS_TRANSPOSE trans, const CBLAS_INT M, const CBLAS_INT N, const double alpha, const double *A, const CBLAS_INT LDA, const double *X, const CBLAS_INT strideX, const double beta, double *Y, const CBLAS_INT strideY )
```

#### c_dgemv_ndarray( trans, M, N, alpha, \*A, sa1, sa2, oa, \*X, sx, ox, beta, \*Y, sy, oy )

Performs one of the matrix-vector operations `y = α*A*x + β*y` or `y = α*A^T*x + β*y`, using indexing alternative semantics and where `α` and `β` are scalars, `x` and `y` are vectors, and `A` is an `M` by `N` matrix.

```c
#include "stdlib/blas/base/shared.h"

const double A[] = { 1.0, 0.0, 0.0, 2.0, 1.0, 0.0, 3.0, 2.0, 1.0 };
const double x[] = { 1.0, 2.0, 3.0 };
double y[] = { 1.0, 2.0, 3.0 };

c_dgemv_ndarray( CblasNoTrans, 3, 3, 1.0, A, 1, 3, 0, x, 1, 0, 1.0, y, 1, 0 );
```

The function accepts the following arguments:

-   **trans**: `[in] CBLAS_TRANSPOSE` specifies whether `A` should be transposed, conjugate-transposed, or not transposed.
-   **M**: `[in] CBLAS_INT` number of rows in the matrix `A`.
-   **N**: `[in] CBLAS_INT` number of columns in the matrix `A`.
-   **alpha**: `[in] double` scalar.
-   **A**: `[in] double*` input matrix.
-   **sa1**: `[in] CBLAS_INT` stride of the first dimension of `A`.
-   **sa2**: `[in] CBLAS_INT` stride of the second dimension of `A`.
-   **oa**: `[in] CBLAS_INT` starting index for `A`.
-   **X**: `[in] double*` first input vector.
-   **sx**: `[in] CBLAS_INT` stride length for `X`.
-   **ox**: `[in] CBLAS_INT` starting index for `X`.
-   **beta**: `[in] double` scalar.
-   **Y**: `[inout] double*` second input vector.
-   **sy**: `[in] CBLAS_INT` stride length for `Y`.
-   **oy**: `[in] CBLAS_INT` starting index for `Y`.

```c
void c_dgemv_ndarray( const CBLAS_TRANSPOSE trans, const CBLAS_INT M, const CBLAS_INT N, const double alpha, const double *A, const CBLAS_INT strideA1, const CBLAS_INT strideA2, const CBLAS_INT offsetA, const double *X, const CBLAS_INT strideX, const CBLAS_INT offsetX, const double beta, double *Y, const CBLAS_INT strideY, const CBLAS_INT offsetY )
```

</section>

<!-- /.usage -->

<!-- C API usage notes. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="notes">

</section>

<!-- /.notes -->

<!-- C API usage examples. -->

<section class="examples">

### Examples

```c
#include "stdlib/blas/base/dgemv.h"
#include "stdlib/blas/base/shared.h"
#include <stdio.h>

int main( void ) {
    // Define a 3x3 matrix stored in row-major order:
    const double A[ 3*3 ] = {
        1.0, 2.0, 3.0,
        4.0, 5.0, 6.0,
        7.0, 8.0, 9.0
    };

    // Define `x` and `y` vectors:
    const double x[ 3 ] = { 1.0, 2.0, 3.0 };
    double y[ 3 ] = { 1.0, 2.0, 3.0 };

    // Specify the number of elements along each dimension of `A`:
    const int M = 3;
    const int N = 3;

    // Perform the matrix-vector operation `y = α*A*x + β*y`:
    c_dgemv( CblasRowMajor, CblasNoTrans, M, N, 1.0, A, M, x, 1, 1.0, y, 1 );

    // Print the result:
    for ( int i = 0; i < N; i++ ) {
        printf( "y[ %i ] = %lf\n", i, y[ i ] );
    }

    // Perform the matrix-vector operation `y = α*A*x + β*y` using alternative indexing semantics:
    c_dgemv_ndarray( CblasNoTrans, M, N, 1.0, A, N, 1, 0, x, 1, 0, 1.0, y, 1, 0 );

    // Print the result:
    for ( int i = 0; i < N; i++ ) {
        printf( "y[ %i ] = %lf\n", i, y[ i ] );
    }
}
```

</section>

<!-- /.examples -->

</section>

<!-- /.c -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2026. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/blas-base-dgemv.svg
[npm-url]: https://npmjs.org/package/@stdlib/blas-base-dgemv

[test-image]: https://github.com/stdlib-js/blas-base-dgemv/actions/workflows/test.yml/badge.svg?branch=v0.1.0
[test-url]: https://github.com/stdlib-js/blas-base-dgemv/actions/workflows/test.yml?query=branch:v0.1.0

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/blas-base-dgemv/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/blas-base-dgemv?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/blas-base-dgemv.svg
[dependencies-url]: https://david-dm.org/stdlib-js/blas-base-dgemv/main

-->

[chat-image]: https://img.shields.io/badge/zulip-join_chat-brightgreen.svg
[chat-url]: https://stdlib.zulipchat.com

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/blas-base-dgemv/tree/deno
[deno-readme]: https://github.com/stdlib-js/blas-base-dgemv/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/blas-base-dgemv/tree/umd
[umd-readme]: https://github.com/stdlib-js/blas-base-dgemv/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/blas-base-dgemv/tree/esm
[esm-readme]: https://github.com/stdlib-js/blas-base-dgemv/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/blas-base-dgemv/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/blas-base-dgemv/main/LICENSE

[blas]: http://www.netlib.org/blas

[blas-dgemv]: https://www.netlib.org/lapack/explore-html/d7/dda/group__gemv_ga4ac1b675072d18f902db8a310784d802.html#ga4ac1b675072d18f902db8a310784d802

[mdn-float64array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float64Array

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

</section>

<!-- /.links -->
