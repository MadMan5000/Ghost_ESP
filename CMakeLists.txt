cmake_minimum_required(VERSION 3.16.0)

# Set IDF_TARGET from environment variable if defined
if(DEFINED ENV{IDF_TARGET})
    # Workaround GCC 14 internal compiler error in esp_lcd RGB sources on Windows
    # Prevent IRA hoisting that triggers an ICE in xtensa-esp32s3-elf-gcc 14.2
    add_compile_options(-fno-ira-hoist-pressure)
    set(IDF_TARGET $ENV{IDF_TARGET})
    message(STATUS "Setting IDF_TARGET to ${IDF_TARGET} from environment variable")
endif()

# Partition table settings
add_link_options("-Wl,-z,muldefs")

set(SDKCONFIG_DEFAULTS "sdkconfig.defaults")
add_compile_definitions(HOLD_LIMIT=1000) # For joystick hold duration
add_compile_definitions(LED_ORDER=0)     # 2 for RGB, 0 for GRB
add_compile_definitions(DNS_SERVER_MAX_ITEMS=1)
add_compile_definitions(MAX_WPS_NETWORKS=15)

# define esp32s2 target macro globally for legacy guards
if("${IDF_TARGET}" STREQUAL "esp32s2")
    add_compile_definitions(CONFIG_IDF_TARGET_ESP32S2)
endif()

# Include project settings
include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(Ghost_ESP_IDF)
