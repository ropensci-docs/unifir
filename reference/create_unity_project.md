# Create a new Unity project.

Create a new Unity project.

## Usage

``` r
create_unity_project(path, quit = TRUE, unity = NULL)
```

## Arguments

- path:

  The path to create a new Unity project at.

- quit:

  Logical: quit Unity after creating the project?

- unity:

  The path to the Unity executable on your system (importantly, *not*
  the UnityHub executable). If `NULL`, checks to see if the environment
  variable or option `unifir_unity_path` is set; if so, uses that path
  (preferring the environment variable over the option if the two
  disagree).

## Value

TRUE, invisibly.

## See also

Other utilities:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`add_prop()`](https://docs.ropensci.org/unifir/reference/add_prop.md),
[`find_unity()`](https://docs.ropensci.org/unifir/reference/find_unity.md),
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
# \donttest{

if (interactive()) create_unity_project(file.path(tempdir(), "project"))
# }
```
