cmake_minimum_required(VERSION 3.14.4)
project(gw_tests_pkg)

# Default to C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

## dependencies
find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()

find_package(libloragw REQUIRED)

ament_auto_add_library(${PROJECT_NAME} SHARED
  src/gw_test.cpp
  )

ament_target_dependencies(${PROJECT_NAME}
  "libloragw"
  )

# target_link_libraries(${PROJECT_NAME} ${libloragw_LIBRARIES})

add_executable(gw_test src/gw_test.cpp)

target_include_directories(gw_test PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(gw_test PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17

target_link_libraries(gw_test ${libloragw_LIBRARIES})

install(TARGETS gw_test
  DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
