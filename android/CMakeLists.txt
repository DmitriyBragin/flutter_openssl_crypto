cmake_minimum_required(VERSION 3.6)
project(flutter_openssl_crypto)
# Add to CMakeLists.txt for debug


# Point to your custom 16KB OpenSSL
set(OPENSSL_ROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR}/android_openssl/ssl_3)
set(LIBCRYPTO_PATH ${CMAKE_CURRENT_SOURCE_DIR}/android_openssl/ssl_3)
set(OPENSSL_USE_STATIC_LIBS OFF)  # Use shared libcrypto.so

# Find for current ABI (CMake sets ANDROID_ABI)
find_library(crypto-lib
        NAMES crypto libcrypto
        PATHS ${LIBCRYPTO_PATH}/${ANDROID_ABI}
        NO_DEFAULT_PATH
        REQUIRED
)
message(STATUS "ABI: ${ANDROID_ABI}")
message(STATUS "Searching: ${LIBCRYPTO_PATH}/${ANDROID_ABI}")
message(STATUS "Found crypto-lib: ${crypto-lib}")


find_package(openssl REQUIRED CONFIG)
add_library(${PROJECT_NAME} SHARED empty.c)
# Link to YOUR 16KB libcrypto.so
target_link_libraries(${PROJECT_NAME} PRIVATE ${crypto-lib})