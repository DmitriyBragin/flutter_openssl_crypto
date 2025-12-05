cmake_minimum_required(VERSION 3.6)
project(flutter_openssl_crypto)
# Add to CMakeLists.txt for debug


# Point to your custom 16KB OpenSSL
set(OPENSSL_ROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR}/android_openssl/ssl_3)
set(OPENSSL_USE_STATIC_LIBS OFF)  # Use shared libcrypto.so

# Alternative: Direct paths (more explicit)
find_library(crypto-lib
        NAMES crypto libcrypto
        PATHS ${CMAKE_CURRENT_SOURCE_DIR}/android_openssl/ssl_3
        NO_DEFAULT_PATH
)

message(STATUS "CMAKE_CURRENT_SOURCE_DIR: ${CMAKE_CURRENT_SOURCE_DIR}")
message(STATUS "Looking for libcrypto in: ${CMAKE_CURRENT_SOURCE_DIR}/android_openssl/ssl_3")
message(STATUS "Found: ${crypto-lib}")
message(STATUS "Custom libcrypto: ${crypto-lib}")

find_package(openssl REQUIRED CONFIG)
add_library(${PROJECT_NAME} SHARED empty.c)
# Link to YOUR 16KB libcrypto.so
target_link_libraries(${PROJECT_NAME} PRIVATE ${crypto-lib})

# Include OpenSSL headers (if needed)
target_include_directories(${PROJECT_NAME} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/android_openssl/ssl_3/include/openssl
)

# Include OpenSSL headers (if needed)
target_include_directories(${PROJECT_NAME} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/android_openssl/ssl_3/include/crypto
)
