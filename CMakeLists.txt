cmake_minimum_required(VERSION 3.14)

project(openaicpp)

set(HEADERS include/openai/openai.hpp)
add_library(${PROJECT_NAME} INTERFACE ${HEADERS})

target_include_directories(${PROJECT_NAME} INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/include/openai
    ${CMAKE_CURRENT_SOURCE_DIR}/nlohmann/single_include)

add_subdirectory(examples)
