cmake_minimum_required(VERSION 3.14)

project(openaicpp)

option(OPENAI_BUILD_EXAMPLES "Build examples" ON)

set(HEADERS include/openai/openai.hpp)
add_library(${PROJECT_NAME} INTERFACE ${HEADERS})

target_include_directories(${PROJECT_NAME} INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/include/openai
    ${CMAKE_CURRENT_SOURCE_DIR}/nlohmann/single_include)

if(OPENAI_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()
