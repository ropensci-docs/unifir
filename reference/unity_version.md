# Print the version of the Unity Editor in use.

Print the version of the Unity Editor in use.

## Usage

``` r
unity_version(unity = NULL)
```

## Arguments

- unity:

  The path to the Unity executable on your system (importantly, *not*
  the UnityHub executable). If `NULL`, checks to see if the environment
  variable or option `unifir_unity_path` is set; if so, uses that path
  (preferring the environment variable over the option if the two
  disagree).

## Value

A character vector of length 1 containing the version of Unity in use.

## Examples

``` r
try(
  unity_version()
)
#> Error in find_unity() : Couldn't find Unity executable at provided path. 
#> Please make sure the path provided to 'unity' is correct.
```
