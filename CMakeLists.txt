# The following lines of boilerplate have to be in your project's
# CMakeLists in this exact order for cmake to work correctly
cmake_minimum_required(VERSION 3.16)

# Глобальное подавление предупреждения о недостающих инициализаторах
add_compile_options(-Wno-missing-field-initializers)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)

# Минимальная сборка (опционально, но полезно для CI)
idf_build_set_property(MINIMAL_BUILD ON)

set(PROJECT_VER "2.2.4")
project(xiaozhi)

# 🔥 Точечное отключение -Werror для esp_lvgl_port (правильный CMake-способ)
if(TARGET __idf_espressif__esp_lvgl_port)
    get_target_property(_target_type __idf_espressif__esp_lvgl_port TYPE)
    if(_target_type STREQUAL "INTERFACE_LIBRARY")
        target_compile_options(__idf_espressif__esp_lvgl_port INTERFACE -Wno-error=maybe-uninitialized)
    else()
        target_compile_options(__idf_espressif__esp_lvgl_port PRIVATE -Wno-error=maybe-uninitialized)
    endif()
    message(STATUS "✅ Disabled -Werror for esp_lvgl_port")
endif()
