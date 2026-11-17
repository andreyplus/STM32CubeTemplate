cmake_minimum_required(VERSION 3.24)

project(CubeIDE LANGUAGES C ASM)

file(GLOB_RECURSE CubeIDESources CONFIGURE_DEPENDS *.c *.s)
add_library(CubeIDE OBJECT ${CubeIDESources})

target_compile_definitions(CubeIDE PUBLIC 
    ${MCU_SUBFAMILY}
    USE_HAL_DRIVER
)

target_include_directories(CubeIDE PUBLIC
    Core/Inc
    Drivers/${MCU_FAMILY}_HAL_Driver/Inc
    Drivers/${MCU_FAMILY}_HAL_Driver/Inc/Legacy
    Drivers/CMSIS/Device/ST/${MCU_FAMILY}/Include
    Drivers/CMSIS/Include
)
