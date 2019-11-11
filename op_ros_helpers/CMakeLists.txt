cmake_minimum_required(VERSION 2.8.3)
project(op_ros_helpers)

find_package(autoware_build_flags REQUIRED)

find_package(catkin REQUIRED COMPONENTS
  autoware_msgs
  geometry_msgs
  jsk_recognition_msgs
  libwaypoint_follower
  map_file
  op_planner
  op_simu
  op_utility
  pcl_conversions
  pcl_ros
  roscpp
  sensor_msgs
  tf
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES op_ros_helpers
  CATKIN_DEPENDS
    op_utility
    op_planner
    op_simu
)

SET(CMAKE_CXX_FLAGS "-O2 -g -Wall -Wno-unused-result ${CMAKE_CXX_FLAGS}")

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

set(ROS_HELPERS_SRC
  src/PolygonGenerator.cpp
  src/op_ROSHelpers.cpp
)

## Declare a cpp library
add_library(${PROJECT_NAME}
  ${ROS_HELPERS_SRC}
)

target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)

install(TARGETS op_ros_helpers
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
