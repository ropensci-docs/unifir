# A waiver object.

This function is borrowed from ggplot2. It creates a "flag" object
indicating that a value has been intentionally left blank (because it
will be filled in by something else). Often, a function argument being
missing or `NULL` will result in an error, while passing `waiver()` will
cause the function to look elsewhere in the script for an acceptable
value.

## Usage

``` r
waiver()
```

## Value

An empty list of class `waiver`.

## References

H. Wickham. ggplot2: Elegant Graphics for Data Analysis. Springer-Verlag
New York, 2016.

## See also

Other utilities:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`add_prop()`](https://docs.ropensci.org/unifir/reference/add_prop.md),
[`create_unity_project()`](https://docs.ropensci.org/unifir/reference/create_unity_project.md),
[`find_unity()`](https://docs.ropensci.org/unifir/reference/find_unity.md),
[`get_asset()`](https://docs.ropensci.org/unifir/reference/get_asset.md),
[`load_png()`](https://docs.ropensci.org/unifir/reference/load_png.md),
[`load_scene()`](https://docs.ropensci.org/unifir/reference/load_scene.md),
[`new_scene()`](https://docs.ropensci.org/unifir/reference/new_scene.md),
[`read_raw()`](https://docs.ropensci.org/unifir/reference/read_raw.md),
[`save_scene()`](https://docs.ropensci.org/unifir/reference/save_scene.md),
[`set_active_scene()`](https://docs.ropensci.org/unifir/reference/set_active_scene.md),
[`validate_path()`](https://docs.ropensci.org/unifir/reference/ValidatePath.md)

## Examples

``` r
waiver()
#> list()
#> attr(,"class")
#> [1] "waiver"
```
