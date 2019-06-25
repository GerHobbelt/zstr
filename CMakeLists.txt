cmake_minimum_required(VERSION 3.13)

project(zstr VERSION 1.0.1 LANGUAGES CXX)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(zstr INTERFACE) # an interface (not a library), to enable automatic include_directory

target_include_directories(zstr INTERFACE
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
    $<INSTALL_INTERFACE:include>
)

# CMake package
install(TARGETS zstr
        EXPORT exportedTargets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${CMAKE_PROJECT_NAME})

set(INSTALL_CONFIG_DIR ${CMAKE_INSTALL_LIBDIR}/cmake/${CMAKE_PROJECT_NAME})

install(EXPORT exportedTargets FILE ${CMAKE_PROJECT_NAME}Targets.cmake NAMESPACE Zstr:: DESTINATION ${INSTALL_CONFIG_DIR})
write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}ConfigVersion.cmake COMPATIBILITY SameMajorVersion)
configure_package_config_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/${CMAKE_PROJECT_NAME}Config.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}Config.cmake"
  INSTALL_DESTINATION ${INSTALL_CONFIG_DIR})
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}Config.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}ConfigVersion.cmake
  DESTINATION ${INSTALL_CONFIG_DIR})

# Exporting from the build tree
export(EXPORT exportedTargets FILE ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}Targets.cmake NAMESPACE Zstr::)
export(PACKAGE ${CMAKE_PROJECT_NAME})
