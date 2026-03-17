cmake_minimum_required(VERSION 3.5)
project(rf2o_laser_odometry)


if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(tf2  REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(nav_msgs REQUIRED)

## System dependencies are found with CMake's conventions
find_package(Boost REQUIRED)
find_package(Eigen3 REQUIRED)

include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}/include
  ${Boost_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
)

set(laser_odom_dependencies
  "rclcpp"
  "std_msgs"
  "geometry_msgs"
  "tf2_geometry_msgs"
  "tf2"
  "tf2_ros"
  "sensor_msgs"
  "nav_msgs"
)

set(laser_odometry_src
  src/CLaserOdometry2DNode.cpp
  src/CLaserOdometry2D.cpp
)

add_library(lib_laser_odometry SHARED
  ${laser_odometry_src}
)
ament_target_dependencies(lib_laser_odometry
  ${laser_odom_dependencies}
)
add_executable(rf2o_laser_odometry_exe
  src/CLaserOdometry2DNode.cpp
)
target_link_libraries(rf2o_laser_odometry_exe
  lib_laser_odometry
)
install(TARGETS
  rf2o_laser_odometry_exe
  DESTINATION lib/${PROJECT_NAME}
)
install(TARGETS
  lib_laser_odometry
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)

install(DIRECTORY include/${PROJECT_NAME}
  DESTINATION include/
)

install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME})

ament_package()

