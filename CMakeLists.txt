cmake_minimum_required(VERSION 4.2)
project(photo)
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(WIN32)
    set(PHOTO_RC_FILE ${CMAKE_SOURCE_DIR}/hp.rc)
endif()

add_executable(photo
    src/lexer.cpp
    src/generator.cpp
    src/main.cpp
    src/translator.cpp
	 ${PHOTO_RC_FILE}
)

if(MSVC)
    target_compile_options(photo PRIVATE /utf-8)
endif() 

target_precompile_headers(photo PRIVATE
    src/pch.hpp
)
target_include_directories(photo PRIVATE
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_SOURCE_DIR}/THIRD_PARTY/include
)

# Copy cpp_dic/ next to the built executable so photo can find
# cpp_dic/dic.json regardless of the working directory it's run from.
add_custom_command(TARGET photo POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${CMAKE_SOURCE_DIR}/cpp_dic
    $<TARGET_FILE_DIR:photo>/cpp_dic
)
