# Add a prefab to a Unity scene

This function creates objects (specifically, prefabs) within a Unity
scene. This function is vectorized over all functions from `prefab_path`
through `z_rotation`; to add multiple objects, simply provide vectors to
each argument. Note that all arguments will be automatically recycled if
not the same length; this may produce undesired results. This function
is only capable of altering a single scene at once – call the function
multiple times if you need to manipulate multiple scenes.

## Usage

``` r
instantiate_prefab(
  script,
  method_name = NULL,
  destination_scene = NULL,
  prefab_path,
  x_position = 0,
  y_position = 0,
  z_position = 0,
  x_scale = 1,
  y_scale = 1,
  z_scale = 1,
  x_rotation = 0,
  y_rotation = 0,
  z_rotation = 0,
  exec = TRUE
)
```

## Arguments

- script:

  A `unifir_script` object, created by
  [make_script](https://docs.ropensci.org/unifir/reference/make_script.md)
  or returned by an `add_prop_*` function.

- method_name:

  The internal name to use for the C# method created. Will be randomly
  generated if not set.

- destination_scene:

  Optionally, the scene to instantiate the prefabs in. Ignored if NULL,
  the default.

- prefab_path:

  File path to the prefab to be instantiated. This should be relative to
  the Unity project root directory, and likely begins with "Assets".
  Alternatively, if this is one of the elements in

- x_position, y_position, z_position:

  The position of the GameObject in world space.

- x_scale, y_scale, z_scale:

  The scale of the GameObject (relative to its parent object).

- x_rotation, y_rotation, z_rotation:

  The rotation of the GameObject to create, as Euler angles.

- exec:

  Logical: Should the C# method be included in the set executed by
  MainFunc?

## See also

Other props:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`add_light()`](https://docs.ropensci.org/unifir/reference/add_light.md),
[`add_prop()`](https://docs.ropensci.org/unifir/reference/add_prop.md),
[`add_texture()`](https://docs.ropensci.org/unifir/reference/add_texture.md),
[`create_terrain()`](https://docs.ropensci.org/unifir/reference/create_terrain.md),
[`import_asset()`](https://docs.ropensci.org/unifir/reference/import_asset.md),
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
script <- make_script("example_script", unity = waiver())

# Now add props:
script <- instantiate_prefab(script, prefab_path = "Assets/some.prefab")

# Lastly, execute the script via the `action` function
```
