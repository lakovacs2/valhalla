file(GLOB headers ${VALHALLA_SOURCE_DIR}/valhalla/thor/*.h)

set(sources
  astar_bss.cc
    alternates.cc
  bidirectional_astar.cc
  costmatrix.cc
  dijkstras.cc
  matrix_action.cc
  multimodal.cc
  timedistancematrix.cc
    triplegbuilder.cc
  unidirectional_astar.cc
    worker.cc)

set(sources_with_warnings
  centroid.cc
  expansion_action.cc
  isochrone_action.cc
  isochrone.cc
  map_matcher.cc
  optimized_route_action.cc
  optimizer.cc
  route_action.cc
  route_matcher.cc
  status_action.cc
  timedistancebssmatrix.cc
  trace_attributes_action.cc
  trace_route_action.cc
  triplegbuilder_utils.h)

# treat date library as system
set(system_includes
  ${VALHALLA_SOURCE_DIR}/third_party/date/include
  $<$<BOOL:${WIN32}>:${VALHALLA_SOURCE_DIR}/third_party/dirent/include>)
if(APPLE)
  list(APPEND system_includes ${VALHALLA_SOURCE_DIR}/third_party/date/include/date)
endif()

valhalla_module(NAME thor
  SOURCES ${sources}
  SOURCES_WITH_WARNINGS ${sources_with_warnings}
  HEADERS ${headers}
  INCLUDE_DIRECTORIES
    PUBLIC
      ${VALHALLA_SOURCE_DIR}
      ${VALHALLA_SOURCE_DIR}/valhalla
    PRIVATE
      ${CMAKE_BINARY_DIR}
  SYSTEM_INCLUDE_DIRECTORIES
    PUBLIC
      ${system_includes}
    PRIVATE
      ${VALHALLA_SOURCE_DIR}/third_party/rapidjson/include
      ${VALHALLA_SOURCE_DIR}/third_party/robin-hood-hashing/src/include

  DEPENDS
    valhalla::proto
    valhalla::sif
    valhalla::meili
    ${valhalla_protobuf_targets}
    Boost::boost
    ${libprime_server_targets})
