# axf
Approximation eXchange Format, generic file format to import / export mathematical function approximations

# Example
```
approximated_fct: exp(x)
approximation_range: Interval(0, 1)
error_target: dar(2**-25)
description:
  - subapproximation:
        type: piecewise_polynomial_uniform_deg
        sub_approx_range: [0, 0.5]
        sub_approx: Polynomial([0, 1])
  - subapproximation:
        type: piecewise_polynomial_uniform_deg
        sub_approx_range: [0.5, 1.0]
        sub_approx: Polynomial([0, 1])
```
# Specification

### approximated_fct

Free variables: `x`, `y`, `z`, `t`
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

