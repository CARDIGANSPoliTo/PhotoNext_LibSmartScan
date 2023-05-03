cmake_minimum_required(VERSION 2.8.9)
project (smartscan)

set(smartscan_VERSION_MAJOR 0)
set(smartscan_VERSION_MINOR 1)

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_CXX_FLAGS "-Wall -std=c++11")

find_package (Threads REQUIRED)
find_library(LIBUTILS utils)    #custom utils library

if(NOT LIBUTILS)
  message(FATAL_ERROR "utils library not found")
endif()

include_directories(include)
set (HEADERS  include/data_queue.h
              include/msg_queue.h
              include/smartscan_constants.h
              include/smartscan_utils.h
              include/socket.h
)

set (SOURCES  src/smartscan_utils.c
              src/msg_queue.c
              src/data_queue.c
)

set (CONFIG   smartscan_config.txt)

# add_library(msg_queue STATIC src/msg_queue.c)
# add_library(data_queue STATIC src/data_queue.c)

add_library(smartscan SHARED ${SOURCES})
target_link_libraries(smartscan -lutils)
# target_link_libraries(smartscan msg_queue)
# target_link_libraries(smartscan data_queue)

install(TARGETS smartscan DESTINATION lib)
install(FILES ${HEADERS} DESTINATION include/libsmartscan)
install(FILES ${CONFIG} DESTINATION etc/libsmartscan)
