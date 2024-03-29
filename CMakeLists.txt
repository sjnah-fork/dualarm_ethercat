cmake_minimum_required(VERSION 2.8.3)
project(ethercat_test)

execute_process(COMMAND /usr/xenomai/bin/xeno-config --skin=alchemy --cflags OUTPUT_VARIABLE XENO_CFLAGS OUTPUT_STRIP_TRAILING_WHITESPACE)
execute_process(COMMAND /usr/xenomai/bin/xeno-config --skin=alchemy --ldflags OUTPUT_VARIABLE XENO_LDFLAGS OUTPUT_STRIP_TRAILING_WHITESPACE)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${XENO_CFLAGS}")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${XENO_LDFLAGS}")

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  soem
  std_msgs
        genmsg
        message_generation
)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)


## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

################################################
## Declare ROS messages, services and actions ##
################################################

## To declare and build messages, services or actions from within this
## package, follow these steps:
## * Let MSG_DEP_SET be the set of packages whose message types you use in
##   your messages/services/actions (e.g. std_msgs, actionlib_msgs, ...).
## * In the file package.xml:
##   * add a build_depend tag for "message_generation"
##   * add a build_depend and a exec_depend tag for each package in MSG_DEP_SET
##   * If MSG_DEP_SET isn't empty the following dependency has been pulled in
##     but can be declared for certainty nonetheless:
##     * add a exec_depend tag for "message_runtime"
## * In this file (CMakeLists.txt):
##   * add "message_generation" and every package in MSG_DEP_SET to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * add "message_runtime" and every package in MSG_DEP_SET to
##     catkin_package(CATKIN_DEPENDS ...)
##   * uncomment the add_*_files sections below as needed
##     and list every .msg/.srv/.action file to be processed
##   * uncomment the generate_messages entry below
##   * add every package in MSG_DEP_SET to generate_messages(DEPENDENCIES ...)

## Generate messages in the 'msg' folder
 add_message_files(
   FILES
   pos.msg
   vel.msg
   trajectory.msg
#   Message1.msg
#   Message2.msg
 )

## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )

## Generate actions in the 'action' folder
# add_action_files(
#   FILES
#   Action1.action
#   Action2.action
# )

## Generate added messages and services with any dependencies listed here
 generate_messages(
   DEPENDENCIES
   std_msgs
 )

################################################
## Declare ROS dynamic reconfigure parameters ##
################################################

## To declare and build dynamic reconfigure parameters within this
## package, follow these steps:
## * In the file package.xml:
##   * add a build_depend and a exec_depend tag for "dynamic_reconfigure"
## * In this file (CMakeLists.txt):
##   * add "dynamic_reconfigure" to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * uncomment the "generate_dynamic_reconfigure_options" section below
##     and list every .cfg file to be processed

## Generate dynamic reconfigure parameters in the 'cfg' folder
# generate_dynamic_reconfigure_options(
#   cfg/DynReconf1.cfg
#   cfg/DynReconf2.cfg
# )

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if your package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES ethercat_test
#  CATKIN_DEPENDS roscpp rospy soem std_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
 include
  ${catkin_INCLUDE_DIRS}
)

add_library(SERVO src/servo_def.c)
add_library(ECAT_DC src/ecat_dc.c)


#add_executable(ethercat_control src/ethercat_control.cpp)
#target_link_libraries(ethercat_control ${catkin_LIBRARIES} SERVO ECAT_DC)
#add_dependencies(ethercat_control ethercat_test_generate_messages_cpp)

#add_executable(mani_ecat_control src/mani_ecat_control.cpp)
#target_link_libraries(mani_ecat_control ${catkin_LIBRARIES} SERVO ECAT_DC)
#add_dependencies(mani_ecat_control ethercat_test_generate_messages_cpp)

#add_executable(ecat_profile_pos src/ecat_profile_pos.cpp)
#target_link_libraries(ecat_profile_pos ${catkin_LIBRARIES} SERVO ECAT_DC)
#add_dependencies(ecat_profile_pos ethercat_test_generate_messages_cpp)

add_executable(ecat_profile_pos_last src/ecat_profile_pos_last.cpp)
target_link_libraries(ecat_profile_pos_last ${catkin_LIBRARIES} SERVO ECAT_DC)
add_dependencies(ecat_profile_pos_last ethercat_test_generate_messages_cpp)

add_executable(ecat_profile_pos_2 src/ecat_profile_pos_2.cpp)
target_link_libraries(ecat_profile_pos_2 ${catkin_LIBRARIES} SERVO ECAT_DC)
add_dependencies(ecat_profile_pos_2 ethercat_test_generate_messages_cpp)

add_executable(ecat_profile_pos_2_changed src/ecat_profile_pos_2_changed.cpp)
target_link_libraries(ecat_profile_pos_2_changed ${catkin_LIBRARIES} SERVO ECAT_DC)
add_dependencies(ecat_profile_pos_2_changed ethercat_test_generate_messages_cpp)


add_executable(ethercat_mobile_control src/ethercat_mobile_control.cpp)
target_link_libraries(ethercat_mobile_control ${catkin_LIBRARIES} SERVO ECAT_DC)
add_dependencies(ethercat_mobile_control ethercat_test_generate_messages_cpp)

add_executable(dualarm_ecat_ctrl_j src/dualarm_ecat_ctrl_j.cpp)
target_link_libraries(dualarm_ecat_ctrl_j ${catkin_LIBRARIES} SERVO ECAT_DC)
add_dependencies(dualarm_ecat_ctrl_j ethercat_test_generate_messages_cpp)

add_executable(mani_ecat_homing src/mani_ecat_homing.cpp)
target_link_libraries(mani_ecat_homing ${catkin_LIBRARIES} SERVO ECAT_DC m)
add_dependencies(mani_ecat_homing ethercat_test_generate_messages_cpp)

add_executable(mani_ecat_homing_j src/mani_ecat_homing_j.cpp)
target_link_libraries(mani_ecat_homing_j ${catkin_LIBRARIES} SERVO ECAT_DC m)
add_dependencies(mani_ecat_homing_j ethercat_test_generate_messages_cpp)

add_executable(ethercat_mobile_test src/ethercat_mobile_test.cpp)
target_link_libraries(ethercat_mobile_test ${catkin_LIBRARIES} SERVO ECAT_DC m)
add_dependencies(ethercat_mobile_test ethercat_test_generate_messages_cpp)

add_executable(singlearm_ecat_ctrl_j src/singlearm_ecat_ctrl_j.cpp)
target_link_libraries(singlearm_ecat_ctrl_j ${catkin_LIBRARIES} SERVO ECAT_DC m)

add_executable(singlearm_ecat_ctrl_j_mod src/singlearm_ecat_ctrl_j_mod.cpp)
target_link_libraries(singlearm_ecat_ctrl_j_mod ${catkin_LIBRARIES} SERVO ECAT_DC m)

## Declare a C++ library
# add_library(${PROJECT_NAME}
#   src/${PROJECT_NAME}/ethercat_test.cpp
# )

## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
## With catkin_make all packages are built within a single CMake context
## The recommended prefix ensures that target names across packages don't collide
# add_executable(${PROJECT_NAME}_node src/ethercat_test_node.cpp)

## Rename C++ executable without prefix
## The above recommended prefix causes long target names, the following renames the
## target back to the shorter version for ease of user use
## e.g. "rosrun someones_pkg node" instead of "rosrun someones_pkg someones_pkg_node"
# set_target_properties(${PROJECT_NAME}_node PROPERTIES OUTPUT_NAME node PREFIX "")

## Add cmake target dependencies of the executable
## same as for the library above
# add_dependencies(${PROJECT_NAME}_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
# target_link_libraries(${PROJECT_NAME}_node
#   ${catkin_LIBRARIES}
# )

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_executables.html
# install(TARGETS ${PROJECT_NAME}_node
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark libraries for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_libraries.html
# install(TARGETS ${PROJECT_NAME}
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_ethercat_test.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
