option(BUILD_SHARED_LIBS "Whether the libraries built should be shared" ON)

# Set build type as CMake does not provide defaults
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  set(
    CMAKE_BUILD_TYPE "RelWithDebInfo"
    CACHE STRING "Build type to be used."
    FORCE
  )
  message(
    STATUS
    "Setting build type to '${CMAKE_BUILD_TYPE}' as none was specified."
  )
endif()

# Include local CMake modules
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR} PARENT_SCOPE)

if(WIN32)
  list(
    APPEND CMAKE_EXE_LINKER_FLAGS
    "-Wl,--allow-multiple-definition"
  )
endif()
