cmake_minimum_required(VERSION 2.8.3)
project(ppl_detection)

set(CMAKE_BUILD_TYPE Debug)

find_package(catkin REQUIRED COMPONENTS 
    roscpp
    rospy
    std_msgs
    message_generation
    roslaunch
    tf
    pcl_ros
    laser_geometry
    visualization_msgs
    geometry_msgs
    genmsg
)

find_package(PCL)

add_message_files(
    FILES
    Human.msg
    Tracker.msg
)

generate_messages(
    DEPENDENCIES
    std_msgs
    geometry_msgs
    visualization_msgs
)

###################################
## catkin specific configuration ##
###################################

catkin_package(
    INCLUDE_DIRS include
    #  LIBRARIES ros_ssc32
    CATKIN_DEPENDS message_runtime
    #  DEPENDS system_lib
)

###########
## Build ##
###########

include_directories(
    ${catkin_INCLUDE_DIRS}
    ${PCL_INCLUDE_DIRS}
    include
)

add_library(ppl_detection 
    src/svm.cpp
    src/pca.cpp
    src/plyfile.c
)

add_dependencies(ppl_detection
    ppl_detection_generate_messages_cpp
)

add_executable(kinect_detect 
    src/kinect_detect.cpp
)

add_dependencies(kinect_detect
    ppl_detection_generate_messages_cpp
)

target_link_libraries(kinect_detect 
    ppl_detection
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
)

add_executable(train 
    src/train.cpp
)

add_dependencies(train
    ppl_detection_generate_messages_cpp
)

target_link_libraries(train 
    ppl_detection
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
)

add_executable(predict 
    src/predict.cpp
)

add_dependencies(predict
    ppl_detection_generate_messages_cpp
)

target_link_libraries(predict 
    ppl_detection
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
)

