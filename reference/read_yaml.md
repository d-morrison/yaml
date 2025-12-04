# Read a YAML file

Read a YAML document from a file and create an R object from it

## Usage

``` r
read_yaml(
  file,
  fileEncoding = "UTF-8",
  text,
  error.label,
  readLines.warn = TRUE,
  ...
)
```

## Arguments

- file:

  Either a character string naming a file or a
  [connection](https://rdrr.io/r/base/connections.html) open for
  reading.

- fileEncoding:

  Character string: if non-empty declares the encoding used on a file
  (not a connection) so the character data can be re-encoded. See
  [`file()`](https://rdrr.io/r/base/connections.html).

- text:

  Character string: if `file` is not supplied and this is, then data are
  read from the value of `text` via a text connection. Notice that a
  literal string can be used to include (small) data sets within R code.

- error.label:

  A label to prepend to error messages (see Details).

- readLines.warn:

  Logical (default: TRUE). Suppress warnings from readLines used inside
  read_yaml.

- ...:

  Arguments to pass to
  [`yaml.load()`](https://yaml.r-lib.org/reference/yaml.load.md).

## Value

If the root YAML object is a map, a named list or list with an attribute
of 'keys' is returned. If the root object is a sequence, a list or
vector is returned, depending on the contents of the sequence. A vector
of length 1 is returned for single objects.

## Details

This function is a convenient wrapper for
[`yaml.load()`](https://yaml.r-lib.org/reference/yaml.load.md) and is a
nicer alternative to
[`yaml.load_file()`](https://yaml.r-lib.org/reference/yaml.load.md).

You can specify a label to be prepended to error messages via the
`error.label` argument. If `error.label` is missing, `read_yaml` will
make an educated guess for the value of `error.label` by either using
the specified filename (when `file` is a character vector) or using the
description of the supplied connection object (via the `summary`
function). If `text` is used, the default value of `error.label` will be
`NULL`.

## References

YAML: http://yaml.org

libyaml: https://pyyaml.org/wiki/LibYAML

## See also

[`yaml.load()`](https://yaml.r-lib.org/reference/yaml.load.md),
[`write_yaml()`](https://yaml.r-lib.org/reference/write_yaml.md),
[`yaml.load_file()`](https://yaml.r-lib.org/reference/yaml.load.md)

## Author

Jeremy Stephens <jeremy.f.stephens@vumc.org>

## Examples

``` r
if (FALSE) { # \dontrun{
  # reading from a file connection
  filename <- tempfile()
  cat("test: data\n", file = filename)
  con <- file(filename, "r")
  read_yaml(con)
  close(con)

  # using a filename to specify input file
  read_yaml(filename)
} # }

  # reading from a character vector
  read_yaml(text="- hey\n- hi\n- hello")
#> [1] "hey"   "hi"    "hello"
```
