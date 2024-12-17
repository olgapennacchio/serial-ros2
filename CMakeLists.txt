cmake_minimum_required(VERSION 3.8)
project(serial)

find_package(ament_cmake REQUIRED)

set(rt_LIBRARIES rt)
set(pthread_LIBRARIES pthread)

ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

## Sources
set(serial_SRCS
    src/serial.cc
    include/serial/serial.h
    include/serial/v8stdint.h
)

list(APPEND serial_SRCS src/impl/unix.cc)
list(APPEND serial_SRCS src/impl/list_ports/list_ports_linux.cc)


## Add serial library
add_library(${PROJECT_NAME} ${serial_SRCS})

target_link_libraries(${PROJECT_NAME} rt pthread)

include_directories(include)

## Install executable
install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

## Install headers
install(FILES include/serial/serial.h include/serial/v8stdint.h
    DESTINATION include/serial
)

ament_package()
