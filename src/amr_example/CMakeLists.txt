cmake_minimum_required(VERSION 3.5)
project(amr_example)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(custom_msg REQUIRED)
find_package(visualization_msgs REQUIRED)


if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

add_executable(OrderOptimizer src/OrderOptimizer.cpp)
ament_target_dependencies(OrderOptimizer rclcpp geometry_msgs std_msgs custom_msg visualization_msgs)

target_link_libraries(OrderOptimizer yaml-cpp stdc++fs)

add_executable(unit_test_node src/unit_test.cpp)
ament_target_dependencies(unit_test_node rclcpp geometry_msgs std_msgs custom_msg visualization_msgs)

add_executable(publish_order src/publish_order.cpp)
ament_target_dependencies(publish_order rclcpp geometry_msgs std_msgs custom_msg)

target_link_libraries(unit_test_node yaml-cpp stdc++fs)

install(TARGETS
  OrderOptimizer
  unit_test_node
  publish_order
  DESTINATION lib/${PROJECT_NAME})

ament_package()