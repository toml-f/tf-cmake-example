get_directory_property(toml-f-found "toml-f")
if(NOT toml-f-found)
  message(SEND_ERROR "Initialize toml-f with:\ngit submodule add https://github.com/toml-f/toml-f subprojects/toml-f")
endif()

# Configuration options for TOML-Fortran dependency
set(BUILD_SHARED_LIBS OFF)
add_subdirectory("toml-f")
list(
  APPEND lib-deps
  "toml-f-lib"
)
set(lib-deps "${lib-deps}" PARENT_SCOPE)

# Package license files of TOML-Fortran in case we link statically
if(NOT BUILD_SHARED_LIBS)
  install(
    FILES
    ${toml-f-lic}
    DESTINATION "${CMAKE_INSTALL_DATADIR}/licenses/${PROJECT_NAME}/toml-f"
  )
endif()
