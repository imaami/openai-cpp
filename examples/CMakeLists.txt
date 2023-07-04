cmake_minimum_required(VERSION 3.14)

project(examples)

find_package(CURL REQUIRED)

option(CURL_STATIC_LINKING "Statically link against libcurl." OFF)
option(OPENAI_VERBOSE_OUTPUT "Enable openai-cpp verbose output." OFF)

file(GLOB EXAMPLES_SRC [0-9][0-9]-*.cpp)

foreach(src_file ${EXAMPLES_SRC})
    get_filename_component(name ${src_file} NAME_WE)
    add_executable(${name})
    target_sources(${name} PRIVATE ${src_file})

    if(CURL_STATIC_LINKING)
        target_compile_definitions(${name} PRIVATE CURL_STATICLIB)
    endif()

    if(OPENAI_VERBOSE_OUTPUT)
        target_compile_definitions(${name} PRIVATE OPENAI_VERBOSE_OUTPUT=1)
    else()
        target_compile_definitions(${name} PRIVATE OPENAI_VERBOSE_OUTPUT=0)
    endif()

    target_compile_definitions(${name} PRIVATE JSON_USE_IMPLICIT_CONVERSIONS=0)
    target_compile_features(${name} PRIVATE cxx_std_20)
    target_compile_options(${name} PRIVATE
        $<$<CXX_COMPILER_ID:MSVC>:/W4>
        $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wall -Wextra -pedantic>
        $<$<CXX_COMPILER_ID:Clang>:-Weverything
            -Wno-c++98-compat
            -Wno-c++98-compat-pedantic
            -Wno-covered-switch-default
            -Wno-disabled-macro-expansion
            -Wno-padded
            -Wno-pre-c++17-compat
            -Wno-pre-c++20-compat
            -Wno-unsafe-buffer-usage
            -Wno-weak-vtables>
    )
    target_link_libraries(${name} ${CURL_LIBRARIES} openaicpp)
endforeach()
