# yaml

yaml provides R bindings to [libyaml](https://pyyaml.org/wiki/LibYAML),
a fast [YAML](https://yaml.org/) parser and emitter.

## Installation

Install from CRAN:

``` r
install.packages("yaml")
```

Or install the development version from GitHub:

``` r
# install.packages("pak")
pak::pak("r-lib/r-yaml")
```

## Usage

``` r
library(yaml)
```

Parse YAML with
[`yaml.load()`](https://yaml.r-lib.org/reference/yaml.load.md) or
[`read_yaml()`](https://yaml.r-lib.org/reference/read_yaml.md):

``` r
yaml.load(
  "
- 1
- 2
- 3
"
)
#> [1] 1 2 3

yaml.load(
  "
a: 1
b: 2
"
)
#> $a
#> [1] 1
#> 
#> $b
#> [1] 2
```

Convert R objects to YAML with
[`as.yaml()`](https://yaml.r-lib.org/reference/as.yaml.md) or
[`write_yaml()`](https://yaml.r-lib.org/reference/write_yaml.md):

``` r
cat(as.yaml(list(a = 1:3, b = 4:6)))
#> a:
#> - 1
#> - 2
#> - 3
#> b:
#> - 4
#> - 5
#> - 6
```

See [`vignette("yaml")`](https://yaml.r-lib.org/articles/yaml.md) for
more details on handlers, formatting options, and advanced usage.
