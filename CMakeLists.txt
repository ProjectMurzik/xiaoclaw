cmake# The following lines of boilerplate have to be in your project's
# CMakeLists in this exact order for cmake to work correctly
cmake_minimum_required(VERSION 3.16)

# Глобальное подавление предупреждения (опционально, но полезно)
add_compile_options(-Wno-missing-field-initializers)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)

idf_build_set_property(MINIMAL_BUILD ON)
set(PROJECT_VER "2.2.4")

# 1. Сначала запускаем сборку проекта (скачиваются компоненты и создаются цели компиляции)
project(xiaozhi)

# 2. 🔥 Теперь точечно отключаем -Werror для скачанного порта LVGL
if(TARGET __idf_espressif__esp_lvgl_port)
    target_compile_options(__idf_espressif__esp_lvgl_port PRIVATE -Wno-error=maybe-uninitialized)
    message(STATUS "🚨 [XIAOCLAW HACK] Успешно отключен -Werror для esp_lvgl_port")
endif()
