cmake_minimum_required(VERSION 3.10)

project("SafeheronCryptoSuites" VERSION 1.0.1)

set(CMAKE_CXX_STANDARD 11)
#SET(CMAKE_BUILD_TYPE "Release")
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -Wall -Wextra -Wno-unused-parameter -Wno-unused-variable -fmacro-prefix-map=${CMAKE_SOURCE_DIR}=/safeheron")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -Wall -Wextra -Wno-unused-parameter -Wno-unused-variable -fmacro-prefix-map=${CMAKE_SOURCE_DIR}=/safeheron")

cmake_policy(SET CMP0076 NEW)
cmake_policy(SET CMP0077 NEW)
cmake_policy(SET CMP0079 NEW)

#find_package(PkgConfig REQUIRED)
#pkg_search_module(PROTOBUF REQUIRED protobuf)  # depend on pkg-config, this looks for opencv.pc file
#set(OPENSSL_USE_STATIC_LIBS TRUE)
#find_package(OpenSSL REQUIRED)


 set(PROTOBUF_INCLUDE_DIRS  "${CMAKE_PREFIX_PATH}/include/")
 set(PROTOBUF_LIBRARY_DIRS  "${CMAKE_PREFIX_PATH}/lib/")
 set(PROTOBUF_LIBRARIES  "${CMAKE_PREFIX_PATH}/lib/libprotobuf.a")


 #设置openssl的库头文件
 set(OPENSSL_INCLUDE_DIR  "${CMAKE_PREFIX_PATH}/include/")
 #设置openssl的库路径
 set(OPENSSL_SSL_LIBRARY "${CMAKE_PREFIX_PATH}/lib/libssl.a")
 #设置openssl的工具库
 set(OPENSSL_CRYPTO_LIBRARY "${CMAKE_PREFIX_PATH}/lib/libcrypto.a")


add_library(${CMAKE_PROJECT_NAME} "")

target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
        src/crypto-bn-cpp/src
        src/crypto-hash-cpp/src
        src/crypto-encode-cpp/src
        src/crypto-curve-cpp/src
        src/crypto-commitment-cpp/src
        src/crypto-paillier-cpp/src
        src/crypto-sss-cpp/src
        src/crypto-zkp-cpp/src
        src/crypto-bip32-cpp/src
        src/crypto-ecies-cpp/src
        src/crypto-bip39-cpp/src
        ${PROTOBUF_INCLUDE_DIRS}
        ${OPENSSL_INCLUDE_DIR}
        /usr/local/include
        )
message(2324r2534${PROTOBUF_LIBRARY_DIRS})
target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE ${CMAKE_PREFIX_PATH}/lib)
target_link_libraries(${CMAKE_PROJECT_NAME} PUBLIC
        ${PROTOBUF_LIBRARIES}
        OPENSSL_CRYPTO_LIBRARY
        -ldl
        )

set(ENABLE_ASSEMBLE ON)
add_definitions(-DENABLE_ASSEMBLE)
add_subdirectory(src/crypto-bn-cpp/src)
add_subdirectory(src/crypto-hash-cpp/src)
add_subdirectory(src/crypto-encode-cpp/src)
add_subdirectory(src/crypto-curve-cpp/src)
add_subdirectory(src/crypto-commitment-cpp/src)
add_subdirectory(src/crypto-paillier-cpp/src)
add_subdirectory(src/crypto-sss-cpp/src)
add_subdirectory(src/crypto-zkp-cpp/src)
add_subdirectory(src/crypto-bip32-cpp/src)
add_subdirectory(src/crypto-ecies-cpp/src)
add_subdirectory(src/crypto-bip39-cpp/src)

option(ENABLE_TESTS "Enable tests" OFF)
if (${ENABLE_TESTS})
    enable_testing()
    include_directories(
            src/crypto-bn-cpp/src
            src/crypto-hash-cpp/src
            src/crypto-encode-cpp/src
            src/crypto-curve-cpp/src
            src/crypto-commitment-cpp/src
            src/crypto-paillier-cpp/src
            src/crypto-sss-cpp/src
            src/crypto-zkp-cpp/src
            src/crypto-bip32-cpp/src
            src/crypto-ecies-cpp/src
            src/crypto-bip39-cpp/src
            /usr/local/include
    )
    link_directories(/usr/local/lib)

    add_subdirectory(src/crypto-bn-cpp/test)
    add_subdirectory(src/crypto-hash-cpp/test)
    add_subdirectory(src/crypto-encode-cpp/test)
    add_subdirectory(src/crypto-curve-cpp/test)
    add_subdirectory(src/crypto-commitment-cpp/test)
    add_subdirectory(src/crypto-paillier-cpp/test)
    add_subdirectory(src/crypto-sss-cpp/test)
    add_subdirectory(src/crypto-zkp-cpp/test)
    add_subdirectory(src/crypto-bip32-cpp/test)
    add_subdirectory(src/crypto-ecies-cpp/test)
    add_subdirectory(src/crypto-bip39-cpp/test)
endif()

include("cmake/setup_installation.cmake")