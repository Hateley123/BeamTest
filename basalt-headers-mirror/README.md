## Basalt Headers

This repository is a custom fork of https://github.com/BEAMRobotics/basalt-headers-mirror. This repository differs in the following ways:
1. It is compiled with the c++ 14 standard
2. The serialization library has been removed. This library is not required by Beam Robotics.
3. the submodules within the test folder have been removed. All dependencies of this custom fork have been included in beam_install_scripts.
4. The thirdparty folder has been removed. See 3. for note on dependencies.
5. The scripts and doc folders have been removed. Other Non-licensing files have been removed.
6. The cmake_modules folder has been removed as this package is meant to be compiled as a catkin package within a workspace
7. tests have been integrated using catkin. 

Currently, only imu and spline libraries are built (with tests) as they are required by beam_slam. Compiling the remaining libraries will require upgrading to c++20
