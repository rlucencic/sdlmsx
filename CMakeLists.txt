# sdlmsx: the MSX1 and MSX2 with SDL 3.
#
#   cmake -S . -B build -G Ninja && cmake --build build
#   build/sdlmsx --help
#
# The MSX itself is esp32msx's (../esp32msx/components/msx, platform
# independent), the runtime sdlemu's (../sdlemu), the Z80 esp32emu's
# (../esp32emu): all four cloned side by side.
cmake_minimum_required(VERSION 3.20)
project(sdlmsx C)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "" FORCE)
endif()

set(ESP32MSX_DIR "${CMAKE_CURRENT_LIST_DIR}/../esp32msx" CACHE PATH "esp32msx checkout: the MSX")
if(NOT EXISTS "${ESP32MSX_DIR}/components/msx/include/msx.h")
    message(FATAL_ERROR "No esp32msx at ${ESP32MSX_DIR}: clone github.com/rlucencic/esp32msx beside sdlmsx")
endif()

add_subdirectory("${CMAKE_CURRENT_LIST_DIR}/../sdlemu" sdlemu)

file(GLOB MSX_SOURCES "${ESP32MSX_DIR}/components/msx/*.c")
add_executable(sdlmsx
    src/main.c
    src/settings.c
    src/rom_store.c
    ${MSX_SOURCES})
target_include_directories(sdlmsx PRIVATE
    src
    "${ESP32MSX_DIR}/components/msx/include"
    "${ESP32MSX_DIR}/components/msx")
# The Z80 with the MSX's memory: 8 KB banks, the slot register at $FFFF, as esp32msx's
target_compile_definitions(sdlmsx PRIVATE CONFIG_Z80_TIMING_TSTATES=1 CONFIG_Z80_MSX=1 CONFIG_Z80_READ_SPECIAL=1)
sdlemu_add_cores(sdlmsx z80 diskspace)
target_link_libraries(sdlmsx PRIVATE sdlemu)
target_compile_options(sdlmsx PRIVATE -Wall -Wextra -Wno-unused-parameter -Wno-unused-result)

install(TARGETS sdlmsx)
