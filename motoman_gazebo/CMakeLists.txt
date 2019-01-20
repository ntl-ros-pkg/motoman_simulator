cmake_minimum_required(VERSION 2.8.3)
project(motoman_gazebo)
find_package(catkin REQUIRED)

catkin_package()


#############
## Install ##
#############
set(LAUNCH_DIRS
  launch/motomini
  launch/sda5
  launch/sia5
  launch/sia20
)
set(MODEL_DIRS
  models/meshes
  models/urdf
)
set(WORLD_DIRS
  worlds/motomini
  worlds/sda5
  worlds/sia5
  worlds/sia20
)
foreach(dir ${LAUNCH_DIRS} ${MODEL_DIRS} ${WORLD_DIRS})
  install(DIRECTORY ${dir}/
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach(dir)

#############
## Testing ##
#############
if (CATKIN_ENABLE_TESTING)
  find_package(roslaunch REQUIRED)
  roslaunch_add_file_check(launch/motomini)
  roslaunch_add_file_check(launch/sda5)
  roslaunch_add_file_check(launch/sia5)
  roslaunch_add_file_check(launch/sia20)
endif()
