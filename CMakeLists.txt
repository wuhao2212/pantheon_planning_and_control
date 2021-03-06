cmake_minimum_required(VERSION 2.8.3)
project(rvb_mpc)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${PROJECT_SOURCE_DIR}/cmake_modules" )
set(CMAKE_CXX_FLAGS "-std=c++0x")

OPTION( GENERATE_ACADO_CPP
  "Build package with shared libraries."
  ON
)

find_package(catkin REQUIRED COMPONENTS
  nav_msgs
  roscpp
  rospy
  sensor_msgs
  std_msgs
  cv_bridge
  image_transport
  roslib
  tf
  rm3_ackermann_controller
  visualization_msgs
  message_generation
)

find_package(yaml-cpp REQUIRED)
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
message("OpenCV Include Dir: " ${OpenCV_INCLUDE_DIRS})
find_package(ACADO REQUIRED)
find_package(Eigen3 REQUIRED)

add_service_files(
  FILES
  Obstacles.srv
)

generate_messages(
  DEPENDENCIES
  std_msgs
)

catkin_package()

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${ACADO_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
  ${PROJECT_SOURCE_DIR}
)

#ASSIMP
find_package(ASSIMP REQUIRED)
if(ASSIMP_FOUND)
    include_directories(${ASSIMP_INCLUDE_DIR})
endif()

find_package(GLEW REQUIRED)
if (GLEW_FOUND)
    include_directories($(GLEW_INCLUDE_DIRS))
endif()

add_subdirectory(${PROJECT_SOURCE_DIR}/src)
