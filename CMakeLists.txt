cmake_minimum_required(VERSION 3.16)
project(vtk-gazebo-cross-talking)

option(SEARCH_FIRST_GAZEBO_THEN_VTK "Specify wheter to search first Gazebo and then VTK or the other way around" ON)

if(SEARCH_FIRST_GAZEBO_THEN_VTK)
  find_package(Gazebo REQUIRED)
endif()

find_package(VTK REQUIRED)
message (STATUS "VTK_VERSION: ${VTK_VERSION}")
if (VTK_VERSION VERSION_LESS "8.90.0")
  include(${VTK_USE_FILE})
endif()

if(NOT SEARCH_FIRST_GAZEBO_THEN_VTK)
  find_package(Gazebo REQUIRED)
endif()

add_executable(${PROJECT_NAME} ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_definitions(${PROJECT_NAME} PRIVATE _USE_MATH_DEFINES)
target_link_libraries(${PROJECT_NAME} PRIVATE ${VTK_LIBRARIES})

if (VTK_VERSION VERSION_GREATER_EQUAL "8.90.0")
  vtk_module_autoinit(TARGETS ${PROJECT_NAME} MODULES ${VTK_LIBRARIES})
endif()

install(TARGETS ${PROJECT_NAME} DESTINATION bin)
