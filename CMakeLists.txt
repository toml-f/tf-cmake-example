cmake_minimum_required(VERSION 3.9)
get_directory_property(is-subproject PARENT_DIRECTORY)

project(
  "tf-example"
  LANGUAGES "Fortran"
  VERSION "0.1"
)

# Follow GNU conventions for installing directories
include(GNUInstallDirs)

# General configuration information
add_subdirectory("config")

# Get the toml-f subproject
set(lib-deps)
add_subdirectory("subprojects")

# Collect source of the project
set(srcs)
add_subdirectory("src")

# Create example library target
add_library(
  "${PROJECT_NAME}-lib"
  "${srcs}"
)
set_target_properties(
  "${PROJECT_NAME}-lib"
  PROPERTIES
  POSITION_INDEPENDENT_CODE TRUE
  OUTPUT_NAME "${PROJECT_NAME}"
  VERSION "${PROJECT_VERSION}"
  SOVERSION "${PROJECT_VERSION_MAJOR}"
)
target_link_libraries(
  "${PROJECT_NAME}-lib"
  PRIVATE
  "${lib-deps}"
)
target_include_directories(
  "${PROJECT_NAME}-lib"
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

# Collect source of the application
set(prog)
add_subdirectory("app")

# Example application target
add_executable(
  "${PROJECT_NAME}-exe"
  "${prog}"
)
set_target_properties(
  "${PROJECT_NAME}-exe"
  PROPERTIES
  OUTPUT_NAME "${PROJECT_NAME}"
)
target_link_libraries(
  "${PROJECT_NAME}-exe"
  PRIVATE
  "${PROJECT_NAME}-lib"
)

if(NOT is-subprojects OR BUILD_SHARED_LIBS)
  # Export targets for other projects
  install(
    TARGETS
    "${PROJECT_NAME}-exe"
    "${PROJECT_NAME}-lib"
    EXPORT
    "${PROJECT_NAME}-config"
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
  )

  install(
    EXPORT "${PROJECT_NAME}-config"
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}"
  )

  # Package license files
  install(
    FILES
    "LICENSE"
    DESTINATION "${CMAKE_INSTALL_DATADIR}/licenses/${PROJECT_NAME}"
  )
endif()
