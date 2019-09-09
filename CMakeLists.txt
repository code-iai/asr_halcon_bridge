cmake_minimum_required(VERSION 3.5)
project(asr_halcon_bridge)

if(NOT CMAKE_CXX_STANDARD)
	set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
	add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

find_package(Halcon REQUIRED)
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)

#
#catkin_package(
#	CATKIN_DEPENDS roscpp sensor_msgs
#	LIBRARIES ${PROJECT_NAME}
#        INCLUDE_DIRS include
#        DEPENDS Halcon
#)

include_directories(
	include
	${Halcon_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME}
    src/${PROJECT_NAME}/halcon_image.cpp
#    src/${PROJECT_NAME}/halcon_pointcloud.cpp
)

target_link_libraries(${PROJECT_NAME} ${Halcon_LIBRARIES})
ament_target_dependencies(${PROJECT_NAME} "rclcpp" "std_msgs")

install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

install(DIRECTORY include/${PROJECT_NAME}/
    DESTINATION include/${PROJECT_NAME}
)

ament_export_include_directories(include)
ament_export_include_directories(${Halcon_INCLUDE_DIRS})
ament_export_libraries(${PROJECT_NAME})
ament_export_libraries(${Halcon_LIBRARIES})
ament_export_dependencies(rclcpp)
ament_export_dependencies(std_msgs)
ament_package()
