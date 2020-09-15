set(dir "${CMAKE_CURRENT_SOURCE_DIR}")

list(
  APPEND prog
  "${dir}/main.f90"
)

set(prog "${prog}" PARENT_SCOPE)
