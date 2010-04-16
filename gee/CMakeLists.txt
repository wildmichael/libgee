set(GEE_SRCS
  "${CMAKE_CURRENT_SOURCE_DIR}/abstractcollection.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/abstractlist.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/abstractmap.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/abstractmultimap.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/abstractmultiset.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/abstractqueue.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/abstractset.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/arraylist.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/bidiriterator.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/collection.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/comparable.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/deque.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/functions.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/hashmap.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/hashmultimap.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/hashmultiset.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/hashset.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/iterable.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/iterator.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/linkedlist.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/list.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/listiterator.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/map.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/mapiterator.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/multimap.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/multiset.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/priorityqueue.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/queue.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/readonlycollection.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/readonlylist.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/readonlymap.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/readonlyset.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/set.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/sortedset.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/timsort.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/treemap.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/treemultimap.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/treemultiset.vala"
  "${CMAKE_CURRENT_SOURCE_DIR}/treeset.vala"
  )
set(GEE_SRCS "${GEE_SRCS}" PARENT_SCOPE)

vala_add_library(gee SHARED
  ${GEE_SRCS}
  LIBRARY gee-1.0
  GENERATE_VAPI gee-1.0.vapi
  GENERATE_INTERNAL_VAPI gee-internals-1.0.vapi
  GENERATE_HEADER gee.h
  GENERATE_INTERNAL_HEADER gee-internals.h
  COMPILE_FLAGS --gir Gee-1.0.gir
  )

set_target_properties(gee PROPERTIES
  VERSION ${GEE_VERSION}
  SOVERSION ${GEE_SOVERSION}
  PUBLIC_HEADER "${CMAKE_CURRENT_BINARY_DIR}/gee.h"
  )

install(TARGETS gee EXPORT GeeExports
  ARCHIVE DESTINATION "${GEE_LIB_DIR}" COMPONENT dev
  RUNTIME DESTINATION "${GEE_BIN_DIR}" COMPONENT shlibs
  LIBRARY DESTINATION "${GEE_LIB_DIR}" COMPONENT shlibs
  PUBLIC_HEADER DESTINATION "${GEE_INC_DIR}/gee-1.0" COMPONENT dev
  )

install(FILES "${CMAKE_CURRENT_BINARY_DIR}/gee-1.0.vapi"
  DESTINATION "${GEE_DATA_DIR}/vala/vapi" COMPONENT data)

install(FILES "${CMAKE_CURRENT_BINARY_DIR}/Gee-1.0.gir"
  DESTINATION "${GEE_DATA_DIR}/gir-1.0" COMPONENT data)
