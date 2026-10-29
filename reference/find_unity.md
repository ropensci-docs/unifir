# Find the Unity executable on a machine.

If the path to Unity is not provided to a function, this function is
invoked to attempt to find it. To do so, it goes through the following
steps:

1.  Attempt to load the "unifir_unity_path" environment variable.

2.  Attempt to load the "unifir_unity_path" option.

Assuming that neither points to an actual file, this function will then
check the default installation paths for Unity on the user's operating
system. If not found, this function will error.

## Usage

``` r
find_unity(unity = NULL, check_path = TRUE)
```

## Arguments

- unity:

  Character: If provided, this function will quote the provided string
  (if necessary) and return it.

- check_path:

  Logical: If `TRUE`, this function will check if the Unity executable
  provided as an argument, environment variable, or option exists. If it
  does not, this function will then attempt to find one, and will error
  if not found. If `FALSE`, this function will never error.

## Value

The path to the Unity executable on the user's machine, as a length-1
character vector.

## See also

Other utilities:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`add_prop()`](https://docs.ropensci.org/unifir/reference/add_prop.md),
[`create_unity_project()`](https://docs.ropensci.org/unifir/reference/create_unity_project.md),
[`get_asset()`](https://docs.ropensci.org/unifir/reference/get_asset.md),
[`load_png()`](https://docs.ropensci.org/unifir/reference/load_png.md),
[`load_scene()`](https://docs.ropensci.org/unifir/reference/load_scene.md),
[`new_scene()`](https://docs.ropensci.org/unifir/reference/new_scene.md),
[`read_raw()`](https://docs.ropensci.org/unifir/reference/read_raw.md),
[`save_scene()`](https://docs.ropensci.org/unifir/reference/save_scene.md),
[`set_active_scene()`](https://docs.ropensci.org/unifir/reference/set_active_scene.md),
[`validate_path()`](https://docs.ropensci.org/unifir/reference/ValidatePath.md),
[`waiver()`](https://docs.ropensci.org/unifir/reference/waiver.md)

## Examples

``` r
if (interactive()) {
  try(find_unity())
}
```
