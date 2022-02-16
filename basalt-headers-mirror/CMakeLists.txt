cmake_minimum_required(VERSION 3.14)
project(basalt)

find_package(catkin REQUIRED roslint)
find_package(Eigen3 REQUIRED)
find_package(basalt)

catkin_package(
  INCLUDE_DIRS
    include
  LIBRARIES
    ${PROJECT_NAME}
  CATKIN_DEPENDS
  DEPENDS
    CERES
    EIGEN3
)

set(ROSLINT_CPP_OPTS "--filter=-build/c++14,-runtime/references")
roslint_cpp()

###########
## Build ##
###########
add_compile_options(-Wall -Werror)

## Declare a C++ library
add_library(${PROJECT_NAME}
  #src/calibration.cpp
  #src/camera.cpp
  #src/image.cpp
  src/imu.cpp
  src/spline.cpp
)

target_include_directories(${PROJECT_NAME}
  PUBLIC
    include
    ${catkin_INCLUDE_DIRS}
    ${CERES_INCLUDE_DIRS}
    ${EIGEN3_INCLUDE_DIRS}
)
target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
  ${CERES_LIBRARIES}
)
set_target_properties(${PROJECT_NAME}
  PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED YES
)

#############
## Install ##
#############

## Mark executables and/or libraries for installation
install(
  TARGETS
    ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark cpp header files for installation
install(
  DIRECTORY
    include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)

#############
## Testing ##
#############

if(CATKIN_ENABLE_TESTING)
  find_package(roslint REQUIRED)
  find_package(rostest REQUIRED)

  # Lint tests
  roslint_add_test()

  # Preintegration tests
  catkin_add_gtest(test_preintegration
    test/test_preintegration.cpp
  )
  target_link_libraries(test_preintegration
    ${PROJECT_NAME}
    ${catkin_LIBRARIES}
    ${CERES_LIBRARIES}
  )
  set_target_properties(test_preintegration
    PROPERTIES
      CXX_STANDARD 14
      CXX_STANDARD_REQUIRED YES
  )

  # Sophus tests
  catkin_add_gtest(test_sophus
    test/test_sophus.cpp
  )
  target_link_libraries(test_sophus
    ${PROJECT_NAME}
    ${catkin_LIBRARIES}
    ${CERES_LIBRARIES}
  )
  set_target_properties(test_sophus
    PROPERTIES
      CXX_STANDARD 14
      CXX_STANDARD_REQUIRED YES
  )

  # Spline SE3 tests
  catkin_add_gtest(test_spline_se3
    test/test_spline_se3.cpp
  )
  target_link_libraries(test_spline_se3
    ${PROJECT_NAME}
    ${catkin_LIBRARIES}
    ${CERES_LIBRARIES}
  )
  set_target_properties(test_spline_se3
    PROPERTIES
      CXX_STANDARD 14
      CXX_STANDARD_REQUIRED YES
  )

  # Spline tests
  catkin_add_gtest(test_spline
    test/test_spline.cpp
  )
  target_link_libraries(test_spline
    ${PROJECT_NAME}
    ${catkin_LIBRARIES}
    ${CERES_LIBRARIES}
  )
  set_target_properties(test_spline
    PROPERTIES
      CXX_STANDARD 14
      CXX_STANDARD_REQUIRED YES
  )
endif()
