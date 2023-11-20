cmake_minimum_required(VERSION 3.8)
project(cpp_listener)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

add_executable(listener src/main.cpp)
ament_target_dependencies(listener rclcpp std_msgs)

install(TARGETS
  listener
  DESTINATION lib/${PROJECT_NAME})

ament_package()