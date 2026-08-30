# Create a terrain tile with optional image overlay

Create a terrain tile with optional image overlay

## Usage

``` r
create_terrain(
  script,
  method_name = NULL,
  heightmap_path,
  x_pos,
  z_pos,
  width,
  height,
  length,
  heightmap_resolution,
  texture_path = "",
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

- heightmap_path:

  The file path to the heightmap to import as terrain.

- x_pos, z_pos:

  The position of the corner of the terrain.

- width, height, length:

  The dimensions of the terrain tile, in linear units.

- heightmap_resolution:

  The resolution of the heightmap image.

- texture_path:

  Optional: the file path to the image to use as a terrain overlay.

- exec:

  Logical: Should the C# method be included in the set executed by
  MainFunc?

## See also

Other props:
[`add_default_player()`](https://docs.ropensci.org/unifir/reference/add_asset.md),
[`add_light()`](https://docs.ropensci.org/unifir/reference/add_light.md),
[`add_prop()`](https://docs.ropensci.org/unifir/reference/add_prop.md),
[`add_texture()`](https://docs.ropensci.org/unifir/reference/add_texture.md),
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
if (requireNamespace("terra", quietly = TRUE)) {
  raster <- tempfile(fileext = ".tiff")
  r <- terra::rast(matrix(rnorm(1000^2, mean = 100, sd = 20), 1000),
    extent = terra::ext(0, 1000, 0, 1000)
  )
  terra::writeRaster(r, raster)

  script <- make_script("example_script",
    unity = waiver()
  )
  create_terrain(
    script,
    heightmap_path = raster,
    x_pos = 0,
    z_pos = 0,
    width = 1000,
    height = terra::minmax(r)[[2]],
    length = 1000,
    heightmap_resolution = 1000
  )
}
#> A `unifir_script` object with 4 props
#> 
#>                name          type
#> 1    LoadPNGAutoAdd       LoadPNG
#> 2 AddTextureAutoAdd    AddTexture
#> 3    ReadRawAutoAdd       ReadRaw
#> 4  SailAnGovernCall CreateTerrain
```
