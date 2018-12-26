cmake_minimum_required(VERSION 2.8.3)
project(motoman_control)

add_definitions("-std=c++11")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  actionlib
  actionlib_msgs
  trajectory_msgs
  control_msgs
  sensor_msgs
)

catkin_package(
  CATKIN_DEPENDS actionlib_msgs
)

###########
## Build ##
###########
include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_executable(motoman_control_node
  src/motoman_control_node.cpp
)

add_dependencies(motoman_control_node motoman_control_generate_messages_cpp)

target_link_libraries(motoman_control_node
  ${catkin_LIBRARIES}
)

#############
## Install ##
#############
# CPP codes
foreach(nodes motoman_control_node)
  install(TARGETS ${nodes}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
    )
endforeach(nodes)

# Python scripts
set(PYTHON_SCRPTS
  scripts/move_to_joint_real.py
  scripts/move_to_joint_sim.py
)
foreach(scripts ${PYTHON_SCRIPTS})
  catkin_install_python(PROGRAMS ${scripts}
    DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
endforeach(scripts)

# Install all files
set(CONFIG_DIRS
  config/sda5
  config/sia5
  config/sia20
  )
set(LAUNCH_DIRS
  launch/sda5
  launch/sia5
  launch/sia20
)

foreach(dir ${CONFIG_DIRS} ${LAUNCH_DIRS})
  install(DIRECTORY ${dir}/
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach(dir)

#############
## Testing ##
#############
if (CATKIN_ENABLE_TESTING)
  find_package(roslaunch REQUIRED)
  roslaunch_add_file_check(launch/sda5)
  roslaunch_add_file_check(launch/sia5)
  roslaunch_add_file_check(launch/sia20)
endif()
