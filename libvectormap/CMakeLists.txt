cmake_minimum_required(VERSION 2.8.12)
project(libvectormap)

find_package(autoware_build_flags REQUIRED)
find_package(catkin REQUIRED COMPONENTS
  vector_map
  vector_map_msgs
)
find_package(Eigen3 QUIET)

if (NOT EIGEN3_FOUND)
  # Fallback to cmake_modules
  find_package(cmake_modules REQUIRED)
  find_package(Eigen REQUIRED)
  set(EIGEN3_INCLUDE_DIRS ${EIGEN_INCLUDE_DIRS})
  set(EIGEN3_LIBRARIES ${EIGEN_LIBRARIES})  # Not strictly necessary as Eigen is head only
  # Possibly map additional variables to the EIGEN3_ prefix.
else ()
  set(EIGEN3_INCLUDE_DIRS ${EIGEN3_INCLUDE_DIR})
endif ()

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")

catkin_package(
  INCLUDE_DIRS include ${EIGEN3_INCLUDE_DIRS}
  LIBRARIES libvectormap
  CATKIN_DEPENDS vector_map
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
)

add_library(libvectormap STATIC
  src/vector_map.cpp
)

target_link_libraries(libvectormap
  ${catkin_LIBRARIES}
  ${EIGEN3_LIBRARIES}
)

add_dependencies(libvectormap
  ${catkin_EXPORTED_TARGETS}
)

install(TARGETS libvectormap
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  PATTERN ".svn" EXCLUDE
)
