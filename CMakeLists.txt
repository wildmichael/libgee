project(libgee C)

# version information
#####################
set(GEE_VERSION 0.5.1)
set(GEE_SOVERSION 2.0.0)
set(GEE_MINIMUM_VALA_VERSION 0.7.7)
set(GEE_MINIMUM_CMAKE_VERSION 2.8)

cmake_minimum_required(VERSION ${GEE_MINIMUM_CMAKE_VERSION})

# options and installation paths
################################
option(GEE_ENABLE_DOC "Enable documentation generation" FALSE)
option(GEE_ENABLE_BENCHMARKS "Enable benchmark" FALSE)

set(GEE_BIN_DIR bin CACHE STRING "Installation directory for executables")
set(GEE_LIB_DIR lib CACHE STRING "Installation directory for libraries")
set(GEE_INC_DIR include CACHE STRING
  "Installation directory for development headers")
set(GEE_DATA_DIR share CACHE STRING
  "Installation directory for data files")
# make paths absolute
foreach(d BIN LIB INC DATA)
  if(NOT IS_ABSOLUTE "${GEE_${d}_DIR}")
    set(GEE_${d}_DIR "${CMAKE_INSTALL_PREFIX}/${GEE_${d}_DIR}")
  endif()
endforeach()

mark_as_advanced(GEE_ENABLE_DOC GEE_ENABLE_BENCHMARKS GEE_BIN_DIR GEE_LIB_DIR
  GEE_INC_DIR GEE_DATA_DIR)

if(APPLE)
  set(CMAKE_INSTALL_NAME_DIR "${GEE_LIB_DIR}")
elseif(UNIX)
  set(CMAKE_INSTALL_RPATH "${GEE_LIB_DIR}")
endif()

# find required tools
#####################
find_package(Vala ${GEE_MINIMUM_VALA_VERSION} REQUIRED)
include("${VALA_USE_FILE}")

if(GEE_ENABLE_DOC)
  find_package(Valadoc REQUIRED)
endif()

# build gee
###########
add_subdirectory(gee)

if(GEE_ENABLE_BENCHMARKS)
  add_subdirectory(benchmark)
endif()

if(GEE_ENABLE_DOC)
  add_subdirectory(doc)
endif()

# export targets to build and install tree
##########################################
export(TARGETS gee
  NAMESPACE Gee:
  FILE "${CMAKE_BINARY_DIR}/GeeExports.cmake")
if(${CMAKE_VERSION} VERSION_GREATER 2.6.4)
  export(PACKAGE Gee)
endif()
install(EXPORT GeeExports
  DESTINATION "${GEE_DATA_DIR}/gee/cmake"
  NAMESPACE Gee:
  COMPONENT dev)

# configure and install CMake files
###################################
configure_file(cmake/GeeConfigVersion.cmake.in
  "${CMAKE_BINARY_DIR}/GeeConfigVersion.cmake" @ONLY)
# set up for build tree
set(CONFIG_CMAKE_DIR "${CMAKE_BINARY_DIR}")
set(CONFIG_VAPI_DIR "${CMAKE_BINARY_DIR}/gee")
set(CONFIG_INC_DIR "${CMAKE_BINARY_DIR}/gee")
configure_file(cmake/GeeConfig.cmake.in
  "${CMAKE_BINARY_DIR}/GeeConfig.cmake" @ONLY)
# set up for install tree
set(CONFIG_CMAKE_DIR "${GEE_DATA_DIR}/gee/cmake")
set(CONFIG_VAPI_DIR "${GEE_DATA_DIR}/vala/vapi")
set(CONFIG_INC_DIR "${GEE_INC_DIR}/gee-1.0")
configure_file(cmake/GeeConfig.cmake.in
  "${CMAKE_BINARY_DIR}/install_files/GeeConfig.cmake" @ONLY)
# install the cmake files
install(FILES
  "${CMAKE_BINARY_DIR}/GeeConfigVersion.cmake"
  "${CMAKE_BINARY_DIR}/install_files/GeeConfig.cmake"
  DESTINATION "${CONFIG_CMAKE_DIR}" COMPONENT dev)

# configure and install pkg-config file
#######################################
set(VERSION "${GEE_VERSION}")
set(prefix "${CMAKE_INSTALL_PREFIX}")
set(exec_prefix)
set(libdir "${GEE_LIB_DIR}")
set(includedir "${GEE_INC_DIR}")
set(datarootdir "${GEE_DATA_DIR}")
set(datadir "\${datarootdir}")
configure_file(gee-1.0.pc.in "${CMAKE_BINARY_DIR}/install_files/gee-1.0.pc" @ONLY)
install(FILES "${CMAKE_BINARY_DIR}/install_files/gee-1.0.pc"
  DESTINATION "${GEE_LIB_DIR}/pkgconfig" COMPONENT dev)
