cmake_minimum_required(VERSION 3.5)
set(CMAKE_CXX_STANDARD 14)
project(vmc4_msgs)

# Some ROS version specific defines
if(DEFINED ENV{ROS_DISTRO})
  if ("$ENV{ROS_DISTRO}" STREQUAL "rolling")
    set(EXTRA_DEPS service_msgs)
  elseif ("$ENV{ROS_DISTRO}" STREQUAL "iron")
    set(EXTRA_DEPS service_msgs)
  elseif ("$ENV{ROS_DISTRO}" STREQUAL "humble")
  elseif ("$ENV{ROS_DISTRO}" STREQUAL "galactic")
  elseif ("$ENV{ROS_DISTRO}" STREQUAL "foxy")
  else()
    set(EXTRA_DEPS service_msgs)
  endif()
endif()

find_package(ament_cmake REQUIRED)
find_package(rosidl_default_runtime REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(std_msgs REQUIRED)

foreach (EXTRA_DEP IN LISTS EXTRA_DEPS)
  find_package(${EXTRA_DEP} REQUIRED)
endforeach()

# Locate the common code and messages
set(COMMON_DIR "${${PROJECT_NAME}_SOURCE_DIR}")


# Generate Message Files
set(MSG_DIR "${COMMON_DIR}/msg")
file(GLOB MSG_FILES RELATIVE "${MSG_DIR}"
    "${MSG_DIR}/*.msg")
list(TRANSFORM MSG_FILES PREPEND ${COMMON_DIR}:msg/)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${MSG_FILES}
  DEPENDENCIES std_msgs ${EXTRA_DEPS}
)

ament_export_dependencies(rosidl_default_runtime)
ament_package()