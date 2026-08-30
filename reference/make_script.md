# Create an empty `unifir_script` object.

unifir relies upon "script" objects, which collect "prop" objects (C#
methods) which then may be executed within a Unity project via the
[action](https://docs.ropensci.org/unifir/reference/action.md) function.

## Usage

``` r
make_script(
  project,
  script_name = NULL,
  scene_name = NULL,
  unity = find_unity(),
  initialize_project = NULL
)
```

## Arguments

- project:

  The directory path of the Unity project.

- script_name:

  The file name to save the script at. The folder location and file
  extensions will be added automatically.

- scene_name:

  The default scene to operate within. If a function requires a scene
  name and one is not provided, this field will be used.

- unity:

  The location of the Unity executable to create projects with.

- initialize_project:

  If TRUE, will call
  [create_unity_project](https://docs.ropensci.org/unifir/reference/create_unity_project.md)
  to create a Unity project at `project`. If FALSE, will not create a
  new project. If NULL, will create a new project if `project` does not
  exist.

## Value

A `unifir_script` object.

## Examples

``` r
# Create an empty script file
# In practice, you'll want to set `project` to the project path to create
# and `unity` to `NULL` (the default)
make_script(project = waiver(), unity = waiver())
#> A `unifir_script` object with 0 props
#> 
#> [1] name type
#> <0 rows> (or 0-length row.names)
```
