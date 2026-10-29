# Save a scene in a Unity project.

Save a scene in a Unity project.

## Usage

``` r
save_scene(script, scene_name = NULL, method_name = NULL, exec = TRUE)
```

## Arguments

- script:

  A `unifir_script` object, created by
  [make_script](https://docs.ropensci.org/unifir/reference/make_script.md)
  or returned by an `add_prop_*` function.

- scene_name:

  The name to save the scene to.

- method_name:

  The internal name to use for the C# method created. Will be randomly
  generated if not set.

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
[`instantiate_prefab()`](https://docs.ropensci.org/unifir/reference/instantiate_prefab.md),
[`load_png()`](https://docs.ropensci.org/unifir/reference/load_png.md),
[`load_scene()`](https://docs.ropensci.org/unifir/reference/load_scene.md),
[`new_scene()`](https://docs.ropensci.org/unifir/reference/new_scene.md),
[`read_raw()`](https://docs.ropensci.org/unifir/reference/read_raw.md),
[`set_active_scene()`](https://docs.ropensci.org/unifir/reference/set_active_scene.md),
[`validate_path()`](https://docs.ropensci.org/unifir/reference/ValidatePath.md)

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
[`set_active_scene()`](https://docs.ropensci.org/unifir/reference/set_active_scene.md),
[`validate_path()`](https://docs.ropensci.org/unifir/reference/ValidatePath.md),
[`waiver()`](https://docs.ropensci.org/unifir/reference/waiver.md)

## Examples

``` r
# First, create a script object.
# CRAN doesn't have Unity installed, so pass
# a waiver object to skip the Unity-lookup stage:
script <- make_script("example_script",
  unity = waiver()
)

# Now add props:
script <- save_scene(script, scene_name = "some_scene")

# Lastly, execute the script via the `action` function
```
