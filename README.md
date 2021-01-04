# AXF (version 0.3)
Approximation eXchange Format, a.k.a AXF, is a generic file format to import / export mathematical function approximations
It is designed to exchange elementary function implementation between tools, for example between an approximation generation tool (e.g. sollya http://sollya.gforge.inria.fr/) and a function implementation generator tool (e.g. metalibm https://github.com/kalray/metalibm).

# Example
The following code snippet is an extract of a piecewise approximation of tanh(x) over [0.125, 16.125]. Only the first two sub-intervals are listed here.
```
{
    "function": "tanh(_x_)",
    "precision": "float",
    "interval": "[0.125;16.125]",
    "class": "!PieceWiseApprox",
    "approx_error": {"type": "absolute", bound: "5.9604644775390625e-8"},
    "approx_parameter": {
        "even": false,
        "odd": false,
        "max_degree": 7,
        "num_intervals": 2048,
        "indexing": "SubIntervalIndexing([0.125;16.125], 2048)",
    },
    "tag": "",
    "approx_data": [
        {
            "absolute": true,
            "approx_error": "2.7402990785775974270337621608727434791043137206093e-9",
            "class": "!SimplePolyApprox",
            "degree_list": [ 0, 1, 2, 3, 4, 5, 6, 7 ],
            "format_list": [ "float", "float", "float", "float", "float",
                "float", "float", "float" ],
            "function": "tanh(_x_ + 0.125)",
            "interval": "[0.125;0.1328125]",
            "poly": {
                "0": "0.124352999031543731689453125",
                "1": "0.984536349773406982421875",
                "2": "-0.122070901095867156982421875",
                "3": "-0.314812242984771728515625",
                "4": "-11.6888446807861328125",
                "5": "52.44486236572265625",
                "6": "1.101809140625e5",
                "7": "-4.4541990625e5"
            }
        },
    ...
    ]
}
```
# Specification

This section describes the syntax of an **AXF** file.

Each file contain a list of approximations (one to many).

Each approximation is a map containing at least the following fields:
- **interval** which stipulates the range where the approximation is valid (e.g. `"[0.125;1.0]"`)
- **approx_error** field is a map describing the approximation error, it has 2 fields:
    - **type** which indicates which type of error is considered (e.g. `"absolute"`)
    - **bound** which gives an upper bound on the approximation error (.e.g `"0x1p-23"`)
- **function** which gives the expression being approximated (e.g. `"tanh(_x_)"`)
- **class** which indicates which class of approximation is described (e.g. `"!SimplePolyApprox"`)
- **precision** which indicates the assumed precision for the approximation results
- **approx_data** which describes the approximation

Other fields are optional:
- **tag** which permits to give a comprehensive name to an approximation (e.g. `"tanh-fp32"`)
- **approx_param** which is a class-specific map describing the parameter of the approximation instance

### Numerical values and their derivatives

integers are stored by their decimal encoding (e.g. two is 2).

Other numerical values are encoded by strings starting and finishing with character '.
Multiple encodings are accepted:
- decimal notation (e.g. `"0.125"`)
- scientific notation (e.g. `"1.6e-7"`)
- hexadecimal notation (e.g. `"-0x1.8p-1"` for -0.75)

Intervals are encoded by `"[inf_bound;sup_bound]"`, where `inf_bound` (respectively `sup_bound`) is the numerical value encoding the lower (resp. upper) bound of the interval (with quote omitted within `[]`).

boolean are encoded by *true* or *false*

### Numerical formats

The supported formats are: `"float"`, `"double"`, `"floatfloat"`, `"doubledouble"`.

### Polynomial

A map with `"class": "!Polynomial"` with a single other field `"coeff_map"`.
`"coeff_map"` is a map of coefficient index -> coefficient value.
coefficient index are quoted integers, coefficient values are Numerical values.

```
{
    "class": "!Polynomial",
    "coeff_map": {
        "0": "0x1.fd5992p-4", "1": "0x1.f81526p-1", "2": "-0x1.f4009ep-4",
        "3": "-0x1.425e24p-2", "4": "-0x1.760b04p3", "5": "0x1.a38f14p5",
        "6": "0x1.ae64eap16", "7": "-0x1.b2fafap18"
    }
}
```
### Simple Polynomial approximation

A map with `"class": "!SimplePolyApprox"` with the following fields:
- `"absolute`: boolean indicating if the error is absolute (`true`)or relative (`false"`)
- `"approx_error"`: string-encoded numerical value, indicating an upper bound the approximation error
- `"degree_list"`: list of integer values indicating which were the non-zero index used to generate the polynomial approximation
- `"format_list"`: list of numerical formats used to store the polynomial coefficients
- `"interval"`: validity interval for the approximation
- `"poly"`: actual approximation as a Polynomial object
- `"function"`: approximated function expression

### Simple Piecewise approximation

A map with `"class": "!PieceWiseApprox"` with the following fields:
- `"interval"`: approximation interval
- `"error_bound"`: upper bound of the approximation error
- `"indexing"`: indexing function used to build sub-interval list and to index within it
- `"even"`: boolean, are even-indexed coefficients allowed in the polynomial sub-approximations
- `"odd"`: boolean, are odd-indexed coefficients allowed in the polynomial sub-approximations
- `"function"`: function expression being approximated
- `"max_degree"`: maximum polynomial degree accepted for a sub-approximation
- `"num_intervals"`: integer, number of sub-intervals dividing the full approximation interval
- `"precision"`: format of the approximation's result
- `"approx_list"`: ordered list of sub-approximation (one per sub-interval)

### approximated fct

The syntax for the `"function"` field expression accepts the following:

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



