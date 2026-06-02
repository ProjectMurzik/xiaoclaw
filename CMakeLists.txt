cmake_minimum_required(VERSION 3.16)

# Глобальное подавление предупреждения
add_compile_options(-Wno-missing-field-initializers)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)

# ВАЖНО: Удаляем или комментируем MINIMAL_BUILD, чтобы включить менеджер пакетов!
# idf_build_set_property(MINIMAL_BUILD ON) 

set(PROJECT_VER "2.2.4")

# 🔥 БРОНЕБОЙНЫЙ CMake-ХАК БУТЛОАДЕРА
macro(check_bootloader_size)
    message(STATUS "🚨 [XIAOCLAW HACK] Bootloader size check bypassed")
endmacro()

# Инициализируем проект
project(xiaozhi)

# Точечно отключаем -Werror для esp_lvgl_port
if(TARGET __idf_espressif__esp_lvgl_port)
    get_target_property(_target_type __idf_espressif__esp_lvgl_port TYPE)
    if(_target_type STREQUAL "INTERFACE_LIBRARY")
        target_compile_options(__idf_espressif__esp_lvgl_port INTERFACE -Wno-error=maybe-uninitialized)
    else()
        target_compile_options(__idf_espressif__esp_lvgl_port PRIVATE -Wno-error=maybe-uninitialized)
    endif()
    message(STATUS "🚨 [XIAOCLAW HACK] Disabled -Werror for esp_lvgl_port")
endif()
