# Add a light to a Unity scene

This function creates light objects within a Unity scene. This function
can only add one light at a time – call the function multiple times to
add more than one light.

## Usage

``` r
add_light(
  script,
  light_type = c("Directional", "Point", "Spot", "Area"),
  method_name = NULL,
  light_name = "Light",
  x_position = 0,
  y_position = 0,
  z_position = 0,
  x_scale = 1,
  y_scale = 1,
  z_scale = 1,
  x_rotation = 50,
  y_rotation = -30,
  z_rotation = 0,
  exec = TRUE
)
```

## Arguments

- script:

  A `unifir_script` object, created by
  [make_script](https://docs.ropensci.org/unifir/reference/make_script.md)
  or returned by an `add_prop_*` function.

- light_type:

  One of "Directional", "Point", "Spot", or "Area". See
  <https://docs.unity3d.com/Manual/Lighting.html> for more information.

- method_name:

  The internal name to use for the C# method created. Will be randomly
  generated if not set.

- light_name:

  The name to assign the Light object.

- x_position, y_position, z_position:

  The position of the GameObject in world space.

- x_scale, y_scale, z_scale:

  The scale of the GameObject (relative to its parent object).

- x_rotation, y_rotation, z_rotation:

  The rotation of the GameObject to create, as Euler angles.

- exec:

  Logical: Should the C# method be included in the set executed by
  MainFunc?

## Value

The `unifir_script` object passed to `script`, with props for adding
lights appended.

## See also

Other props:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
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

## Examples

``` r
# First, create a script object.
# CRAN doesn't have Unity installed, so pass
# a waiver object to skip the Unity-lookup stage:
script <- make_script("example_script", unity = waiver())

# Now add props:
script <- add_light(script)

# Lastly, execute the script via the `action` function
```
