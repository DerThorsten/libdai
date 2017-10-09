cmake_minimum_required (VERSION 2.8)
project(LIBDAI)


# default build-type (release)
IF(DEFINED CMAKE_BUILD_TYPE)
   SET(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE} CACHE STRING "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel.")
ELSE()
   SET(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel.")
ENDIF()


set(${PROJECT_NAME}_VERSION_MAJOR 0)
set(${PROJECT_NAME}_VERSION_MINOR 3)
set(${PROJECT_NAME}_VERSION_PATCH 8)




#---------------------------------------------------
# Boost
#---------------------------------------------------
find_package(Boost REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})









set(${PROJECT_NAME}_VERSION 
    ${${PROJECT_NAME}_VERSION_MAJOR}.${${PROJECT_NAME}_VERSION_MINOR}.${${PROJECT_NAME}_VERSION_PATCH}
)

file(GLOB ${PROJECT_NAME}_SRC_FILES 
  ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp
  #EXCLUDE matlab
)

file(GLOB ${PROJECT_NAME}_HEADERS 
  ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h
  #EXCLUDE matlab
)


include_directories(include)
ADD_LIBRARY(libdai SHARED ${${PROJECT_NAME}_SRC_FILES})


include(GNUInstallDirs)
include(CMakePackageConfigHelpers)


install(TARGETS libdai 
    DESTINATION ${CMAKE_INSTALL_LIBDIR})

install(FILES ${${PROJECT_NAME}_HEADERS}
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/libdai)


set(${PROJECT_NAME}_CMAKECONFIG_INSTALL_DIR "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}" CACHE
    STRING "install path for ${PROJECT_NAME}Config.cmake")

configure_package_config_file(${PROJECT_NAME}Config.cmake.in
                              "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
                              INSTALL_DESTINATION ${${PROJECT_NAME}_CMAKECONFIG_INSTALL_DIR})


write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
                                 VERSION ${${PROJECT_NAME}_VERSION}
                                 COMPATIBILITY AnyNewerVersion)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
              ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
        DESTINATION ${${PROJECT_NAME}_CMAKECONFIG_INSTALL_DIR})
