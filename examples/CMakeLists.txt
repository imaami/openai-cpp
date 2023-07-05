cmake_minimum_required(VERSION 3.14)

project(examples)

file(GLOB EXAMPLES_SRC [0-9][0-9]-*.cpp)

foreach(src_file ${EXAMPLES_SRC})
    get_filename_component(name ${src_file} NAME_WE)
    add_executable(${name})
    target_sources(${name} PRIVATE ${src_file})
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
    target_link_libraries(${name} openaicpp)
endforeach()
