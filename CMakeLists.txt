cmake_minimum_required(VERSION 3.30)

project(Ats-Session-Server LANGUAGES CXX)

set(CMAKE_CXX_MODULE_STD 1)

set(CMAKE_CXX_STANDARD 23)

set(BOOST_ROOT "/opt/homebrew/opt/boost")
find_package(Boost 1.87 REQUIRED COMPONENTS system)

add_executable(Ats-Session-Server src/main.cpp
)

target_sources(Ats-Session-Server
        PUBLIC
        FILE_SET modules TYPE CXX_MODULES FILES
        src/Transport/Server.cppm
        src/StdAll.cppm
)

if (Boost_FOUND)
    target_include_directories(Ats-Session-Server PRIVATE ${Boost_INCLUDE_DIRS})
    target_link_libraries(Ats-Session-Server PRIVATE ${Boost_LIBRARIES})
else ()
    message(FATAL_ERROR "Boost not found!")
endif ()

add_library(unused_std_target STATIC)

target_sources(unused_std_target
        PRIVATE
        FILE_SET CXX_MODULES
        BASE_DIRS /opt/homebrew/Cellar/llvm/19.1.7/share/libc++/v1
        FILES /opt/homebrew/Cellar/llvm/19.1.7/share/libc++/v1/std.cppm /opt/homebrew/Cellar/llvm/19.1.7/share/libc++/v1/std.compat.cppm)