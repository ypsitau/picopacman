cmake_minimum_required(VERSION 3.12)

# Pull in SDK (must be before project)
include(pico_sdk_import.cmake)

project(lcd-pacman C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialize the SDK
pico_sdk_init()

add_executable(lcd-pacman
	spi-lcdpacman.c
	ili9341_spi.c
	graphlib.c
	pacman2data.c
	graphlib.h
	LCDdriver.h
	pacman2.h
)

# Pull in basic dependencies
target_link_libraries(lcd-pacman pico_stdlib hardware_spi hardware_pwm)

# create map/bin/hex file etc.
pico_add_extra_outputs(lcd-pacman)

target_link_libraries(lcd-pacman jxglib_USBHost)
target_sources(lcd-pacman PRIVATE jxglib_adapter.cpp)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/../pico-jxglib pico-jxglib)
jxglib_configure_USBHost(lcd-pacman CFG_TUH_HID 3)
