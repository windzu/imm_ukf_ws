cmake_minimum_required(VERSION 3.0.2)
project(mot_tracking)
SET(CMAKE_BUILD_TYPE Debug)

# # Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

# SET(CMAKE_BUILD_TYPE "Release")
# # Find catkin macros and libraries
# # if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
# # is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  image_transport
  image_transport
  roscpp
  sensor_msgs
  std_msgs
  message_generation
)

# # System dependencies are found with CMake's conventions
find_package(Eigen3 REQUIRED)
find_package(OpenCV)
find_package(PCL REQUIRED)
find_package(Boost REQUIRED COMPONENTS filesystem thread system program_options)

add_definitions(${PCL_DEFINITIONS})

add_message_files(
  FILES
  detect.msg
  object.msg

  # Message1.msg
  # Message2.msg
)

# # Generate added messages and services with any dependencies listed here
generate_messages(
  DEPENDENCIES
  sensor_msgs
  std_msgs
  geometry_msgs
)

catkin_package(

  # INCLUDE_DIRS include
  # LIBRARIES image_handle_node
  CATKIN_DEPENDS cv_bridge image_transport roscpp sensor_msgs std_msgs geometry_msgs message_runtime

  # DEPENDS system_lib
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIR} ${Boost_INCLUDE_DIRS} ${PCL_INCLUDE_DIRS}
)

add_executable(tracking_node_ori src/testkitti.cpp src/tracker.cpp src/imm_ukf.cpp src/hungarianAlg.cpp src/ukf.cpp)
target_link_libraries(tracking_node_ori ${catkin_LIBRARIES} ${OpenCV_LIBS} ${Boost_LIBRARIES} ${PCL_LIBRARIES})
add_executable(data_send_node src/datasender_node.cpp src/datasend.cpp)
target_link_libraries(data_send_node ${catkin_LIBRARIES} ${OpenCV_LIBS} ${Boost_LIBRARIES} ${PCL_LIBRARIES})
add_executable(tracking_node src/track_node.cpp src/track_process.cpp src/tracker.cpp src/imm_ukf.cpp src/hungarianAlg.cpp src/ukf.cpp)
target_link_libraries(tracking_node ${catkin_LIBRARIES} ${OpenCV_LIBS} ${Boost_LIBRARIES} ${PCL_LIBRARIES})
