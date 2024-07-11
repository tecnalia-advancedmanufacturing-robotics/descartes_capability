cmake_minimum_required(VERSION 3.5)
project(descartes_capability)

# C++ 14
add_compile_options(-std=c++17)

# Warnings
add_definitions(-W -Wall -Wextra -Wwrite-strings -Wunreachable-code
  -Wpointer-arith -Winit-self -Wredundant-decls -Wno-unused-parameter
  -Wno-unused-function)

# Find catkin macros and libraries
find_package(ament_cmake REQUIRED)
set(AMENT_DEPENDENCIES
  descartes_core
  descartes_moveit
  descartes_planner
  descartes_trajectory
  moveit_core
  moveit_ros_move_group
  moveit_ros_planning
  moveit_msgs
  moveit_visual_tools
  rviz_visual_tools
  # eigen_conversions
  rclcpp
)
foreach(DEP ${AMENT_DEPENDENCIES})
  find_package(${DEP} REQUIRED)
endforeach()

# System dependencies are found with CMake's conventions
find_package(Boost REQUIRED)
find_package(Eigen3 REQUIRED)

###################################
## Catkin specific configuration ##
###################################

###########
## Build ##
###########

# Specify additional locations of header files
# Your package locations should be listed before other locations
set(INCLUDE_DIRS include ${ament_cmake_INCLUDE_DIRS})
include_directories(${INCLUDE_DIRS})
# Declare a C++ library with project namespace to avoid naming collision
add_library(${PROJECT_NAME} src/descartes_path_service_capability.cpp)

ament_target_dependencies(${PROJECT_NAME} ${AMENT_DEPENDENCIES})
# Specify libraries to link a library or executable target against
target_link_libraries(${PROJECT_NAME} ${LIBS} ${Boost_LIBRARIES})

#############
## Install ##
#############

# Mark executables and/or libraries for installation
install(TARGETS ${PROJECT_NAME}
  ARCHIVE
  DESTINATION lib
  LIBRARY
  DESTINATION lib
  RUNTIME
  DESTINATION lib/${PROJECT_NAME})

# Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME})

# Mark config files for installation
install(DIRECTORY config DESTINATION share/${PROJECT_NAME})

# Mark python demo script for installation
install(PROGRAMS scripts/example_cartesian_command.py
  DESTINATION lib/${PROJECT_NAME})

install(FILES ${PROJECT_NAME}_plugin_description.xml
  DESTINATION share/${PROJECT_NAME})

#############
## Testing ##
#############

# if(BUILD_TESTING)
#   find_package(rostest REQUIRED)
#   find_package(pluginlib REQUIRED)

#   add_rostest_gtest(descartes_path_service_capability_test
#     test/descartes_path_service_capability_test.test
#     test/descartes_path_service_capability_test.cpp)
#   target_link_libraries(descartes_path_service_capability_test ${PROJECT_NAME}
#     ${LIBS} ${Boost_LIBRARIES})

# endif()

## Test for correct C++ source code
# roslint_cpp()

ament_export_dependencies(descartes_core)
ament_export_dependencies(eigen_conversions)
ament_export_dependencies(moveit_core)
ament_export_dependencies(descartes_moveit)
ament_export_dependencies(rclpy)
ament_export_dependencies(ament_cmake)
ament_export_dependencies(moveit_visual_tools)
ament_export_dependencies(rclcpp)
ament_export_dependencies(roslint)
ament_export_dependencies(descartes_trajectory)
ament_export_dependencies(descartes_planner)
ament_export_dependencies(moveit_ros_planning)
ament_export_dependencies(moveit_ros_move_group)
ament_export_include_directories(${INCLUDE_DIRS})
ament_export_libraries(${PROJECT_NAME} ${LIBS})

ament_package()
