cmake_minimum_required(VERSION 3.6)
project(flutter_openssl_crypto)
# Force 16KB page alignment for ALL linked libraries
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,-z,max-page-size=16384")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wl,-z,max-page-size=16384")
find_package(openssl REQUIRED CONFIG)
add_library(${PROJECT_NAME} SHARED empty.c)
target_link_libraries(${PROJECT_NAME} PRIVATE openssl::crypto)
