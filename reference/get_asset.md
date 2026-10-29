# Download prefabs for Unity

This is a simple helper function downloading the assets stored at
https://github.com/mikemahoney218/unity_assets .

## Usage

``` r
get_asset(asset, directory = NULL)
```

## Arguments

- asset:

  The asset to download. Available asset names are provided in
  [available_assets](https://docs.ropensci.org/unifir/reference/available_assets.md).

- directory:

  Optionally, the directory to extract the downloaded models in. If
  NULL, the default, saves to `tools::R_user_dir("unifir")`.

## See also

Other utilities:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`add_prop()`](https://docs.ropensci.org/unifir/reference/add_prop.md),
[`create_unity_project()`](https://docs.ropensci.org/unifir/reference/create_unity_project.md),
[`find_unity()`](https://docs.ropensci.org/unifir/reference/find_unity.md),
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
  get_asset(asset = "tree_1", directory = tempdir())
}
```
