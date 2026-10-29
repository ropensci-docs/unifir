# Add a prop to a unifir script

This function is exported so that developers can add their own props in
new packages, without needing to re-implement the prop and script
classes themselves. It is not expected that end users will need this
function.

## Usage

``` r
add_prop(script, prop, exec = TRUE)
```

## Arguments

- script:

  A script object (from
  [make_script](https://docs.ropensci.org/unifir/reference/make_script.md))
  to append the prop to.

- prop:

  A `unifir_prop` object (from
  [unifir_prop](https://docs.ropensci.org/unifir/reference/unifir_prop.md))
  to add to the script.

- exec:

  Logical: Should the method created by the prop be called in the
  MainFunc method?

## See also

Other props:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`add_light()`](https://docs.ropensci.org/unifir/reference/add_light.md),
[`add_texture()`](https://docs.ropensci.org/unifir/reference/add_texture.md),
[`create_terrain()`](https://docs.ropensci.org/unifir/reference/create_terrain.md),
[`import_asset()`](https://docs.ropensci.org/unifir/reference/import_asset.md),
[`instantiate_prefab()`](https://docs.ropensci.org/unifir/reference/instantiate_prefab.md),
[`load_png()`](https://docs.ropensci.org/unifir/reference/load_png.md),
[`load_scene()`](https://docs.ropensci.org/unifir/reference/load_scene.md),
[`new_scene()`](https://docs.ropensci.org/unifir/reference/new_scene.md),
[`read_raw()`](https://docs.ropensci.org/unifir/reference/read_raw.md),
[`save_scene()`](https://docs.ropensci.org/unifir/reference/save_scene.md),
[`set_active_scene()`](https://docs.ropensci.org/unifir/reference/set_active_scene.md),
[`validate_path()`](https://docs.ropensci.org/unifir/reference/ValidatePath.md)

Other utilities:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`create_unity_project()`](https://docs.ropensci.org/unifir/reference/create_unity_project.md),
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
script <- make_script("example_script", unity = waiver())
prop <- unifir_prop(
  prop_file = waiver(), # Must be a file that exists or waiver()
  method_name = NULL, # Auto-generated if NULL or NA
  method_type = "ExampleProp", # Length-1 character vector
  parameters = list(), # Not validated, usually a list
  build = function(script, prop, debug) {},
  using = character(0)
)
script <- add_prop(script, prop)
```
