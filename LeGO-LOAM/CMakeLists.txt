cmake_minimum_required( VERSION 3.1 )
project(lego_loam)


# Compiler options
set( CMAKE_CXX_STANDARD 11 )


# ROS find package tedium
find_package(
    catkin REQUIRED
    COMPONENTS
        message_generation
        message_runtime
        pcl_conversions
        pcl_ros
        roscpp
        tf

        geometry_msgs
        nav_msgs
        sensor_msgs
        std_msgs
)


# Generate messages
add_message_files(
    DIRECTORY msg
    FILES cloud_info.msg
)

generate_messages(
    DEPENDENCIES
    geometry_msgs
    std_msgs
    nav_msgs
)


# Find third party packages
find_package(GTSAM REQUIRED QUIET)
find_package(PCL REQUIRED QUIET)
find_package(OpenCV REQUIRED QUIET)


# ROS package tedium
catkin_package(
    INCLUDE_DIRS include
    CATKIN_DEPENDS
        message_generation
        message_runtime
        pcl_conversions
        pcl_ros
        roscpp
        tf

        geometry_msgs
        nav_msgs
        sensor_msgs
        std_msgs
    DEPENDS PCL
)

include_directories(
	include
	${catkin_INCLUDE_DIRS}
	${PCL_INCLUDE_DIRS}
	${OpenCV_INCLUDE_DIRS}
	${GTSAM_INCLUDE_DIR}
)


# Build executables
add_executable(
    imageProjection
    src/imageProjection.cpp
)
target_link_libraries(
    imageProjection
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
    ${OpenCV_LIBRARIES}
)

add_executable(
    featureAssociation
    src/featureAssociation.cpp
)
target_link_libraries(
    featureAssociation
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
    ${OpenCV_LIBRARIES}
)

add_executable(
    mapOptmization
    src/mapOptmization.cpp
)
target_link_libraries(
    mapOptmization
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
    ${OpenCV_LIBRARIES}
    gtsam
)

add_executable(
    transformFusion
    src/transformFusion.cpp
)
target_link_libraries(
    transformFusion
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
    ${OpenCV_LIBRARIES}
)
