add_custom_command(OUTPUT "${CMAKE_CURRENT_BINARY_DIR}/gee-1.0/index.html"
  # IMPORTANT: -o must not be an absolute path
  COMMAND ${VALADOC_EXECUTABLE} -o "gee-1.0/" --wiki ./ --force
  -b "${CMAKE_SOURCE_DIR}" --vapidir "${VALA_VAPI_DIR}" ${GEE_SRCS}
  DEPENDS ${GEE_SRCS}
  WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
  COMMENT "Generating documentation in ${CMAKE_CURRENT_BINARY_DIR}/gee-1.0"
  VERBATIM
  )

add_custom_target(doc ALL DEPENDS "${CMAKE_CURRENT_BINARY_DIR}/gee-1.0/index.html")

install(DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/gee-1.0"
  DESTINATION "${GEE_DATA_DIR}/devhelp/references")
