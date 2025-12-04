# Convert a YAML string into R objects

Parse a YAML string and return R objects.

## Usage

``` r
yaml.load(
  string,
  as.named.list = TRUE,
  handlers = NULL,
  error.label = NULL,
  eval.expr = getOption("yaml.eval.expr", FALSE),
  merge.precedence = c("order", "override"),
  merge.warning = FALSE
)

yaml.load_file(input, error.label, readLines.warn = TRUE, ...)
```

## Arguments

- string:

  The YAML string to be parsed.

- as.named.list:

  Whether or not to return a named list for maps (TRUE by default).

- handlers:

  Named list of custom handler functions for YAML types (see Details).

- error.label:

  A label to prepend to error messages (see Details).

- eval.expr:

  Whether or not to evaluate expressions found in the YAML document (see
  Details).

- merge.precedence:

  Precedence behavior during map merges (see Details).

- merge.warning:

  Whether or not to warn about ignored key/value pairs during map
  merges.

- input:

  A filename or connection; if `input` is a filename, that file must be
  encoded in UTF-8.

- readLines.warn:

  Logical (default: TRUE). Suppress warnings from readLines used inside
  read_yaml.

- ...:

  Arguments to pass to yaml.load.

## Value

If the root YAML object is a map, a named list or list with an attribute
of 'keys' is returned. If the root object is a sequence, a list or
vector is returned, depending on the contents of the sequence. A vector
of length 1 is returned for single objects.

## Details

Use `yaml.load` to load a YAML string. For files and connections, use
`yaml.load_file`, which calls `yaml.load` with the contents of the
specified file or connection.

Sequences of uniform data (e.g. a sequence of integers) are converted
into vectors. If the sequence is not uniform, it's returned as a list.
Maps are converted into named lists by default, and all the keys in the
map are converted to strings. If you don't want the keys to be coerced
into strings, set `as.named.list` to FALSE. When it's FALSE, a list will
be returned with an additional attribute named 'keys', which is a list
of the un-coerced keys in the map (in the same order as the main list).

You can specify custom handler functions via the `handlers` argument.
This argument must be a named list of functions, where the names are the
YAML types (i.e., 'int', 'float', 'seq', etc). The functions you provide
will be passed one argument. Custom handler functions for string types
(all types except sequence and map) will receive a character vector of
length 1. Custom sequence functions will be passed a list of objects.
Custom map functions will be passed the object that the internal map
handler creates, which is either a named list or a list with a 'keys'
attribute (depending on `as.named.list`). ALL functions you provide must
return an object. See the examples for custom handler use.

You can specify a label to be prepended to error messages via the
`error.label` argument. When using `yaml.load_file`, you can either set
the `error.label` argument explicitly or leave it missing. If missing,
`yaml.load_file` will make an educated guess for the value of
`error.label` by either using the specified filename (when `input` is a
character vector) or using the description of the supplied connection
object (via the `summary` function). You can explicitly set
`error.label` to `NULL` if you don't want to use this functionality.

There is a built-in handler that will evaluate expressions that are
tagged with the ‘!expr’ tag. Currently this handler is disabled by
default for security reasons. If a ‘!expr’ tag exists and this is set to
FALSE a warning will occur. Alternately, you can set the option named
‘yaml.eval.expr’ via the `options` function to turn on evaluation.

The `merge.precedence` parameter controls how merge keys are handled.
The YAML merge key specification is not specific about how key/value
conflicts are resolved during map merges. As a result, various YAML
library implementations vary in merge key behavior (notably Python and
Ruby). This package's default behavior (when `merge.precedence` is
‘order’) is to give precedence to key/value pairs that appear first. If
you set `merge.precedence` to ‘override’, natural map key/value pairs
will override any duplicate keys found in merged maps, regardless of
order. This is the default behavior in Python's YAML library.

This function uses the YAML parser provided by libyaml, which conforms
to the YAML 1.1 specification.

## References

YAML: http://yaml.org

libyaml: https://pyyaml.org/wiki/LibYAML

YAML merge specification: http://yaml.org/type/merge.html

## See also

[`as.yaml()`](https://r-yaml.r-lib.org/reference/as.yaml.md)

## Author

Jeremy Stephens <jeremy.f.stephens@vumc.org>

## Examples

``` r
  yaml.load("- hey\n- hi\n- hello")
#> [1] "hey"   "hi"    "hello"
  yaml.load("foo: 123\nbar: 456")
#> $foo
#> [1] 123
#> 
#> $bar
#> [1] 456
#> 
  yaml.load("- foo\n- bar\n- 3.14")
#> [[1]]
#> [1] "foo"
#> 
#> [[2]]
#> [1] "bar"
#> 
#> [[3]]
#> [1] 3.14
#> 
  yaml.load("foo: bar\n123: 456", as.named.list = FALSE)
#> [[1]]
#> [1] "bar"
#> 
#> [[2]]
#> [1] 456
#> 
#> attr(,"keys")
#> attr(,"keys")[[1]]
#> [1] "foo"
#> 
#> attr(,"keys")[[2]]
#> [1] 123
#> 

if (FALSE) { # \dontrun{
  # reading from a file (uses readLines internally)
  filename <- tempfile()
  cat("foo: 123", file=filename, sep="\n")
  yaml.load_file(filename)
} # }

  # custom scalar handler
  my.float.handler <- function(x) { as.numeric(x) + 123 }
  yaml.load("123.456", handlers=list("float#fix"=my.float.handler))
#> [1] 246.456

  # custom sequence handler
  yaml.load("- 1\n- 2\n- 3", handlers=list(seq=function(x) { as.integer(x) + 3 }))
#> [1] 4 5 6

  # custom map handler
  yaml.load("foo: 123", handlers=list(map=function(x) { x$foo <- x$foo + 123; x }))
#> $foo
#> [1] 246
#> 

  # handling custom types
  yaml.load("!sqrt 555", handlers=list(sqrt=function(x) { sqrt(as.integer(x)) }))
#> [1] 23.55844
  yaml.load("!foo\n- 1\n- 2", handlers=list(foo=function(x) { as.integer(x) + 1 }))
#> [1] 2 3
  yaml.load("!bar\none: 1\ntwo: 2", handlers=list(bar=function(x) { x$one <- "one"; x }))
#> $one
#> [1] "one"
#> 
#> $two
#> [1] 2
#> 

  # loading R expressions
  # NOTE: this will not be done by default in the near future
  doc <- yaml.load("inc: !expr function(x) x + 1", eval.expr=TRUE)
  doc$inc(1)
#> [1] 2

  # adding a label to error messages
  try(yaml.load("*", error.label = "foo"))
#> Error in yaml.load("*", error.label = "foo") : 
#>   (foo) Scanner error: while scanning an alias at line 1, column 1 did not find expected alphabetic or numeric character at line 1, column 2
#> 
```
