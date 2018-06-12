cmake_minimum_required (VERSION 2.8)
project (pixyusb CXX)
message(cmake pixy)

set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake" )


find_package ( libusb-1.0 REQUIRED )

# Define Operating System #

IF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
add_definitions(-D__MACOS__)
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")

IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
add_definitions(-D__LINUX__)
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")


file(STRINGS "cmake/VERSION" LIBPIXY_VERSION)
add_definitions(-D__LIBPIXY_VERSION__="${LIBPIXY_VERSION}")


add_library (${PROJECT_NAME} STATIC src/chirpreceiver.cpp
                            src/pixyinterpreter.cpp
                            src/pixy.cpp
                            src/usblink.cpp
                            src/utils/timer.cpp
                            ../../common/src/chirp.cpp)

set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 11)
target_include_directories (${PROJECT_NAME} PUBLIC
                     src
                     src/util
                     include
                     ../../common/inc
                     ${LIBUSB_1_INCLUDE_DIRS})
