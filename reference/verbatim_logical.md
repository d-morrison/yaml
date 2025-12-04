# Alternative logical handler

A yaml handler function that causes logical vectors to emit
`true`/`false` instead of `yes`/`no` values.

## Usage

``` r
verbatim_logical(x)
```

## Arguments

- x:

  Logical vector to convert to `true`/`false`.

## Value

Returns a vector of strings of either `true` or `false` of class
`verbatim`.

## Details

Pass this function to
[`as.yaml()`](https://yaml.r-lib.org/reference/as.yaml.md) as part of
the `handlers` argument list like `list(logical = verbatim_logical)`.

## See also

[`as.yaml()`](https://yaml.r-lib.org/reference/as.yaml.md)

## Author

Charles Dupont and James Goldie (jimjam-slam)

## Examples

``` r
vector <- c(TRUE, FALSE, TRUE)

as.yaml(vector, handlers=list(logical=verbatim_logical))
#> [1] "- true\n- false\n- true\n"
```
