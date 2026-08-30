# unifir 102 - A developer's guide

This document provides an overview of the inner workings of unifir,
focusing on how scripts, props, and actions work together to produce a
Unity scene. The tone and focus here are going to be extremely technical
and fiddly; for a friendlier introduction to [unifir 101 - A user’s
guide](https://docs.ropensci.org/unifir/articles/unifir-user-guide.md).

At a high level, unifir operates around the idea of a *script* object, a
container that stores all the parameters and instructions you want to
run through Unity in a single place. Those parameters and instructions
are specified in the form of *props*, which let users add certain
parameterized pre-written commands to their script to execute in
sequence. The actual execution is then handled by the
[`action()`](https://docs.ropensci.org/unifir/reference/action.md)
function, which both prepares the script for execution and then executes
it. This basic pattern is modeled after the fantastic
[recipes](https://recipes.tidymodels.org/) package.

With that framework established, let’s walk through what exactly scripts
and props are and how they interact with
[`action()`](https://docs.ropensci.org/unifir/reference/action.md). But
first, we should talk about
[`waiver()`](https://docs.ropensci.org/unifir/reference/waiver.md).

## Waivers

As we’ll discuss in a moment, unifir does a lot of input checking to
make sure that scripts are going to execute successfully in an attempt
to give users more useful feedback than Unity’s command line interface
often offers. That involves making sure that scripts and props have the
correct values provided to all of their parameters, and also validating
that the user is going to have a working version of Unity to run these
commands in at all.

Of course, a short list of places that *don’t* have a working version of
Unity includes “CRAN check machines” and “GitHub actions”. And so as a
result, even the simplest function calls will fail on CRAN – for
instance, running the following would throw an error:

``` r

library(unifir)
make_script("example")
```

In order to work around this, I’ve borrowed an idea (and the entire
function) from ggplot2:
[`waiver()`](https://docs.ropensci.org/unifir/reference/waiver.md). The
[`waiver()`](https://docs.ropensci.org/unifir/reference/waiver.md)
function is a way to indicate to unifir that yes, you *know* this value
normally needs to be provided, and you *know* that it’s missing, and
that’s *fine*. All
[`waiver()`](https://docs.ropensci.org/unifir/reference/waiver.md) does
is return an object of class `waiver`:

``` r

waiver()
#> list()
#> attr(,"class")
#> [1] "waiver"
```

On its own, this object doesn’t do anything. However, in a few key
places, unifir will understand
[`waiver()`](https://docs.ropensci.org/unifir/reference/waiver.md) as
meaning “don’t validate this argument”, which is essential for some odd
props and CRAN checks to succeed. I’ll be using it throughout this
vignette, starting with:

## Scripts

The “script” is the core object in unifir. To create a script, we use
`make_script`:

``` r

script <- make_script(
  project = file.path(tempdir(), "unifir"),
  unity = waiver() # Don't error if we can't find Unity
)
script
#> A `unifir_script` object with 0 props
#> 
#> [1] name type
#> <0 rows> (or 0-length row.names)
```

This creates an [R6](https://github.com/r-lib/R6) object of the class
`unifir_script`. If you haven’t worked with R6 before, I recommend
checking out [the section of Advanced R on the
subject](https://adv-r.hadley.nz/r6.html). At the end of the day,
however, a script is effectively just a glorified list. We can check out
its contents using [`names()`](https://rdrr.io/r/base/names.html):

``` r

names(script)
#>  [1] ".__enclos_env__"    "using"              "beats"             
#>  [4] "props"              "initialize_project" "unity"             
#>  [7] "scene_exists"       "scene_name"         "script_name"       
#> [10] "project"            "clone"              "initialize"
```

Some of these contents – `.__encols_env__`, `clone`, `initialize` – will
be familiar to anyone used to working with R6. Others, such as the file
names for the scene and script unifir is operating on, are set in
[`make_script()`](https://docs.ropensci.org/unifir/reference/make_script.md)
(and documented in
[`?make_script`](https://docs.ropensci.org/unifir/reference/make_script.md))
and default to `NULL` if not set:

``` r

all(
  is.null(script$initialize_project),
  is.null(script$scene_name),
  is.null(script$script_name)
)
#> [1] TRUE
```

Others, like `using`, `beats`, and `props`, are a little bit more
involved. We’ll use these objects in order to track the props we add to
our script; to talk about that, it’s time we talk about props.

## Props

At a low level, a unifir prop is just another R6 object, created using
the function
[`unifir_prop()`](https://docs.ropensci.org/unifir/reference/unifir_prop.md):

``` r

prop_file <- tempfile()
file.create(prop_file)
#> [1] TRUE

prop <- unifir_prop(
  prop_file = prop_file,
  method_name = "ExampleName",
  method_type = "ExampleMethod",
  build = function(script, prop, debug) {},
  using = "ExampleDependencies",
  parameters = list()
)
prop
#> <unifir_prop>
#>   Public:
#>     build: function (script, prop, debug) 
#>     clone: function (deep = FALSE) 
#>     initialize: function (prop_file, method_name, method_type, parameters, build, 
#>     method_name: ExampleName
#>     method_type: ExampleMethod
#>     parameters: list
#>     prop_file: /tmp/RtmpapN344/file86565a65eb6
#>     using: ExampleDependencies
```

Just as before, we can see our prop’s contents using
[`names()`](https://rdrr.io/r/base/names.html):

``` r

names(prop)
#> [1] ".__enclos_env__" "using"           "build"           "parameters"     
#> [5] "method_type"     "method_name"     "prop_file"       "clone"          
#> [9] "initialize"
```

These fields are all documented in
[`?unifir_prop`](https://docs.ropensci.org/unifir/reference/unifir_prop.md).

Prop objects are the actual method unifir uses to translate R inputs
into C# methods. A typical prop will take some input parameters,
provided either by the prop constructor function (more on that in a
moment) or set at the script level, interpolate them into a pre-written
C# method, and then add that method to a pile of C# code that will be
run in sequence to produce a scene. Of course, the details of this
process depend on what exactly the C# code is expected to *do*.

Most of those details are sorted out by prop constructor functions.
Rather than forcing users to use
[`unifir_prop()`](https://docs.ropensci.org/unifir/reference/unifir_prop.md)
directly, unifir provides a number of wrappers around this function to
add specific props to a script. For instance, if we look at the code
that powers our
[`new_scene()`](https://docs.ropensci.org/unifir/reference/new_scene.md)
function:

``` r

new_scene
#> function (script, setup = c("EmptyScene", "DefaultGameObjects"), 
#>     mode = c("Additive", "Single"), method_name = NULL, exec = TRUE) 
#> {
#>     setup <- match.arg(setup)
#>     mode <- match.arg(mode)
#>     prop <- unifir_prop(prop_file = system.file("NewScene.cs", 
#>         package = "unifir"), method_name = method_name, method_type = "NewScene", 
#>         parameters = list(setup = setup, mode = mode), build = function(script, 
#>             prop, debug) {
#>             glue::glue(readChar(prop$prop_file, file.info(prop$prop_file)$size), 
#>                 .open = "%", .close = "%", method_name = prop$method_name, 
#>                 setup = setup, mode = mode)
#>         }, using = c("UnityEngine.SceneManagement", "UnityEditor", 
#>             "UnityEditor.SceneManagement"))
#>     add_prop(script, prop, exec)
#> }
#> <bytecode: 0x5626e363a078>
#> <environment: namespace:unifir>
```

This prop takes two parameters – `setup` and `mode` – and the top of the
script makes sure they exist and are passed correctly. We then get to
the internal
[`unifir_prop()`](https://docs.ropensci.org/unifir/reference/unifir_prop.md)
call. This call passes `system.file("NewScene.cs", package = "unifir")`
to the `prop_file` argument; if we print that file out we can see that
it’s a relatively simple C# method:

``` r

readLines(system.file("NewScene.cs", package = "unifir"))
#> [1] ""                                                                                               
#> [2] "    static void %method_name%() {"                                                              
#> [3] "        var newScene = EditorSceneManager.NewScene(NewSceneSetup.%setup%, NewSceneMode.%mode%);"
#> [4] "    }"
```

This method calls `EditorSceneManager.NewScene`, part of Unity’s
scripting API, and uses it to create a new scene. Because this code
relies on the “UnityEngine.SceneManagement”, “UnityEditor”, and
“UnityEditor.SceneManagement” namespaces, we’ve included those
namespaces in our prop’s `using` argument. Our R code is only going to
edit three parts of this function, marked by `%` signs – the
`method_name`, `setup`, and `mode` arguments will all be replaced by
their equivalent values in R.

Moving further along the `unifir_prop` call, we can then see that
`method_name` is set to `NULL` by default. `method_name` is a unique
identifier for *this method of this prop*, so should not be hard-coded
or provided as a default in your prop constructors. If you leave it as
`NULL`, unifir will attempt to generate a name made of 4 random English
words to fill in the space.

The next argument, `method_type`, is internally set to `NewScene` –
users cannot control this value. `method_type` is meant to be a certain
“key” associated with your specific type of prop, which other props
might search for if they depend on or conflict with your code; as such,
it generally shouldn’t be configurable by your users.

We then see that our function parameters are passed as a list to the
`parameters` argument. These will be essential for our next argument,
the `build()` function, which constructs our C# method. The `build()`
function of every prop must take three (and only three) arguments:
`script`, the unifir script a prop is stored in, `prop`, the prop being
built, and `debug`, which is discussed below. `build()` methods with
more arguments than this will cause errors. As a result, all other
parameters you need to construct your C# method *must* be stored either
in the prop or script object, and it is generally easiest to store them
in `parameters` (which is not checked by the R6 class).

The actual build function here is relatively simple, using `glue` to
replace the snippets between `%` symbols with their R equivalents. If we
don’t change any of the default arguments to this function, that means
our output C# method will look something like this:

``` r

script <- new_scene(script)

script$props[[1]]$build(script, script$props[[1]])
#> static void BigFallObjectWould() {
#>     var newScene = EditorSceneManager.NewScene(NewSceneSetup.EmptyScene, NewSceneMode.Additive);
#> }
```

The last part of our prop constructor is the `add_prop` function, which
registers the prop as part of our script. Its code is incredibly simple,
and mostly deals with creating the `script$beats` table:

``` r

add_prop
#> function (script, prop, exec = TRUE) 
#> {
#>     stopifnot(is.logical(exec))
#>     stopifnot(methods::is(script, "unifir_script"))
#>     idx <- nrow(script$beats) + 1
#>     script$props[[idx]] <- prop
#>     script$beats[idx, ]$idx <- idx
#>     script$beats[idx, ]$name <- prop$method_name
#>     script$beats[idx, ]$type <- prop$method_type
#>     script$beats[idx, ]$exec <- exec
#>     script$using <- c(script$using, prop$using)
#>     script
#> }
#> <bytecode: 0x5626e2e94f30>
#> <environment: namespace:unifir>
```

That table is relatively simple, storing four variables: `idx`, the
order that methods will be executed in, `name`, the `method_name` of
each method, `type`, the `method_type` of each method, and `exec`, a
boolean representing whether or not that method should be called in the
final C# script:

``` r

script$beats
#>    idx               name     type exec
#> NA   1 BigFallObjectWould NewScene TRUE
```

## Action

With our props and scripts written, it’s showtime! We can use the
`action` function to transform our R6 objects into an actual C# script,
and then execute that script in Unity.

The [`action()`](https://docs.ropensci.org/unifir/reference/action.md)
does quite a few things. Namely, it:

- Checks if the Unity project needs to be created, and creates it if
  so.  
- Fills in any missing directory or file names that were left as
  `NULL`.  
- Calls the `build()` method of each prop, in order of
  `script$beats$idx`, and stores the constructed C# methods back in the
  script.
- Creates a “caller” function that will call every method where
  `script$beats$exec` is `TRUE` in sequential order.  
- If `write = TRUE`, writes a final C# script to file.
- If `exec = TRUE`, executes the final C# script in Unity.

Most of this process is internal and doesn’t matter for any prop
constructors you write. So long as your prop is idempotent and can be
constructed using its own `build` argument,
[`action()`](https://docs.ropensci.org/unifir/reference/action.md)
shouldn’t create any issues.

However, if it does cause trouble,
[`action()`](https://docs.ropensci.org/unifir/reference/action.md)
returns a constructed script object with props replaced by their
equivalent C# code. That makes it easy to see how unifir interpreted
your build argument; for instance, we can run our example script through
[`action()`](https://docs.ropensci.org/unifir/reference/action.md) as
so:

``` r

script <- make_script(
  project = file.path(tempdir(), "unifir"),
  unity = waiver(), # Don't error if we can't find Unity
  initialize_project = FALSE, # Don't create the project -- so this runs on CRAN
  script_name = "example_script"
)
script <- new_scene(script)
exec_script <- action(
  script,
  exec = FALSE,
  write = TRUE
)
```

If we’re concerned about how unifir translated our prop code, we can
find the rendered C# inside `props`:

``` r

exec_script$props
#> [1] "static void ShapeInventChanceSleep() {\n    var newScene = EditorSceneManager.NewScene(NewSceneSetup.EmptyScene, NewSceneMode.Additive);\n}\n"
```

To see the entire produced C# script, we need to read the actual script
file itself:

``` r

readLines(
  file.path(tempdir(), "unifir", "Assets", "Editor", "example_script.cs")
  )
#>  [1] "using UnityEngine.SceneManagement;"                                                              
#>  [2] "using UnityEditor;"                                                                              
#>  [3] "using UnityEditor.SceneManagement; "                                                             
#>  [4] ""                                                                                                
#>  [5] "public class example_script {"                                                                   
#>  [6] "static void ShapeInventChanceSleep() {"                                                          
#>  [7] "    var newScene = EditorSceneManager.NewScene(NewSceneSetup.EmptyScene, NewSceneMode.Additive);"
#>  [8] "}"                                                                                               
#>  [9] ""                                                                                                
#> [10] "    static void MainFunc() {"                                                                    
#> [11] "        ShapeInventChanceSleep();"                                                               
#> [12] "    }"                                                                                           
#> [13] "}"
```

Notice how this code matches our prop, with two additions: first, all
the namespaces we provided to `using` are now imported at the top of the
script, and second a function `MainFunc` has been created to call our
prop. When executing the C# script, unifir will execute this `MainFunc`
method, which will in turn call each prop once in the order it’s listed
in `script$beats`.

## Debug

One last thing to know about unifir is that it is also built with a
“debug” mode, in which functions will make no changes to your file
system. unifir code checks if it’s running in debug mode using the
following code:

``` r

function() {
  debug <- FALSE
  if (Sys.getenv("unifir_debugmode") != "" ||
      !is.null(options("unifir_debugmode")$unifir_debugmode)) {
    debug <- TRUE
  }
  debug
}
```

So if `unifir_debugmode` is set to any value either as an environment
variable or as an option, unifir will avoid writing anything to file or
making any changes to a user’s computer.

When [`action()`](https://docs.ropensci.org/unifir/reference/action.md)
is called, it will provide the current state of `debug` to your prop’s
`build` function. For the majority of props, this can be safely ignored;
if all your prop does is add C# code to the final script,
[`action()`](https://docs.ropensci.org/unifir/reference/action.md) will
respect debug on your behalf. However, if your prop makes changes to the
file system before the script is actually executed – for instance, by
moving prefabs to the project directory or editing configuration files
from R – make sure to wrap those sections of your prop in `if (!debug)`!

## Cloning

While I’ve avoided getting too deep into the underlying mechanics of R6
here, there’s one stumbling block that I want to flag for anyone
interested in developing with unifir.

The vast majority of objects in R have what’s referred to as
“copy-on-modify” semantics. Say for instance you have some object `x`:

``` r

x <- 2
x
#> [1] 2
```

If you assign `x` to a new object, `y`, we’d expect `y` and `x` to both
have the same value:

``` r

y <- x
y == x
#> [1] TRUE
```

As an optimization on R’s part, not only are these objects both the same
value, but they actually both point to the same piece of data on your
machine. R only actually creates a new variable, pointing to its own
unique data, when you actually modify the object. As a result, when you
change the value of `x`, you *don’t* in turn change the value of `y`: R
makes a copy when you modify the original object, so they now point to
different data:

``` r

x <- 1
y == x
#> [1] FALSE
```

The same is **not true** for R6 objects, like unifir scripts and props.
If you assign a prop to a new object, both of the objects will point to
the same data, and changing one object will change them both. This is
true whether you change the new object:

``` r

other_prop <- prop
other_prop$method_name <- "NewName"
prop$method_name
#> [1] "NewName"
```

Or the original one:

``` r

prop$method_name <- "AnotherName"
other_prop$method_name
#> [1] "AnotherName"
```

Instead, with R6 objects, we need to make an explicit copy. We can do
this using the `clone()` function inside our prop object, like so:

``` r

disconnected_prop <- prop$clone()
```

This creates an actual disconnected object, with the same values as the
original it was cloned from. Now we can make changes to our prop (or
script) without impacting any of the other copies:

``` r

disconnected_prop$method_name <- "OnlyIGetThisName"
prop$method_name
#> [1] "AnotherName"
```

Make sure that, if you’re trying to have multiple different versions of
a prop or a script, you use `clone()` in your own code!
