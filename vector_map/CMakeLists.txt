cmake_minimum_required(VERSION 2.8.3)
project(vector_map)

find_package(autoware_build_flags REQUIRED)

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
  tf
  vector_map_msgs
  visualization_msgs
)

set(CMAKE_CXX_FLAGS "-O2 -Wall ${CMAKE_CXX_FLAGS}")

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES vector_map
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_library(vector_map
  lib/vector_map/vector_map.cpp
)
add_dependencies(vector_map
  ${catkin_EXPORTED_TARGETS}
)
target_link_libraries(vector_map
  ${catkin_LIBRARIES}
)

install(TARGETS vector_map
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)
