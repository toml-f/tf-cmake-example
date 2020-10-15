# Configuration options for TOML-Fortran dependency
set(BUILD_SHARED_LIBS OFF)

# Include the toml-f project
get_directory_property(toml-f-found "toml-f")
if(toml-f-found)
  add_subdirectory("toml-f")
else()
  set(toml-f-url "https://github.com/toml-f/toml-f")
  message(STATUS "Retrieving TOML-Fortran from ${toml-f-url}")
  include(DownloadProject)
  download_project(
    PROJ "toml-f"
    GIT_REPOSITORY "${toml-f-url}"
    GIT_TAG "HEAD"
    QUIET
  )
  add_subdirectory("${toml-f_SOURCE_DIR}" "${toml-f_BINARY_DIR}")
endif()
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
