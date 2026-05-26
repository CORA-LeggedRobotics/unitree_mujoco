cmake_minimum_required(VERSION 3.16)
project(stand_go2)
set(HOME_DIR $ENV{HOME})
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

#list(APPEND CMAKE_PREFIX_PATH "/opt/unitree_robotics/lib/cmake")
list(APPEND CMAKE_PREFIX_PATH "${HOME_DIR}/GO2_ws/unitree_sdk2/install/lib/cmake/unitree_sdk2")

find_package(unitree_sdk2 REQUIRED)

add_executable(stand_go2 stand_go2.cpp)
target_link_libraries(stand_go2  unitree_sdk2)

SET(CMAKE_BUILD_TYPE Release)