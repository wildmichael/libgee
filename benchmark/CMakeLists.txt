
include_directories("${CMAKE_BINARY_DIR}/gee")

vala_add_executable(benchmarks
  benchmark.vala
  benchmarksorts.vala
  mergesort.vala
  PACKAGES gee
  )

target_link_libraries(benchmarks gee)
