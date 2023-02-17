cmake_minimum_required(VERSION 2.8.3)
project(geograsp)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    add_compile_options(-Wall -Wunused-result -Wextra -Wpedantic)
endif()

#find_package(catkin REQUIRED COMPONENTS
  #pcl_conversions
  #pcl_ros
  #roscpp
  #sensor_msgs
#)

find_package(ament_cmake REQUIRED)
find_package(pcl_conversions REQUIRED)
find_package(pcl_ros REQUIRED)
find_package(roscpp REQUIRED)
find_package(sensor_msgs REQUIRED)

#catkin_package(
  #CATKIN_DEPENDS pcl_conversions pcl_ros roscpp sensor_msgs
  #INCLUDE_DIRS include
  #LIBRARIES GeoGrasp
#)

ament_export_dependencies(pcl_conversions pcl_ros roscpp sensor_msgs)
ament_export_include_directories(include)
ament_export_libraries(GeoGrasp)

include_directories(include)
add_library(GeoGrasp
  lib/${PROJECT_NAME}/GeoGrasp.cpp
)

add_executable(cloud_processor src/cloud_processor.cpp)

target_link_libraries(cloud_processor
  GeoGrasp
  ${catkin_LIBRARIES}
)

install(TARGETS GeoGrasp
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

install(DIRECTORY include/${PROJECT_NAME}/
    DESTINATION include/${PROJECT_NAME}
)
ament_package()
