# 
# Cubzh Core
# 
# Unit Tests target
#  

cmake_minimum_required(VERSION 3.4.1)

project("Cubzh Core - Unit Tests")

# validate environment
if(NOT DEFINED ENV{CUBZH_ARCH})
    message(FATAL_ERROR "env var CUBZH_ARCH is not defined, CMake will exit.")
endif()
set(DEPS_PLATFORM_NAME "linux-ubuntu-$ENV{CUBZH_ARCH}")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CUBZH_CORE_TESTS_DIR "${CMAKE_CURRENT_BINARY_DIR}/..")
set(CUBZH_CORE_ROOT_DIR "${CUBZH_CORE_TESTS_DIR}/..")
set(CUBZH_DEPS_DIR "${CUBZH_CORE_ROOT_DIR}/../deps")
set(SOURCE_FILES "")

# cubzh core source files
file(GLOB CUBZH_CORE_SOURCES
    CONFIGURE_DEPENDS
    ${CUBZH_CORE_ROOT_DIR}/*.c)
set(SOURCE_FILES ${SOURCE_FILES} ${CUBZH_CORE_SOURCES})

# unit tests source files
file(GLOB CUBZH_CORE_TESTS_SOURCES
    CONFIGURE_DEPENDS
    ${CUBZH_CORE_TESTS_DIR}/*.c)
set(SOURCE_FILES ${SOURCE_FILES} ${CUBZH_CORE_TESTS_SOURCES})

# zlib
set(LIBZ_INC_DIR "${CUBZH_DEPS_DIR}/libz/${DEPS_PLATFORM_NAME}/include")
set(LIBZ_LIB_DIR "${CUBZH_DEPS_DIR}/libz/${DEPS_PLATFORM_NAME}/libs")
# pre-compiled lib
find_library(LIBZ z ${LIBZ_LIB_DIR})

# Compile options
add_compile_options(
    -DDEBUG
)

# Search paths
include_directories(
    ${LIBZ_INC_DIR}
    ${CUBZH_CORE_ROOT_DIR}
)

add_executable(unit_tests ${SOURCE_FILES})

target_link_libraries(unit_tests
    ${LIBZ}
    m # libm (math)
)