# Create a new scene in a Unity project.

Create a new scene in a Unity project.

## Usage

``` r
new_scene(
  script,
  setup = c("EmptyScene", "DefaultGameObjects"),
  mode = c("Additive", "Single"),
  method_name = NULL,
  exec = TRUE
)
```

## Arguments

- script:

  A `unifir_script` object, created by
  [make_script](https://docs.ropensci.org/unifir/reference/make_script.md)
  or returned by an `add_prop_*` function.

- setup:

  One of "EmptyScene" ("No game objects are added to the new Scene.") or
  "DefaultGameObjects" ("Adds default game objects to the new Scene (a
  light and camera).")

- mode:

  One of "Additive" ("The newly created Scene is added to the current
  open Scenes.") or "Single" ("All current open Scenes are closed and
  the newly created Scene are opened.")

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
[`read_raw()`](https://docs.ropensci.org/unifir/reference/read_raw.md),
[`save_scene()`](https://docs.ropensci.org/unifir/reference/save_scene.md),
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
[`read_raw()`](https://docs.ropensci.org/unifir/reference/read_raw.md),
[`save_scene()`](https://docs.ropensci.org/unifir/reference/save_scene.md),
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
script <- new_scene(script)

# Lastly, execute the script via the `action` function
```
