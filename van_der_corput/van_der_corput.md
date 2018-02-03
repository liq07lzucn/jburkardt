VAN\_DER\_CORPUT\
The van der Corput\
Quasi Monte Carlo (QMC) sequence {#van_der_corput-the-van-der-corput-quasi-monte-carlo-qmc-sequence align="center"}
================================

------------------------------------------------------------------------

**VAN\_DER\_CORPUT** is a C++ library which computes the van der Corput
Quasi Monte Carlo (QMC) sequence, using a simple interface.

A more sophisticated library is available in VAN\_DER\_CORPUT\_ADVANCED,
but I find this simple version to be preferable for everyday use!

The van der Corput sequence generates a sequence of points in \[0,1\]
which never repeats. For positive index I, the elements of the van der
Corput sequence are strictly between 0 and 1.

The I-th element of the van der Corput sequence is computed by writing I
in the base B (usually 2) and then reflecting its digits about the
decimal point. For example, if we start with I = 11, its binary
expansion is 1011, and so its reflected binary expansion is 0.1101 which
is 1/2+1/4+1/16=0.8125.

The generation is quite simple. Given an index I, the expansion of I in
base B is generated. Then, essentially, the result R is generated by
writing a decimal point followed by the digits of the expansion of I, in
reverse order. This decimal value is actually still in base B, so it
must be properly interpreted to generate a usable value.

Here is an example in base 2:

  I (decimal)   I (binary)   R (binary)   R (decimal)
  ------------- ------------ ------------ -------------
  0             0            .0           0.0
  1             1            .1           0.5
  2             10           .01          0.25
  3             11           .11          0.75
  4             100          .001         0.125
  5             101          .101         0.625
  6             110          .011         0.375
  7             111          .111         0.875
  8             1000         .0001        0.0625

### Licensing: {#licensing align="center"}

The computer code and data files described and made available on this
web page are distributed under [the GNU LGPL
license.](../../txt/gnu_lgpl.txt)

### Languages: {#languages align="center"}

**VAN\_DER\_CORPUT** is available in [a C
version](../../c_src/van_der_corput/van_der_corput.html) and [a C++
version](../../cpp_src/van_der_corput/van_der_corput.html) and [a
FORTRAN90 version](../../f_src/van_der_corput/van_der_corput.html) and
[a MATLAB version](../../m_src/van_der_corput/van_der_corput.html) and
[a Python version](../../py_src/van_der_corput/van_der_corput.html).

### Related Data and Programs: {#related-data-and-programs align="center"}

[BOX\_BEHNKEN](../../cpp_src/box_behnken/box_behnken.html), a C++
library which computes a Box-Behnken design, that is, a set of arguments
to sample the behavior of a function of multiple parameters;

[CVT](../../cpp_src/cvt/cvt.html), a C++ library which computes points
in a Centroidal Voronoi Tessellation.

[FAURE](../../cpp_src/faure/faure.html), a C++ library which computes
Faure sequences.

[GRID](../../cpp_src/grid/grid.html), a C++ library which computes
points on a grid.

[HALTON](../../cpp_src/halton/halton.html), a C++ library which computes
elements of a Halton Quasi Monte Carlo (QMC) sequence, using a simple
interface.

[HAMMERSLEY](../../cpp_src/hammersley/hammersley.html), a C++ library
which computes elements of a Hammersley Quasi Monte Carlo (QMC)
sequence, using a simple interface.

[HEX\_GRID](../../cpp_src/hex_grid/hex_grid.html), a C++ library which
computes sets of points in a 2D hexagonal grid.

[IHS](../../cpp_src/ihs/ihs.html), a C++ library which computes improved
Latin Hypercube datasets.

[LATIN\_CENTER](../../cpp_src/latin_center/latin_center.html), a C++
library which computes Latin square data choosing the center value.

[LATIN\_EDGE](../../cpp_src/latin_edge/latin_edge.html), a C++ library
which computes Latin square data choosing the edge value.

[LATIN\_RANDOM](../../cpp_src/latin_random/latin_random.html), a C++
library which computes Latin square data choosing a random value in the
square.

[NIEDERREITER2](../../cpp_src/niederreiter2/niederreiter2.html), a C++
library which computes Niederreiter sequences with base 2.

[NORMAL](../../cpp_src/normal/normal.html), a C++ library which computes
a sequence of pseudorandom normally distributed values.

[SEQUENCE\_STREAK\_DISPLAY](../../m_src/sequence_streak_display/sequence_streak_display.html),
a MATLAB program which makes a "streak file" of a sequence.

[SOBOL](../../cpp_src/sobol/sobol.html), a C++ library which computes
Sobol sequences.

[UNIFORM](../../cpp_src/uniform/uniform.html), a C++ library which
computes uniform random values.

[VAN\_DER\_CORPUT](../../datasets/van_der_corput/van_der_corput.html), a
dataset directory which contains datasets of van der Corput sequences.

[VAN\_DER\_CORPUT\_ADVANCED](../../cpp_src/van_der_corput_advanced/van_der_corput_advanced.html),
a C++ library which computes elements of a 1D van der Corput Quasi Monte
Carlo (QMC) sequence, allowing the user more advanced and sophisticated
input.

### Reference: {#reference align="center"}

1.  J G van der Corput,\
    Verteilungsfunktionen I & II,\
    Nederl. Akad. Wetensch. Proc.,\
    Volume 38, 1935, pages 813-820, pages 1058-1066.

### Source Code: {#source-code align="center"}

-   [van\_der\_corput.cpp](van_der_corput.cpp), the source code.
-   [van\_der\_corput.hpp](van_der_corput.hpp), the include file.

### Examples and Tests: {#examples-and-tests align="center"}

-   [van\_der\_corput\_prb.cpp](van_der_corput_prb.cpp), a sample
    calling program.
-   [van\_der\_corput\_prb\_output.txt](van_der_corput_prb_output.txt),
    the output file.

### List of Routines: {#list-of-routines align="center"}

-   **R8VEC\_TRANSPOSE\_PRINT** prints an R8VEC "transposed".
-   **TIMESTAMP** prints the current YMDHMS date as a time stamp.
-   **VDC\_BASE** computes an element of the van der Corput sequence in
    any base.
-   **VDC\_INVERSE** inverts an element of the van der Corput sequence.
-   **VDC** computes an element of the van der Corput sequence.
-   **VDC\_SEQUENCE** computes a sequence of elements of the van der
    Corput sequence.

You can go up one level to [the C++ source codes](../cpp_src.html).

------------------------------------------------------------------------

*Last revised on 10 August 2016.*