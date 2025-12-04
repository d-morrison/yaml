# Write a YAML file

Write the YAML representation of an R object to a file

## Usage

``` r
write_yaml(x, file, fileEncoding = "UTF-8", ...)
```

## Arguments

- x:

  The object to be converted.

- file:

  Either a character string naming a file or a
  [connection](https://rdrr.io/r/base/connections.html) open for
  writing.

- fileEncoding:

  Character string: if non-empty declares the encoding to be used on a
  file (not a connection) so the character data can be re-encoded as
  they are written. See
  [`file()`](https://rdrr.io/r/base/connections.html).

- ...:

  Arguments to
  [`as.yaml()`](https://r-yaml.r-lib.org/reference/as.yaml.md).

## Details

If `file` is a non-open connection, an attempt is made to open it and
then close it after use.

This function is a convenient wrapper around
[`as.yaml()`](https://r-yaml.r-lib.org/reference/as.yaml.md).

## See also

[`as.yaml()`](https://r-yaml.r-lib.org/reference/as.yaml.md),
[`read_yaml()`](https://r-yaml.r-lib.org/reference/read_yaml.md),
[`yaml.load_file()`](https://r-yaml.r-lib.org/reference/yaml.load.md)

## Author

Jeremy Stephens <jeremy.f.stephens@vumc.org>

## Examples

``` r
if (FALSE) { # \dontrun{
  # writing to a file connection
  filename <- tempfile()
  con <- file(filename, "w")
  write_yaml(data.frame(a=1:10, b=letters[1:10], c=11:20), con)
  close(con)

  # using a filename to specify output file
  write_yaml(data.frame(a=1:10, b=letters[1:10], c=11:20), filename)
} # }
```
