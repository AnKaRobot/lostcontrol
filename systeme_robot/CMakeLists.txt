cmake_minimum_required(VERSION 2.8.3)
project(systeme_robot)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS roscpp rospy std_msgs genmsg message_generation)

#######################################
## Declare ROS messages and services ##
#######################################

## Generate messages in the 'msg' folder

add_message_files(DIRECTORY msg FILES Telecommande_trajectoire.msg)
add_message_files(DIRECTORY msg FILES Autopilote_trajectoire.msg)

add_message_files(
   DIRECTORY msg
   FILES
   Gps_trajectoire.msg
   Accelerometre_trajectoire.msg
 )

## Generate added messages and services with any dependencies listed here
generate_messages(DEPENDENCIES std_msgs)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
   INCLUDE_DIRS include
   CATKIN_DEPENDS roscpp rospy std_msgs message_runtime
   DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(include ${catkin_INCLUDE_DIRS})


add_executable(telecommande src/telecommande.cpp)
target_link_libraries(telecommande ${catkin_LIBRARIES})
add_dependencies(telecommande system_robot_generate_message_cpp)

add_executable(trajectoire src/trajectoire.cpp)
target_link_libraries(trajectoire ${catkin_LIBRARIES})
add_dependencies(trajectoire system_robot_generate_message_cpp)

add_executable(autopilote src/autopilote.cpp)
target_link_libraries(autopilote ${catkin_LIBRARIES})
add_dependencies(autopilote system_robot_generate_message_cpp)


## Declare a cpp executable
# add_executable(telecommande_robot_node src/telecommande_robot_node.cpp)

## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
# add_dependencies(telecommande_robot_node telecommande_robot_generate_messages_cpp)

## Specify libraries to link a library or executable target against
# target_link_libraries(telecommande_robot_node
#   ${catkin_LIBRARIES}
# )

install(PROGRAMS scripts/daguserie.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
  
