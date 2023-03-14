cmake_minimum_required(VERSION 3.8)
project(simulator)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(gazebo_ros REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(xacro REQUIRED)
find_package(rclpy REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

add_executable(talker scripts/talker.cpp)
ament_target_dependencies(talker rclcpp std_msgs)

add_executable(listener scripts/listener.cpp)
ament_target_dependencies(listener rclcpp std_msgs)


install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})

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

# Generate .urdf files from .urdf.xacro files
# file(GLOB xacro_files urdf/*.urdf.xacro)

# foreach(it ${xacro_files})
#   # remove .xacro extension
#   string(REGEX MATCH "(.*)[.]xacro$" unused ${it})
#   set(output_filename ${CMAKE_MATCH_1})

#   # create a rule to generate ${output_filename} from {it}
#   xacro_add_xacro_file(${it} ${output_filename})

#   list(APPEND urdf_files ${output_filename})
# endforeach(it)

# add an abstract target to actually trigger the builds
add_custom_target(media_files ALL DEPENDS ${urdf_files})

# Install launch files.
install(
  DIRECTORY
    launch
    worlds
    urdf
    scripts
    meshes
    config
  DESTINATION share/${PROJECT_NAME}/
)


ament_package()
