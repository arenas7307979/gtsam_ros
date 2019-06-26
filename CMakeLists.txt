cmake_minimum_required(VERSION 2.8.3)
project(gtsam_ros)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

# The configure step fails at catkin_package() if this directory
# doesn't exist yet. Create it at configure time!
# file(MAKE_DIRECTORY ${CMAKE_INSTALL_PREFIX}/include)

ExternalProject_Add(gtsam_src
  GIT_REPOSITORY https://github.com/borglab/gtsam
  GIT_TAG c82fe1fde2fc988b6bde5e4798b66129bbb5da19 # 4.0.0-alpha2
  CONFIGURE_COMMAND cmake 
    -DCMAKE_BUILD_TYPE=Release
    -DCMAKE_INSTALL_PREFIX=/usr/local #${CATKIN_DEVEL_PREFIX}
    -DGTSAM_USE_SYSTEM_EIGEN=ON
    ../gtsam_src
  BUILD_COMMAND $(MAKE) --silent
  INSTALL_COMMAND $(MAKE) install
)

cs_install()
cs_export(INCLUDE_DIRS /usr/local/include
          LIBRARIES gtsam tbb)
