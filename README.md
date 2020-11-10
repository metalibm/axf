# axf
Approximation eXchange Format, generic file format to import / export mathematical function approximations

# Example
```
!PieceWiseApprox
bound_high: '16.125'
bound_low: '0.125'
error_threshold: '5.9604644775390625e-8'
even: false
function: tanh(_x_)
max_degree: 7
num_intervals: 2048
odd: false
approx_list:
- !SimplePolyApprox
  absolute: true
  approx_error: '2.7402990785775974270337621608727434791043137206093e-9'
  degree_list: [0, 1, 2, 3, 4, 5, 6, 7]
  fct: tanh(_x_ + 0.125)
  format_list: [float, float, float, float, float, float, float, float]
  interval: '[0.125;0.1328125]'
  poly: !Polynomial
    coeff_map: {0: 0x1.fd5992p-4, 1: 0x1.f81526p-1, 2: -0x1.f4009ep-4, 3: -0x1.425e24p-2,
      4: -0x1.760b04p3, 5: 0x1.a38f14p5, 6: 0x1.ae64eap16, 7: -0x1.b2fafap18}
- !SimplePolyApprox
  absolute: true
  approx_error: '2.9820045200028283815690142383890874707928188475846e-9'
  degree_list: [0, 1, 2, 3, 4, 5, 6, 7]
  fct: tanh(_x_ + 0.1328125)
  format_list: [float, float, float, float, float, float, float, float]
  interval: '[0.1328125;0.140625]'
  poly: !Polynomial
    coeff_map: {0: 0x1.0e6974p-3, 1: 0x1.f712ecp-1, 2: -0x1.0a7f7cp-3, 3: -0x1.40643ep-2,
      4: 0x1.9c7c5ap3, 5: 0x1.17a958p6, 6: -0x1.d45536p16, 7: -0x1.21218cp19}
```
# Specification

### Numerical values and their derivatives

integers are stored by their decimal encoding (e.g. two is 2).

Other numerical values are encoded by strings starting and finishing with character '.
Multiple encoding are accepted:
- decimal notation (e.g. '0.125')
- scientific notation (e.g. '1.6e-7)
- hexadecimal notation (e.g. '0x1.ap-3')

Intervals are encoded by '[inf_bound;sup_bound]'

boolean are encoded by *true* or *false*

### Numerical formats

The supported formats are: `float`, `double`.

### Polynomial

user-defined yaml class `!Polynomial` with a single field `coeff_field`

### Simple Polynomial approximation

user-defined yaml class `!SimplePolyApprox` with the following fields:
- `absolute`: boolean indicating if the error is absolute (`true`)or relative (`false`)
- `approx_error`: string-encoded numerical value, indicating an upper bound the approximation error
- `degree_list`: list of integer values indicating which were the non-zero index used to generate the polynomial approximation
- `format_list`: list of numerical formats used to store the polynomial coefficients
- `interval`: validity interval for the approximation
- `poly`: actual approximation as a Polynomial object
- `function`: approximated function expression

### Simple Piecewise approximation

used-defined yaml class `!PieceWiseApprox`, with the following fields:
- `bound_high`: upper bound of the approximation interval
- `bound_low`: lower bound of the approximation interval
- `error_threshold`: upper bound of the approximation error
- `even`: boolean, are even-indexed coefficients allowed in the polynomial sub-approximations
- `odd`: boolean, are odd-indexed coefficients allowed in the polynomial sub-approximations
- `function`: function expression being approximated
- `max_degree`: maximum polynomial degree accepted for a sub-approximation
- `num_intervals`: integer, number of sub-intervals dividing the full approximation interval
- `approx_list`: ordered list of sub-approximation (one per sub-interval)

### approximated fct

The syntax for the `function` field expression accepts the following:

Free variables(:) `_x_`
Basic operations: `+`, `-`, `*`, `/`
Elementary functions:
- `exp`, `exp2`, `exp10`, `expm1`
- `log`, `log2`, `log10`, `log1p`
- `sin`, `cos`, `tan`
- `asin`, `acos`, `atan`
- `sinh`, `cosh`, `tanh`
- `erf`
- `gamma`

### error_target
### description

#### Polynomials

