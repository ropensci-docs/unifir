# Changelog

## unifir (development version)

## unifir 0.2.4

CRAN release: 2024-02-01

- New `print.unifir_script` method hides some of the R6 internals
  backing the package, and makes my dissertation chapter render nicer.
- Bug fixes:
  - Removed trailing commas in some calls to `glue()` to fix errors on R
    devel ([\#17](https://github.com/ropensci/unifir/issues/17))

## unifir 0.2.3

CRAN release: 2022-12-02

- Bug fixes:
  - Fixed bug where spaces in path to Unity would cause
    [`unity_version()`](https://docs.ropensci.org/unifir/reference/unity_version.md)
    and `create_project()` to fail.
  - Fixed `InstantiatePrefab` C# requirements to now include
    `UnityEditor`.
  - Fixed test for new sf and terra versions.
  - [`associate_coordinates()`](https://docs.ropensci.org/unifir/reference/associate_coordinates.md)
    will now only reproject if both objects have coordinate reference
    systems.
- Documentation changes:
  - Added citation information.

## unifir 0.2.2

CRAN release: 2022-08-11

- Redocumented to keep the package on CRAN.
- Internal changes:
  - [`action()`](https://docs.ropensci.org/unifir/reference/action.md)
    is now much more modular, outsourcing to a handful of new internal
    functions

## unifir 0.2.1

CRAN release: 2022-05-13

This is intentionally a very small patch release, intended to fix three
problems:

- Provides an appropriate citation via `citation("unifir")` and in the
  README
- Addresses a failing test on M1 macs
- Uses [`match.arg()`](https://rdrr.io/r/base/match.arg.html) in
  appropriate places

## unifir 0.2.0

CRAN release: 2022-05-04

- Improvements and bug fixes:
  - [`find_unity()`](https://docs.ropensci.org/unifir/reference/find_unity.md)
    now doesn’t escape its Unity path (so the string returned is the
    actual path to the Unity engine, not a quoted version). Accordingly,
    [`action()`](https://docs.ropensci.org/unifir/reference/action.md)
    now wraps `unity` in
    [`shQuote()`](https://rdrr.io/r/base/shQuote.html).
    ([\#4](https://github.com/ropensci/unifir/issues/4))
  - [`add_default_tree()`](https://docs.ropensci.org/unifir/reference/add_asset.md)
    now imports its trees standing upright by default. If you manually
    set `x_rotation` to 0, however, the trees will import as sideways as
    ever. ([\#7](https://github.com/ropensci/unifir/issues/7))
  - Examples are now tested (and work)
    ([\#8](https://github.com/ropensci/unifir/issues/8) 1d5b1f3)
  - [`create_terrain()`](https://docs.ropensci.org/unifir/reference/create_terrain.md)
    handles non-local terrain files
    ([\#6](https://github.com/ropensci/unifir/issues/6))
  - [`unifir_prop()`](https://docs.ropensci.org/unifir/reference/unifir_prop.md)
    now checks to make sure `script` exists and is a `unifir_script`.
    Previously this errored with a baffling message about long vectors.
- Documentation changes:
  - Vignettes have been fleshed out, and a full example added to the
    user-facing vignette.
    ([\#7](https://github.com/ropensci/unifir/issues/7))
  - Return values are better documented
    ([\#5](https://github.com/ropensci/unifir/issues/5))
  - Functions are consistently linked and documentation formatting is
    now more consistent
    ([\#5](https://github.com/ropensci/unifir/issues/5))
  - [`find_unity()`](https://docs.ropensci.org/unifir/reference/find_unity.md)
    doesn’t now have a weird break in its documentation sections
  - The README now links to vignettes and explains why anyone would want
    to deal with Unity in the first place

## unifir 0.1.0

- Added a `NEWS.md` file to track changes to the package.
