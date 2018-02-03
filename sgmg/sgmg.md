SGMG\
Sparse Grids, Mixed Factors, Growth Rules {#sgmg-sparse-grids-mixed-factors-growth-rules align="center"}
=========================================

------------------------------------------------------------------------

**SGMG** is a C++ library which constructs a sparse grid whose factors
are possibly distinct 1D quadrature rules.

**SGMG** is the successor to the SPARSE\_GRID\_MIXED library. It
includes an option to specify the growth rule that converts a level
(which indexes the sparse grids) to the order (which counts the number
of points in a 1D rule.)

**SGMG** calls many routines from the **SANDIA\_RULES** library. Source
code or compiled copies of *both* libraries must be available when a
program wishes to use the **SGMG** library.

Note that there is a variation of this library which is available,
called **SANDIA\_SGMG**. The variant code has the same capabilities, but
uses a different interface when it calls the functions in
**SANDIA\_RULES** that generate points and weights of quadrature rules.

Sparse grids are organized by a series of levels, with level 0 being the
simplest grid. Sparse grids are generated by weighted sums of product
rules. Each product rule is formed by the product of 1D quadrature
rules. The 1D quadrature rules also have levels, starting with 0.
Although the 1D quadrature rule is probably available in versions of any
order (number of points), the sparse grid will usually be based on a
selection of these 1D rules. These rules are indexed by a 1D level. The
relationship between the 1D level and the actual order of the 1D
quadrature rule is somewhat arbitrary. In most cases, if we can
guarantee that the the 1D quadrature rule of level L has 1D polynomial
precision 2\*L+1, then we can deduce that the sparse grid of level L
will also have polynomial precision 2\*L+1. It doesn't hurt for the 1D
rule to have greater precision than that, but if this minimal precision
is not achieved, then the precision of the sparse grid rule cannot be
guaranteed.

This program allows the user a great deal of flexibility in choosing the
growth rules, that is, the formula that determines the order O of a 1D
quadrature rule of 1D level L.

The user may be familiar with the typical sparse grid associated with
the 1D Clenshaw-Curtis rule, for which the growth rule may be termed
"exponential". Specifically, for this growth rule, we have

-   the 1D rule for level L = 0 has order O = 1;
-   for 0 &lt; L, the 1D rule of level L has order 2\^L+1;

For Gauss rules, the typical exponential growth is similar:

-   the 1D rule of level L has order O = 2\^(L+1)-1;

However, these growth rules very quickly produce 1D quadrature rules of
excessive order. So this package allows the user to request variations
of the growth rules, including:

-   slow linear
-   slow linear, odd
-   moderate linear
-   slow exponential
-   moderate exponential
-   full exponential

The details of these growth rules are described in a table below.

The user controls the growth rate by setting an additional argument
whose dummy name is **level\_to\_order**, and which represents a pointer
to a function which determines the relationship between the level of a
1D quadrature rule and the order, or number of points, that it uses:

-   **level\_to\_order\_default** uses the original exponential growth
    rate for the fully nested rules ("CC", "F2" and "GP"), and a simple
    linear growth rate for other rules ("GL", "GH", "GGH", "LG", "GLG",
    "GJ" and "GW").
-   **level\_to\_order\_exponential** uses the original exponential
    growth rate for all rules.
-   **level\_to\_order\_linear** uses the simple linear growth rate for
    all rules.

The 1D quadrature rules are designed to approximate an integral of the
form:

> **Integral ( A &lt; X &lt; B ) F(X) W(X) dX**

where **W(X)** is a weight function, by the quadrature sum:

> **Sum ( 1 &lt;= I &lt;= ORDER) F(X(I)) \* W(I)**

where the set of **X** values are known as *abscissas* and the set of
**W** values are known as *weights*.

Note that the letter **W**, unfortunately, is used to denote both the
weight function in the original integral, and the vector of weight
values in the quadrature sum.

  Index   Name                         Abbreviation   Default Growth Rule    Interval    Weight function
  ------- ---------------------------- -------------- ---------------------- ----------- --------------------------
  1       Clenshaw-Curtis              CC             Moderate Exponential   \[-1,+1\]   1
  2       Fejer Type 2                 F2             Moderate Exponential   \[-1,+1\]   1
  3       Gauss Patterson              GP             Moderate Exponential   \[-1,+1\]   1
  4       Gauss-Legendre               GL             Moderate Linear        \[-1,+1\]   1
  5       Gauss-Hermite                GH             Moderate Linear        (-oo,+oo)   e^-x\*x^
  6       Generalized Gauss-Hermite    GGH            Moderate Linear        (-oo,+oo)   |x|^alpha^ e^-x\*x^
  7       Gauss-Laguerre               LG             Moderate Linear        \[0,+oo)    e^-x^
  8       Generalized Gauss-Laguerre   GLG            Moderate Linear        \[0,+oo)    x^alpha^ e^-x^
  9       Gauss-Jacobi                 GJ             Moderate Linear        \[-1,+1\]   (1-x)^alpha^ (1+x)^beta^
  10      Hermite Genz-Keister         HGK            Moderate Exponential   (-oo,+oo)   e^-x\*x^
  11      User Supplied Open           UO             Moderate Linear        ?           ?
  12      User Supplied Closed         UC             Moderate Linear        ?           ?

For a given family, a growth rule can be prescribed, which determines
the orders O of the sequence of rules selected from the family. The
selected rules are indexed by L, which starts at 0. The polynomial
precision P of the rule is sometimes used to determine the appropriate
order O.

  Index   Name                                                      Order Formula
  ------- --------------------------------------------------------- ----------------------------------------------------------
  0       "DF", Default                                             moderate exponential or moderate linear
  1       "SL", Slow linear                                         O=L+1
  2       "SO", Slow Linear Odd (always use a rule of oddd order)   O=1+2\*((L+1)/2)
  3       "ML", Moderate Linear                                     O=2L+1
  4       "SE", Slow Exponential                                    select smallest exponential order O so that 2L+1 &lt;= P
  5       "ME", Moderate Exponential                                select smallest exponential order O so that 4L+1 &lt;= P
  6       "FE", Full Exponential                                    O=2\^L+1 for Clenshaw Curtis, O=2\^(L+1)-1 otherwise.

A sparse grid is a quadrature rule for a multidimensional integral. It
is formed by taking a certain linear combination of lower-order product
rules. The product rules, in turn, are formed as direct products of 1D
quadrature rules. It is common to form a sparse grid in which the 1D
component quadrature rules are the same. This package, however, is
intended to produce sparse grids based on sums of product rules for
which the rule chosen for each spatial dimension may be freely chosen
from the set listed above.

These sparse grids are still indexed by a number known as **level**, and
assembled as a sum of low order product rules. As the value of **level**
increases, the point growth becomes more complicated. This is because
the 1D rules have somewhat varying point growth patterns to begin with,
and the varying levels of nestedness have a dramatic effect on the
sparsity of the total grid.

Since a sparse grid is made up of a combination of product grids, it is
frequently the case that many of the product grids include the same
point. For efficiency, it is usually desirable to merge or consolidate
such duplicate points when expressing the resulting sparse grid rule. It
is possible to "logically" determine when a duplicate point will be
generated; however, this logic changes depending on the specific
1-dimensional rules being used, and the tests can become quite
elaborate. Moreover, for rules which include real parameters, the
determination of duplication can require a numerical tolerance.

In order to simplify the matter of the detection of duplicate points,
the codes presented begin by generating the entire "naive" set of
points. Then a user-specified tolerance **TOL** is used to determine
when two points are equal. If the maximum difference between any two
components is less than or equal to **TOL**, the points are declared to
be equal.

A reasonable value for **TOL** might be the square root of the machine
precision. Setting **TOL** to zero means that only points which are
identical to the last significant digit are taken to be duplicates.
Setting **TOL** to a negative value means that no duplicate points will
be eliminated - in other words, this choice produces the full or "naive"
grid.

### Golub Welsch Rules {#golub-welsch-rules align="center"}

A multidimensional sparse grid rule is defined in terms of the 1D
quadrature rules used in each dimension. This library provides 9 such
standard rules. If **rule\[dim\]** is between 1 and 9, then the
appropriate internal routines are called to compute the points and
weights, and assumptions are also made about the growth rate in the
rule.

The user may wish to define other quadrature rules, and to combine these
with the built-in rules. Such rules are assumed to be generated by the
Golub Welsch procedure. To build a sparse grid rule which uses a Golub
Welsch rule in dimension **dim**, the user must do the following:

1\) Set **rule\[dim\]=10** to indicate that a Golub Welsch rule is being
used in this dimension. The user may send parameters to this rule by
setting **np** and **p**.

2\) Write a point function of the form

            void compute_points ( int order, int np, double p[], double points[] )
          

which may use np parameter values in the **p** vector as parameters for
the rule, computing the points of the rule of order **order** and
returning them in the array **points\[\]**;

3\) Write a weight function of the form

            void compute_weights ( int order, int np, double p[], double weights[] )
          

which may use np parameter values in the **p** vector as parameters for
the rule, computing the weights of the rule of order **order** and
returning them in the array **weights\[\]**;

The program knows a lot about sparse grid rules which are built entirely
from the predefined internal rules. On the other hand, if even a single
component of the sparse grid rule involves a user-defined Golub-Welsch
rule, there are some things the program does not know how to do.
Generally, these are minor issues, but the user should be aware of them.
In particular:

-   the function **sgmg\_write** which writes files defining a sparse
    grid rule does not have enough information to specify the
    integration region, the "R" file;
-   one of the test codes checks a sparse grid rule's accuracy by
    summing the weights. But the test code does not know enough about
    the correct result for such a sum when a Golub-Welsch rule is one of
    the factors of the sparse grid rule;

### Web Link: {#web-link align="center"}

A version of the sparse grid library is available in
[http://tasmanian.ornl.gov](http://tasmanian.ornl.gov/), the TASMANIAN
library, available from Oak Ridge National Laboratory.

### Licensing: {#licensing align="center"}

The code described and made available on this web page is distributed
under the [GNU LGPL](gnu_lgpl.txt) license.

### Languages: {#languages align="center"}

**SGMG** is available in [a C++ version](../../cpp_src/sgmg/sgmg.html).

### Related Data and Programs: {#related-data-and-programs align="center"}

[NINT\_EXACTNESS\_MIXED](../../cpp_src/nint_exactness_mixed/nint_exactness_mixed.html),
a C++ program which measures the polynomial exactness of a
multidimensional quadrature rule based on a mixture of 1D quadrature
rule factors.

[QUADRULE](../../cpp_src/quadrule/quadrule.html), a C++ library which
defines quadrature rules for various intervals and weight functions.

[SANDIA\_RULES](../../cpp_src/sandia_rules/sandia_rules.html), a C++
library which produces 1D quadrature rules of Chebyshev, Clenshaw
Curtis, Fejer 2, Gegenbauer, generalized Hermite, generalized Laguerre,
Hermite, Jacobi, Laguerre, Legendre and Patterson types.

[SANDIA\_SGMG](../../cpp_src/sandia_sgmg/sandia_sgmg.html), a C++
library which creates a sparse grid dataset based on a mixed set of 1D
factor rules, and experiments with the use of a linear growth rate for
the quadrature rules. This is a version of SGMG that uses a different
procedure for supplying the parameters needed to evaluate certain
quadrature rules.

[SANDIA\_SPARSE](../../cpp_src/sandia_sparse/sandia_sparse.html), a C++
library which computes the points and weights of a Smolyak sparse grid,
based on a variety of 1-dimensional quadrature rules.

[SGMG](../../datasets/sgmg/sgmg.html), a dataset directory which
contains multidimensional Smolyak sparse grids based on a mixed set of
1D factor rules and a choice of growth rules.

[SGMGA](../../cpp_src/sgmga/sgmga.html), a C++ library which creates
sparse grids based on a mixture of 1D quadrature rules, allowing
anisotropic weights for each dimension.

[SMOLPACK](../../c_src/smolpack/smolpack.html), a C library which
implements Novak and Ritter's method for estimating the integral of a
function over a multidimensional hypercube using sparse grids, by Knut
Petras.

[SPARSE\_GRID\_MIXED](../../cpp_src/sparse_grid_mixed/sparse_grid_mixed.html),
a C++ library which creates sparse grids based on a mix of 1D rules.

[TOMS847](../../m_src/toms847/toms847.html), a MATLAB program which uses
sparse grids to carry out multilinear hierarchical interpolation. It is
commonly known as SPINTERP, and is by Andreas Klimke.

### Reference: {#reference align="center"}

1.  Milton Abramowitz, Irene Stegun,\
    Handbook of Mathematical Functions,\
    National Bureau of Standards, 1964,\
    ISBN: 0-486-61272-4,\
    LC: QA47.A34.
2.  Charles Clenshaw, Alan Curtis,\
    A Method for Numerical Integration on an Automatic Computer,\
    Numerische Mathematik,\
    Volume 2, Number 1, December 1960, pages 197-205.
3.  Philip Davis, Philip Rabinowitz,\
    Methods of Numerical Integration,\
    Second Edition,\
    Dover, 2007,\
    ISBN: 0486453391,\
    LC: QA299.3.D28.
4.  Michael Eldred, John Burkardt,\
    Comparison of Non-Intrusive Polynomial Chaos and Stochastic
    Collocation Methods for Uncertainty Quantification,\
    American Institute of Aeronautics and Astronautics,\
    Paper 2009-0976,\
    47th AIAA Aerospace Sciences Meeting Including The New Horizons
    Forum and Aerospace Exposition,\
    5 - 8 January 2009, Orlando, Florida.
5.  Walter Gautschi,\
    Numerical Quadrature in the Presence of a Singularity,\
    SIAM Journal on Numerical Analysis,\
    Volume 4, Number 3, September 1967, pages 357-362.
6.  Thomas Gerstner, Michael Griebel,\
    Numerical Integration Using Sparse Grids,\
    Numerical Algorithms,\
    Volume 18, Number 3-4, 1998, pages 209-232.
7.  Gene Golub, John Welsch,\
    Calculation of Gaussian Quadrature Rules,\
    Mathematics of Computation,\
    Volume 23, Number 106, April 1969, pages 221-230.
8.  Prem Kythe, Michael Schaeferkotter,\
    Handbook of Computational Methods for Integration,\
    Chapman and Hall, 2004,\
    ISBN: 1-58488-428-2,\
    LC: QA299.3.K98.
9.  Albert Nijenhuis, Herbert Wilf,\
    Combinatorial Algorithms for Computers and Calculators,\
    Second Edition,\
    Academic Press, 1978,\
    ISBN: 0-12-519260-6,\
    LC: QA164.N54.
10. Fabio Nobile, Raul Tempone, Clayton Webster,\
    A Sparse Grid Stochastic Collocation Method for Partial Differential
    Equations with Random Input Data,\
    SIAM Journal on Numerical Analysis,\
    Volume 46, Number 5, 2008, pages 2309-2345.
11. Thomas Patterson,\
    The Optimal Addition of Points to Quadrature Formulae,\
    Mathematics of Computation,\
    Volume 22, Number 104, October 1968, pages 847-856.
12. Sergey Smolyak,\
    Quadrature and Interpolation Formulas for Tensor Products of Certain
    Classes of Functions,\
    Doklady Akademii Nauk SSSR,\
    Volume 4, 1963, pages 240-243.
13. Arthur Stroud, Don Secrest,\
    Gaussian Quadrature Formulas,\
    Prentice Hall, 1966,\
    LC: QA299.4G3S7.
14. Joerg Waldvogel,\
    Fast Construction of the Fejer and Clenshaw-Curtis Quadrature
    Rules,\
    BIT Numerical Mathematics,\
    Volume 43, Number 1, 2003, pages 1-18.

### Source Code: {#source-code align="center"}

-   [sgmg.cpp](sgmg.cpp), the source code.
-   [sgmg.hpp](sgmg.hpp), the include file.

### Examples and Tests: {#examples-and-tests align="center"}

**COMP\_NEXT\_PRB** tests COMP\_NEXT.

-   [comp\_next\_prb.cpp](comp_next_prb.cpp), a sample calling program.
-   [comp\_next\_prb.sh](comp_next_prb.sh), commands to compile and run
    the sample program.
-   [comp\_next\_prb\_output.txt](comp_next_prb_output.txt), the output
    file.

**PRODUCT\_MIXED\_GROWTH\_WEIGHT\_PRB** tests
PRODUCT\_MIXED\_GROWTH\_WEIGHT.

-   [product\_mixed\_growth\_weight\_prb.cpp](product_mixed_growth_weight_prb.cpp),
    a sample calling program.
-   [product\_mixed\_growth\_weight\_prb.sh](product_mixed_growth_weight_prb.sh),
    commands to compile and run the sample program.
-   [product\_mixed\_growth\_weight\_prb\_output.txt](product_mixed_growth_weight_prb_output.txt),
    the output file.

**SGMG\_INDEX\_PRB** tests SGMG\_INDEX.

-   [sgmg\_index\_prb.cpp](sgmg_index_prb.cpp), tests SGMG\_INDEX.
-   [sgmg\_index\_prb.sh](sgmg_index_prb.sh), commands to compile and
    run the sample program.
-   [sgmg\_index\_prb\_output.txt](sgmg_index_prb_output.txt), the
    output file.

**SGMG\_POINT\_PRB** tests SGMG\_POINT.

-   [sgmg\_point\_prb.cpp](sgmg_point_prb.cpp), tests SGMG\_POINT.
-   [sgmg\_point\_prb.sh](sgmg_point_prb.sh), commands to compile and
    run the sample program.
-   [sgmg\_point\_prb\_output.txt](sgmg_point_prb_output.txt), the
    output file.

**SGMG\_SIZE\_PRB** tests SGMG\_SIZE and SGMG\_SIZE\_TOTAL.

-   [sgmg\_size\_prb.cpp](sgmg_size_prb.cpp), the test program.
-   [sgmg\_size\_prb.sh](sgmg_size_prb.sh), commands to compile and run
    the sample program.
-   [sgmg\_size\_prb\_output.txt](sgmg_size_prb_output.txt), the output
    file.

**SGMG\_SIZE\_TABLE** makes a point count table.

-   [sgmg\_size\_table.cpp](sgmg_size_table.cpp), the test program.
-   [sgmg\_size\_table.sh](sgmg_size_table.sh), commands to compile and
    run the sample program.
-   [sgmg\_size\_table\_output.txt](sgmg_size_table_output.txt), the
    output file.

**SGMG\_UNIQUE\_INDEX\_PRB** tests SGMG\_UNIQUE\_INDEX.

-   [sgmg\_unique\_index\_prb.cpp](sgmg_unique_index_prb.cpp), the test
    program.
-   [sgmg\_unique\_index\_prb.sh](sgmg_unique_index_prb.sh), commands to
    compile and run the sample program.
-   [sgmg\_unique\_index\_prb\_output.txt](sgmg_unique_index_prb_output.txt),
    the output file.

**SGMG\_WEIGHT\_PRB** tests SGMG\_WEIGHT.

-   [sgmg\_weight\_prb.cpp](sgmg_weight_prb.cpp), the test program.
-   [sgmg\_weight\_prb.sh](sgmg_weight_prb.sh), commands to compile and
    run the sample program.
-   [sgmg\_weight\_prb\_output.txt](sgmg_weight_prb_output.txt), the
    output file.

**SGMG\_WRITE\_PRB** tests SGMG\_WRITE.

-   [sgmg\_write\_prb.cpp](sgmg_write_prb.cpp), the test program.
-   [sgmg\_write\_prb.sh](sgmg_write_prb.sh), commands to compile and
    run the sample program.
-   [sgmg\_write\_prb\_output.txt](sgmg_write_prb_output.txt), the
    output file.

**SGMG\_CC\_SL** creates a sparse grid using the Clenshaw-Curtis 1D
quadrature rule with slow linear growth.

-   [sgmg\_cc\_sl.cpp](sgmg_cc_sl.cpp), the test program.
-   [sgmg\_cc\_sl.sh](sgmg_cc_sl.sh), commands to compile and run the
    sample program.
-   [sgmg\_cc\_sl\_output.txt](sgmg_cc_sl_output.txt), the output file.
-   [sgmg\_cc\_sl\_n.txt](sgmg_cc_sl_n.txt), the N (number of
    parameters) data file;
-   [sgmg\_cc\_sl\_p.txt](sgmg_cc_sl_p.txt), the P (parameter value)
    data file;
-   [sgmg\_cc\_sl\_r.txt](sgmg_cc_sl_r.txt), the R (region dimension)
    data file;
-   [sgmg\_cc\_sl\_w.txt](sgmg_cc_sl_w.txt), the W (sparse grid weights)
    data file;
-   [sgmg\_cc\_sl\_x.txt](sgmg_cc_sl_x.txt), the X (sparse grid
    abscissas) data file;

### List of Routines: {#list-of-routines align="center"}

-   **PRODUCT\_MIXED\_GROWTH\_WEIGHT** computes the weights of a mixed
    product rule.
-   **SGMG\_INDEX** indexes a sparse grid of mixed 1D rules.
-   **SGMG\_POINT** computes the points of a sparse grid rule.
-   **SGMG\_SIZE** sizes a sparse grid, discounting duplicates.
-   **SGMG\_SIZE\_TOTAL** sizes a sparse grid, counting duplicates.
-   **SGMG\_UNIQUE\_INDEX** maps nonunique to unique points.
-   **SGMG\_WEIGHT:** sparse grid weights for a mix of 1D rules.
-   **SGMG\_WRITE** writes a sparse grid rule to five files.

You can go up one level to [the C++ source codes](../cpp_src.html).

------------------------------------------------------------------------

*Last revised on 02 July 2013.*