if (PREFER_EXTERNAL_DEPS)
  find_package(pybind11 QUIET)
  if (NOT pybind11_FOUND)
    message(WARNING "No pybind11 found in system libraries, using vendored pybind11...")
  endif()
endif()

if (NOT TARGET pybind11::pybind11_headers)
  add_subdirectory(${VALHALLA_SOURCE_DIR}/third_party/pybind11 ${CMAKE_BINARY_DIR}/third_party/pybind11)
endif()

pybind11_add_module(_valhalla valhalla/_valhalla.cc)
target_link_libraries(_valhalla PUBLIC valhalla)
set_target_properties(_valhalla PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/valhalla)
set_target_properties(valhalla PROPERTIES POSITION_INDEPENDENT_CODE ON)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/valhalla/__init__.py ${CMAKE_CURRENT_BINARY_DIR}/valhalla/__init__.py COPYONLY)
configure_file(${VALHALLA_SOURCE_DIR}/scripts/valhalla_build_config ${CMAKE_CURRENT_BINARY_DIR}/valhalla/valhalla_build_config.py COPYONLY)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/valhalla/config.py ${CMAKE_CURRENT_BINARY_DIR}/valhalla/config.py COPYONLY)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/valhalla/actor.py ${CMAKE_CURRENT_BINARY_DIR}/valhalla/actor.py COPYONLY)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/valhalla/utils.py ${CMAKE_CURRENT_BINARY_DIR}/valhalla/utils.py COPYONLY)

message(STATUS "Installing python modules to ${Python_SITEARCH}")
install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/valhalla
DESTINATION "${Python_SITEARCH}"
COMPONENT runtime
FILES_MATCHING PATTERN "*.py")
install(TARGETS _valhalla
DESTINATION "${Python_SITEARCH}/valhalla"
COMPONENT runtime)
