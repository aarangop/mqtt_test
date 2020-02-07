cmake_minimum_required(VERSION 3.15)

project(test LANGUAGES CXX)

find_library(
    PAHO_MQTT_CPP 
    NAMES paho-mqttpp3 paho-mqttpp3-static 
    HINTS ${CMAKE_SOURCE_DIR}/lib/paho.mqtt.cpp/lib
)

find_library(
    PAHO_MQTT_C
    NAMES paho-mqtt3a
    HINTS ${CMAKE_SOURCE_DIR}/lib/paho.mqtt.c/lib
)

include_directories(${CMAKE_SOURCE_DIR}/lib/paho.mqtt.cpp/include)
include_directories(${CMAKE_SOURCE_DIR}/lib/paho.mqtt.c/include)

add_executable(test main.cpp)

target_link_libraries(test ${PAHO_MQTT_CPP})
target_link_libraries(test ${PAHO_MQTT_C})
target_compile_options(test PRIVATE -W4)