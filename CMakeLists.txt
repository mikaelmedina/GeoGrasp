cmake_minimum_required(VERSION 3.12)
project(geograsp)

# Force C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -O3)
endif()

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(pcl_conversions REQUIRED)
find_package(pcl_ros REQUIRED)
find_package(eigen3_cmake_module REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(sensor_msgs REQUIRED)

add_library(geograsp SHARED
    lib/geograsp/GeoGrasp.cpp
)

target_include_directories(geograsp
    PUBLIC
    # include
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

add_definitions(${EIGEN_DEFINITIONS})

add_executable(geograsp_exe src/cloud_processor.cpp)

ament_target_dependencies(geograsp
    ament_cmake
    rclcpp
    pcl_conversions
    pcl_ros
    eigen3_cmake_module
    Eigen3
    sensor_msgs
)

target_link_libraries(geograsp_exe geograsp)
target_link_libraries(geograsp pcl_visualization)

ament_export_targets(export_geograsp HAS_LIBRARY_TARGET)
ament_export_dependencies(
    ament_cmake
    rclcpp
    pcl_conversions
    pcl_ros
    eigen3_cmake_module
    Eigen3
    sensor_msgs
)

install(
    DIRECTORY include/
    DESTINATION include
)

install(
    TARGETS geograsp geograsp_exe
    EXPORT export_geograsp
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION lib/${PROJECT_NAME}
    INCLUDES DESTINATION include
)

ament_package()
