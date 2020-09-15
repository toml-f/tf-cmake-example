# Usage example for the TOML-Fortran library

[*Use this template*](https://github.com/toml-f/tf-cmake-example/generate).

- the [TOML Fortran project](https://github.com/toml-f/toml-f)


## Getting Started

Create a new CMake project and include `toml-f` as git-submodule in your subprojects directory with

```
git submodule add https://github.com/toml-f/toml-f subprojects/toml-f
```

To include the project the necessary boilerplate code for subprojects is just

```cmake
# subprojects/CMakeLists.txt
set(BUILD_SHARED_LIBS OFF)
add_subdirectory("toml-f")
list(
  APPEND lib-deps
  "toml-f-lib"
)
set(lib-deps "${lib-deps}" PARENT_SCOPE)
```

Now you can add `toml-f-lib` to your dependencies and access the public API by the `tomlf` module.

We recommend to disable building `toml-f` as shared library and just link your applications or library statically against it.
The `toml-f` subproject will decide based on the library type if it should install itself along with your project.
