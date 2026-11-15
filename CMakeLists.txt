# rpi5msx: the MSX1 and MSX2 on the Raspberry Pi 5.
#
#   cmake -S . -B build -G Ninja && cmake --build build
#   build/rpi5msx --help
#
# The MSX itself is esp32msx's (../esp32msx/components/msx, platform
# independent), the runtime rpi5emu's (../rpi5emu), the Z80 esp32emu's
# (../esp32emu): all four cloned side by side.
cmake_minimum_required(VERSION 3.20)
project(rpi5msx C)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "" FORCE)
endif()

set(ESP32MSX_DIR "${CMAKE_CURRENT_LIST_DIR}/../esp32msx" CACHE PATH "esp32msx checkout: the MSX")
if(NOT EXISTS "${ESP32MSX_DIR}/components/msx/include/msx.h")
    message(FATAL_ERROR "No esp32msx at ${ESP32MSX_DIR}: clone github.com/rlucencic/esp32msx beside rpi5msx")
endif()

add_subdirectory("${CMAKE_CURRENT_LIST_DIR}/../rpi5emu" rpi5emu)

file(GLOB MSX_SOURCES "${ESP32MSX_DIR}/components/msx/*.c")
add_executable(rpi5msx
    src/main.c
    src/settings.c
    src/rom_store.c
    ${MSX_SOURCES})
target_include_directories(rpi5msx PRIVATE
    src
    "${ESP32MSX_DIR}/components/msx/include"
    "${ESP32MSX_DIR}/components/msx")
# The Z80 with the MSX's memory: 8 KB banks, the slot register at $FFFF, as esp32msx's
target_compile_definitions(rpi5msx PRIVATE CONFIG_Z80_TIMING_TSTATES=1 CONFIG_Z80_MSX=1 CONFIG_Z80_READ_SPECIAL=1)
rpi5emu_add_cores(rpi5msx z80 diskspace)
target_link_libraries(rpi5msx PRIVATE rpi5emu)
target_compile_options(rpi5msx PRIVATE -Wall -Wextra -Wno-unused-parameter -Wno-unused-result)

install(TARGETS rpi5msx)
