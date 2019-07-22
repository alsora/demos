cmake_minimum_required(VERSION 3.5)
project(action_tutorials)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(example_interfaces REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_action REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(rosidl_default_generators REQUIRED)

add_library(action_library SHARED
  src/fibonacci_action_client.cpp
  src/fibonacci_action_server.cpp)
rclcpp_components_register_node(action_library PLUGIN "action_tutorials::FibonacciActionClient" EXECUTABLE fibonacci_action_client)
rclcpp_components_register_node(action_library PLUGIN "action_tutorials::FibonacciActionServer" EXECUTABLE fibonacci_action_server)
ament_target_dependencies(action_library
  "rclcpp"
  "rclcpp_action"
  "rclcpp_components"
  "example_interfaces")

install(TARGETS
  action_library
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)

rosidl_generate_interfaces(${PROJECT_NAME}
  "action/Fibonacci.action"
)

install(
  PROGRAMS
    bin/fibonacci_action_client.py
    bin/fibonacci_action_server.py
  DESTINATION lib/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
