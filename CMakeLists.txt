cmake_minimum_required(VERSION 3.22)

set(MCU "STM32H723VGT6")
set(MCU_ARCH_FLAGS "-mcpu=cortex-m7 --specs=nano.specs -mfpu=fpv5-d16 -mfloat-abi=hard -mthumb")

project(CubeTemplate LANGUAGES CXX C ASM)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "" FORCE)
endif()

set(WARNINGS_FLAGS
    -pedantic-errors
    -Wall
    -Wextra
    -Wconversion
    -Wsign-conversion
    -Werror
    -Wshadow
)

set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)
set(CMAKE_COLOR_DIAGNOSTICS ON)

file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS src/*.cpp)

add_executable(${PROJECT_NAME} ${SOURCES})

target_include_directories(${PROJECT_NAME} PRIVATE
    src
)

target_compile_options(${PROJECT_NAME} PRIVATE ${WARNINGS_FLAGS})

add_subdirectory(CubeIDE)

target_link_libraries(${PROJECT_NAME}
    CubeIDE
)

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_OBJCOPY} -O ihex $<TARGET_FILE:${PROJECT_NAME}> ${PROJECT_NAME}.hex     # .hex
    COMMAND ${CMAKE_OBJCOPY} -O binary $<TARGET_FILE:${PROJECT_NAME}> ${PROJECT_NAME}.bin   # .bin
    COMMAND ln -s -f ${CMAKE_BUILD_TYPE}/compile_commands.json ../compile_commands.json     # compile_commands.json
)
