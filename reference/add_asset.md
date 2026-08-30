# Add assets to a Unity scene

These functions add assets available at
https://github.com/mikemahoney218/unity_assets/ to a Unity scene.

## Usage

``` r
add_default_player(
  script,
  controller = c("Player", "FootstepsPlayer", "JetpackPlayer", "Third Person"),
  asset_directory = NULL,
  lazy = TRUE,
  method_name = NULL,
  destination_scene = NULL,
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

add_default_tree(
  script,
  tree,
  asset_directory = NULL,
  lazy = TRUE,
  method_name = NULL,
  destination_scene = NULL,
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

- controller:

  Which controller to use. "Player", the default, is a simple
  first-person controller. "FootstepsPlayer" adds footsteps to this
  controller, while "JetpackPlayer" adds a "jetpack" with limited fuel.
  ""Third Person" lets you control a small cylinder in third person.

- asset_directory:

  A file path to the directory containing the asset, or alternatively,
  to which the default assets should be saved. Defaults to
  `tools::R_user_dir("unifir")`.

- lazy:

  Boolean: if TRUE, unifir will attempt to only copy the files once per
  run of a script; if FALSE, unifir will copy the files as many times as
  requested, overwriting pre-existing files each time.

- method_name:

  The internal name to use for the C# method created. Will be randomly
  generated if not set.

- destination_scene:

  Optionally, the scene to instantiate the prefabs in. Ignored if NULL,
  the default.

- x_position, y_position, z_position:

  The position of the GameObject in world space.

- x_scale, y_scale, z_scale:

  The scale of the GameObject (relative to its parent object).

- x_rotation, y_rotation, z_rotation:

  The rotation of the GameObject to create, as Euler angles.

- exec:

  Logical: Should the C# method be included in the set executed by
  MainFunc?

- tree:

  Which tree to use. There are currently 12 generic tree objects
  available, named "tree_1" through "tree_12". The number of a tree
  (1-12) can be specified instead of the full name.

## Value

The `unifir_script` object passed to `script`, with props for adding
assets appended.

## Details

In effect, these functions provide a thin wrapper across
[instantiate_prefab](https://docs.ropensci.org/unifir/reference/instantiate_prefab.md)
and
[import_asset](https://docs.ropensci.org/unifir/reference/import_asset.md).
By providing the directory an asset is stored in, and the path to the
prefab file once that directory has been copied into Unity, these files
will add prefabs to specified locations throughout the scene. This
function will also download the necessary assets and handles specifying
file paths.

add_default_player adds "player" controllers to a Unity scene.
add_default_tree adds tree GameObjects.

## See also

Other props:
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
[`save_scene()`](https://docs.ropensci.org/unifir/reference/save_scene.md),
[`set_active_scene()`](https://docs.ropensci.org/unifir/reference/set_active_scene.md),
[`validate_path()`](https://docs.ropensci.org/unifir/reference/ValidatePath.md)

Other utilities:
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
[`validate_path()`](https://docs.ropensci.org/unifir/reference/ValidatePath.md),
[`waiver()`](https://docs.ropensci.org/unifir/reference/waiver.md)

## Examples

``` r
if (interactive()) {
  # First, create a script object.
  # CRAN doesn't have Unity installed, so pass
  # a waiver object to skip the Unity-lookup stage:
  script <- make_script("example_script", unity = waiver())

  # Now add props:
  script <- add_default_player(script)
  script <- add_default_tree(script, 1)
  script <- save_scene(script)
}

# Lastly, execute the script via the `action` function
```
