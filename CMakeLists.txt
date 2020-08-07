cmake_minimum_required(VERSION 3.5)
project(asr_halcon_bridge)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

find_package(Halcon REQUIRED)
find_package(ament_cmake REQUIRED)
find_package(sensor_msgs REQUIRED)

add_library(${PROJECT_NAME}
  src/${PROJECT_NAME}/halcon_image.cpp
  #    src/${PROJECT_NAME}/halcon_pointcloud.cpp  # TODO: add migrate to ROS2 Foxy
)
target_include_directories(${PROJECT_NAME} PUBLIC 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> 
  $<INSTALL_INTERFACE:include>)
ament_target_dependencies(${PROJECT_NAME} Halcon sensor_msgs)

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY include/
    DESTINATION include
)

ament_export_include_directories(include ${Halcon_INCLUDE_DIRS})
ament_export_libraries(${PROJECT_NAME} ${Halcon_LIBRARIES})
ament_export_dependencies(sensor_msgs)
ament_package()
