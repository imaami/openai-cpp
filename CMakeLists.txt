cmake_minimum_required(VERSION 3.14)

project(openaicpp)

find_package(CURL REQUIRED)

option(OPENAI_STATIC_LIBCURL "Link against static libcurl" OFF)
option(OPENAI_VERBOSE_OUTPUT "Enable verbose runtime output" OFF)
option(OPENAI_BUILD_EXAMPLES "Build example programs" ON)

set(HEADERS include/openai/openai.hpp)
add_library(${PROJECT_NAME} INTERFACE ${HEADERS})

target_include_directories(${PROJECT_NAME} INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/include/openai
    ${CMAKE_CURRENT_SOURCE_DIR}/nlohmann/single_include)

target_compile_definitions(${PROJECT_NAME} INTERFACE
    $<$<BOOL:${OPENAI_STATIC_LIBCURL}>:CURL_STATICLIB>
    "OPENAI_VERBOSE_OUTPUT=$<BOOL:${OPENAI_VERBOSE_OUTPUT}>"
    JSON_USE_IMPLICIT_CONVERSIONS=0)

target_compile_features(${PROJECT_NAME} INTERFACE cxx_std_20)
target_link_libraries(${PROJECT_NAME} INTERFACE ${CURL_LIBRARIES})

if(OPENAI_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()
