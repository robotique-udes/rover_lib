cmake_minimum_required(VERSION 3.8)
project(rover_shared_libs LANGUAGES CXX)

find_package(rclcpp REQUIRED)
find_package(rover_msgs REQUIRED)
find_package(ament_cmake REQUIRED)

file(GLOB_RECURSE LIB_SOURCES CONFIGURE_DEPENDS
    rover_can2/src/*.cpp
    rover_lib2/src/*.cpp
)

add_library(rover_shared_libs STATIC ${LIB_SOURCES})

target_include_directories(rover_shared_libs PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/rover_can2/src>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/rover_lib2/src>
)

ament_target_dependencies(rover_shared_libs PUBLIC
    rclcpp
    rover_msgs
)

target_compile_features(rover_shared_libs PUBLIC cxx_std_23)
target_compile_options(rover_shared_libs PRIVATE -Wall -Wextra -Wpedantic)
target_compile_definitions(rover_shared_libs PUBLIC ROS)

install(TARGETS rover_shared_libs
        EXPORT rover_shared_libsTargets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        INCLUDES DESTINATION include)

install(DIRECTORY rover_can2/src/rover_can2
        DESTINATION include)

install(DIRECTORY rover_lib2/src/rover_lib2
        DESTINATION include)
