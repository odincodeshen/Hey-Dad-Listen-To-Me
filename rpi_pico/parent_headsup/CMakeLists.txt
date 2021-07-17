
cmake_minimum_required(VERSION 3.12)

project(person_detection C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 11)

add_executable(parent_headsup_int8 "")

target_include_directories(parent_headsup_int8
    PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/.
    )

set_target_properties(
    parent_headsup_int8
    PROPERTIES
    COMPILE_FLAGS -fno-rtti
    COMPILE_FLAGS -fno-exceptions
    COMPILE_FLAGS -fno-threadsafe-statics
    COMPILE_FLAGS -nostdlib
)

target_sources(parent_headsup_int8
    PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/detection_responder.cpp
    ${CMAKE_CURRENT_LIST_DIR}/image_provider.cpp
    ${CMAKE_CURRENT_LIST_DIR}/main.cpp
    ${CMAKE_CURRENT_LIST_DIR}/main_functions.cpp
    ${CMAKE_CURRENT_LIST_DIR}/model_settings.cpp
    ${CMAKE_CURRENT_LIST_DIR}/tensorflow/lite/micro/tools/make/downloads/person_model_int8/person_detect_model_data.cpp
    ${CMAKE_CURRENT_LIST_DIR}/detection_responder.h
    ${CMAKE_CURRENT_LIST_DIR}/image_provider.h
    ${CMAKE_CURRENT_LIST_DIR}/main_functions.h
    ${CMAKE_CURRENT_LIST_DIR}/model_settings.h
    ${CMAKE_CURRENT_LIST_DIR}/person_detect_model_data.h
    )

target_link_libraries(
  parent_headsup_int8
  pico-tflmicro
  hardware_pwm
  hardware_pio
  pico_stdlib
  arducam_hm01b0
  LCD_st7735

)

pico_enable_stdio_uart(parent_headsup_int8 0)
pico_enable_stdio_usb(parent_headsup_int8 1)
pico_add_extra_outputs(parent_headsup_int8)


