cmake_minimum_required(VERSION 3.10)
project(deepstream-test1-segmentation)

execute_process( COMMAND gcc -dumpmachine | COMMAND cut -f1 -d -  OUTPUT_VARIABLE LINUX_ARCH
        OUTPUT_STRIP_TRAILING_WHITESPACE)

set(DS_VERSION 5.0)

message("-- DeepStream Version: ${DS_VERSION}")
message("-- COMPILE_TO_ARCH: ${LINUX_ARCH}")

if("${LINUX_ARCH}" STREQUAL "aarch64")
    message("-- aarch64 Jetson TEGRA")
    # set(CMAKE_C_STANDARD 11) 
    # set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DPLATFORM_TEGRA")
    # If you want to use C++ on Jetson, rename the source file from .c to .cpp
    set(CMAKE_CXX_STANDARD 11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DPLATFORM_TEGRA")

elseif("${LINUX_ARCH}" STREQUAL "x86_64")
    message("-- X86 dGPU")
    set(CMAKE_CXX_STANDARD 17)
    # set(CMAKE_C_STANDARD 11)

endif()

set(SRC_PATH /opt/nvidia/deepstream/deepstream-${DS_VERSION})
set(SRC_INC ${SRC_PATH}/sources/apps/apps-common/includes)
set(SRC_INC2 ${SRC_PATH}/sources/includes)


include_directories(
        ${PROJECT_SOURCE_DIR}
        ${SRC_FOLDER}
        ${SRC_INC}
        ${SRC_INC2}
        ${SRC_SRC}
        /usr/include
        /usr/include/gstreamer-1.0
        /usr/include/glib-2.0
        /usr/lib/${LINUX_ARCH}-linux-gnu/glib-2.0/include
)

link_directories(
        ${SRC_PATH}/lib
        /usr/lib/${LINUX_ARCH}-linux-gnu
        /usr/lib/${LINUX_ARCH}-linux-gnu/gstreamer-1.0
)

file(GLOB SRCS1 ${SRC_FOLDER}/*.c ${SRC_FOLDER}/*.h ${SRC_INC2}/*.h ${SRC_SRC}/*.c)
file(GLOB SRCS2 ${PROJECT_SOURCE_DIR}/*.c ${PROJECT_SOURCE_DIR}/*.h)

add_executable(deepstream-test1-segmentation ${SRCS1} ${SRCS2})

target_link_libraries(deepstream-test1-segmentation -lnvdsgst_meta -lrt -lnvds_meta -lnvdsgst_helper -lnvds_utils -lgstrtp-1.0 -lm
        -lgstreamer-1.0 -lgstrtspserver-1.0 -lglib-2.0 -lgobject-2.0 -lgstvideo-1.0)
