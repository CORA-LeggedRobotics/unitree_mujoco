cmake_minimum_required(VERSION 3.16)
project(unitree_mujoco)
set(HOME_DIR $ENV{HOME})
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

if (CMAKE_BUILD_TYPE MATCHES "Debug")
  message("Debug mode")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -g -O0 -fPIC")
else()
  message("Release mode")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -O3 -DNDEBUG -fPIC")
endif()

# -----------------------------------------------------------------------
#target link library for local installed mujoco
# -----------------------------------------------------------------------
set(MUJOCO_LOCAL_PATH "${CMAKE_CURRENT_SOURCE_DIR}/mujoco")


add_library(mujoco::mujoco SHARED IMPORTED GLOBAL)


set_target_properties(mujoco::mujoco PROPERTIES
    IMPORTED_LOCATION "${MUJOCO_LOCAL_PATH}/lib/libmujoco.so"
)


set_target_properties(mujoco::mujoco PROPERTIES
    INTERFACE_INCLUDE_DIRECTORIES "${MUJOCO_LOCAL_PATH}/include"
)
# -----------------------------------------------------------------------

#list(APPEND CMAKE_PREFIX_PATH "/opt/unitree_robotics/lib/cmake")
list(APPEND CMAKE_PREFIX_PATH "${HOME_DIR}/GO2_ws/unitree_sdk2/install/lib/cmake/unitree_sdk2")
find_package(unitree_sdk2 REQUIRED)
find_package(Boost REQUIRED COMPONENTS program_options)

file(GLOB SIM_SRC
    src/joystick/joystick.cc
    src/mujoco/*.cc
)

set(SIM_DEPENDENCIES
    pthread
    mujoco::mujoco
    glfw
    yaml-cpp
    unitree_sdk2
    boost_program_options
    fmt
)

add_executable(unitree_mujoco ${SIM_SRC} src/main.cc)
target_link_libraries(unitree_mujoco ${SIM_DEPENDENCIES})

add_executable(jstest src/joystick/jstest.cc src/joystick/joystick.cc)