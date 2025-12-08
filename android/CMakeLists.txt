cmake_minimum_required(VERSION 3.6)
project(flutter_openssl_crypto)
# Add to CMakeLists.txt for debug


# Point to your custom 16KB OpenSSL
set(OPENSSL_ROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR}/android_openssl/ssl_3)
set(LIBCRYPTO_PATH ${CMAKE_CURRENT_SOURCE_DIR}/android_openssl/ssl_3)
set(OPENSSL_USE_STATIC_LIBS OFF)  # Use shared libcrypto.so

message(STATUS "CMAKE_CURRENT_SOURCE_DIR: ${CMAKE_CURRENT_SOURCE_DIR}")
message(STATUS "LIBCRYPTO_PATH: ${LIBCRYPTO_PATH}")
message(STATUS "ANDROID_ABI: ${ANDROID_ABI}")

# List directory contents
file(GLOB LIB_FILES "${LIBCRYPTO_PATH}/${ANDROID_ABI}/*.so")
message(STATUS "Files in ${LIBCRYPTO_PATH}/${ANDROID_ABI}: ${LIB_FILES}")
message(STATUS "ABI: ${ANDROID_ABI}")
message(STATUS "Searching: ${LIBCRYPTO_PATH}/${ANDROID_ABI}")

# Find for current ABI (CMake sets ANDROID_ABI)
find_library(crypto-lib
        NAMES crypto libcrypto libcrypto libcrypto.so
        PATHS ${LIBCRYPTO_PATH}/${ANDROID_ABI}
        NO_DEFAULT_PATH
)

message(STATUS "Found crypto-lib: ${crypto-lib}")

set(CRYPTO_PATH "${LIBCRYPTO_PATH}/${ANDROID_ABI}")
set(CRYPTO_FILE "${CRYPTO_PATH}/libcrypto.so")

if(EXISTS "${CRYPTO_FILE}")
    set(crypto-lib "${CRYPTO_FILE}")
    message(STATUS "✅ Direct found: ${crypto-lib}")
else()
    message(FATAL_ERROR "❌ libcrypto.so missing: ${CRYPTO_FILE}")
endif()

find_package(openssl REQUIRED CONFIG)
add_library(${PROJECT_NAME} SHARED empty.c)
# Link to YOUR 16KB libcrypto.so
target_link_libraries(${PROJECT_NAME} PRIVATE ${crypto-lib})