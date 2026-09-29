# The class for unifir prop objects

This function is exported so that developers can add their own props in
new packages, without needing to re-implement the prop and script
classes themselves. It is not expected that end users will need this
function.

## Usage

``` r
unifir_prop(prop_file, method_name, method_type, parameters, build, using)
```

## Arguments

- prop_file:

  The system location for the C# template file

- method_name:

  The name of the method, in C# code

- method_type:

  The type of the method (usually matches its file name); scripts can
  have multiple versions of the same method, each with different
  method_name values, all sharing the same method_type.

- parameters:

  Method-specific parameters, typically used in the build stage.

- build:

  A function that takes three arguments, `script`, `prop`, and `debug`,
  and uses those to construct the C# method.

- using:

  A character vector of imports required for the method.

## Value

An R6 object of class `unifir_prop`

## Details

This function will check each argument for correctness. To be specific,
it performs the following checks:

- `prop_file` must be either a `waiver` object (created by
  [waiver](https://docs.ropensci.org/unifir/reference/waiver.md)) or a
  file path of length 1 pointing to a file that exists

- `method_name` will be automatically generated if not existing. If it
  exists, it must be a character vector of length 1

- `method_type` must be a character vector of length 1

- `build` must be a function with the arguments `script`, `prop`, and
  `debug` (in that order, with no other arguments). Any other arguments
  needed by your build function should be passed as prop parameters.

- `using` must be a character vector (of any length, including 0)

If your prop needs data or arguments beyond these, store them as a list
in `parameters`, which is entirely unchecked.

## The debug argument

When `Sys.getenv(unifir_debugmode)` returns anything other than `""`,
[action](https://docs.ropensci.org/unifir/reference/action.md) runs in
"debug mode". In addition to setting `exec` and `write` to `FALSE` in
[action](https://docs.ropensci.org/unifir/reference/action.md), this
mode also attempts to disable any prop functionality that would make
changes to the user's disk – no files or directories should be altered.
In this mode,
[action](https://docs.ropensci.org/unifir/reference/action.md) will pass
`debug = TRUE` as an argument to your prop; your prop should respect the
debug mode and avoid making any changes.

## Examples

``` r
unifir_prop(
  prop_file = waiver(), # Must be a file that exists or waiver()
  method_name = NULL, # Auto-generated if NULL or NA
  method_type = "ExampleProp", # Length-1 character vector
  parameters = list(), # Not validated, usually a list
  build = function(script, prop, debug) {},
  using = character(0)
)
#> <unifir_prop>
#>   Public:
#>     build: function (script, prop, debug) 
#>     clone: function (deep = FALSE) 
#>     initialize: function (prop_file, method_name, method_type, parameters, build, 
#>     method_name: SpreadOhFamousBird
#>     method_type: ExampleProp
#>     parameters: list
#>     prop_file: waiver
#>     using: 
```
