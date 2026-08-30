# Add a Texture2D layer to a terrain tile object

This function adds a helper method, `AddTexture`, to the C# script. This
function is typically used to add textures to heightmaps in a Unity
scene, for instance by functions like
[create_terrain](https://docs.ropensci.org/unifir/reference/create_terrain.md).
It requires some arguments be provided at the C# level, and so is almost
always called with `exec = FALSE`.

## Usage

``` r
add_texture(script, method_name = NULL, exec = FALSE)
```

## Arguments

- script:

  A `unifir_script` object, created by
  [make_script](https://docs.ropensci.org/unifir/reference/make_script.md)
  or returned by an `add_prop_*` function.

- method_name:

  The internal name to use for the C# method created. Will be randomly
  generated if not set.

- exec:

  Logical: Should the C# method be included in the set executed by
  MainFunc?

## Value

The `unifir_script` object passed to `script`, with an `AddTexture`
method appended.

## See also

Other props:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`add_light()`](https://docs.ropensci.org/unifir/reference/add_light.md),
[`add_prop()`](https://docs.ropensci.org/unifir/reference/add_prop.md),
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

## Examples

``` r
# First, create a script object.
# CRAN doesn't have Unity installed, so pass
# a waiver object to skip the Unity-lookup stage:
script <- make_script("example_script",
  unity = waiver()
)

# Now add props:
script <- add_texture(script)

# Lastly, execute the script via the `action` function
```
