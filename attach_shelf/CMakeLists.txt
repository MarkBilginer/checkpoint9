cmake_minimum_required(VERSION 3.8)
project(attach_shelf)

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
find_package(rclpy REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(custom_interfaces REQUIRED)

# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

add_executable(pre_approach src/pre_approach.cpp)
ament_target_dependencies(pre_approach rclcpp geometry_msgs sensor_msgs nav_msgs tf2 tf2_geometry_msgs)

add_executable(pre_approach_v2 src/pre_approach_v2.cpp)
ament_target_dependencies(pre_approach_v2 rclcpp geometry_msgs sensor_msgs nav_msgs tf2 tf2_geometry_msgs custom_interfaces)

add_executable(approach_service_server src/approach_service_server.cpp)
ament_target_dependencies(approach_service_server rclcpp std_msgs sensor_msgs nav_msgs geometry_msgs tf2 tf2_geometry_msgs custom_interfaces)

# Install the executable and necessary directories
install(TARGETS
  pre_approach
  pre_approach_v2
  approach_service_server
  DESTINATION lib/${PROJECT_NAME}
)

install(DIRECTORY
  launch
  rviz_config
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
