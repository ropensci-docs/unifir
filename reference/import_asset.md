# Import assets into Unity.

Import assets into Unity.

## Usage

``` r
import_asset(script, asset_path, lazy = TRUE)
```

## Arguments

- script:

  A `unifir_script` object, created by
  [make_script](https://docs.ropensci.org/unifir/reference/make_script.md)
  or returned by an `add_prop_*` function.

- asset_path:

  The file path to the asset to import. If a directory, the entire
  directory will be recursively copied. Note that this function doesn't
  have a `method_name` argument: the `asset_path` is used as the method
  name. This function is not currently vectorized; call it separately
  for each asset you need to import.

- lazy:

  Boolean: if TRUE, unifir will attempt to only copy the files once per
  run of a script; if FALSE, unifir will copy the files as many times as
  requested, overwriting pre-existing files each time.

## Value

`script` with a new prop.

## See also

Other props:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`add_light()`](https://docs.ropensci.org/unifir/reference/add_light.md),
[`add_prop()`](https://docs.ropensci.org/unifir/reference/add_prop.md),
[`add_texture()`](https://docs.ropensci.org/unifir/reference/add_texture.md),
[`create_terrain()`](https://docs.ropensci.org/unifir/reference/create_terrain.md),
[`instantiate_prefab()`](https://docs.ropensci.org/unifir/reference/instantiate_prefab.md),
[`load_png()`](https://docs.ropensci.org/unifir/reference/load_png.md),
[`load_scene()`](https://docs.ropensci.org/unifir/reference/load_scene.md),
[`new_scene()`](https://docs.ropensci.org/unifir/reference/new_scene.md),
[`read_raw()`](https://docs.ropensci.org/unifir/reference/read_raw.md),
[`save_scene()`](https://docs.ropensci.org/unifir/reference/save_scene.md),
[`set_active_scene()`](https://docs.ropensci.org/unifir/reference/set_active_scene.md),
[`validate_path()`](https://docs.ropensci.org/unifir/reference/ValidatePath.md)

## Examples

``` r
# First, create a script object.
# CRAN doesn't have Unity installed, so pass
# a waiver object to skip the Unity-lookup stage:
script <- make_script("example_script",
  unity = waiver()
)

# CRAN also doesn't have any props to install,
# so we'll make a fake prop location:
prop_directory <- file.path(tempdir(), "props")
dir.create(prop_directory)

# Now add props:
script <- import_asset(script, prop_directory)

# Lastly, execute the script via the `action` function
```
