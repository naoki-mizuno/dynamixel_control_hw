cmake_minimum_required(VERSION 2.8.3)
project(dynamixel_control_hw)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(LIBDYNAMIXEL "")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  message_generation
  hardware_interface
  controller_manager
)

catkin_package(
 CATKIN_DEPENDS roscpp std_msgs message_runtime
)

if ("$ENV{RESIBOTS_DIR}" STREQUAL "" AND "${LIBDYNAMIXEL}" STREQUAL "")
  message("Using default location for libdynamixel : ~")
  set(libdynamixel_dir "~")
elseif (NOT ("${LIBDYNAMIXEL}" STREQUAL ""))
  message("Using user-defined location for libdynamixel : ${LIBDYNAMIXEL}")
  set(libdynamixel_dir "${LIBDYNAMIXEL}")
elseif (NOT ("$ENV{RESIBOTS_DIR}" STREQUAL ""))
  message("Using enviromnetal-defined location for libdynamixel : $ENV{RESIBOTS_DIR}")
  set(libdynamixel_dir "$ENV{RESIBOTS_DIR}")
endif()

if (NOT (EXISTS "${libdynamixel_dir}/include/dynamixel/"))
  message(FATAL_ERROR "libdynamixel was not found!")
endif()

include_directories(
  ${catkin_INCLUDE_DIRS}
  "${libdynamixel_dir}/include"
  include
)

add_executable(dynamixel_control_hw src/dynamixel_control_hw.cpp src/dynamixel_loop.cpp src/dynamixel_hardware_interface.cpp)
target_link_libraries(dynamixel_control_hw ${catkin_LIBRARIES})
