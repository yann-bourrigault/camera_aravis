cmake_minimum_required(VERSION 2.8.3)
project(camera_aravis)

#set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules)
find_package(catkin REQUIRED COMPONENTS
    roscpp
    sensor_msgs
    image_transport
    camera_info_manager
    dynamic_reconfigure
    driver_base
    tf
)

include(FindPkgConfig)
pkg_check_modules(Aravis aravis)
pkg_check_modules(GLIB2 glib-2.0)

generate_dynamic_reconfigure_options(
    cfg/CameraAravis.cfg
)

catkin_package(
    INCLUDE_DIRS
    LIBRARIES
    CATKIN_DEPENDS roscpp sensor_msgs image_transport camera_info_manager dynamic_reconfigure driver_base tf
    DEPENDS Aravis GLIB2
)

include_directories(
    ${catkin_INCLUDE_DIRS}
    ${ARAVIS_INCLUDE_PATH}
    ${GLIB2_INCLUDE_DIRS}
)

add_executable(camnode
    src/camnode.cpp
)
add_dependencies(camnode
    ${PROJECT_NAME}_gencfg
)

target_link_libraries(camnode
    ${catkin_LIBRARIES}
    ${ARAVIS_LIBRARY}
    glib-2.0
    gmodule-2.0
    gobject-2.0
)

install(TARGETS camnode
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
