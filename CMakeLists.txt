cmake_minimum_required(VERSION 3.14)

# set the project name
project(multitail VERSION 5.2.8)

set(CMAKE_VERBOSE_MAKEFILE on)

file(STRINGS "version" ver_raw)
string(STRIP ${ver_raw} ver)
string(REGEX REPLACE "VERSION=" "" version_number "${ver}")

add_compile_definitions(VERSION="${version_number}")
add_compile_definitions(CONFIG_FILE="/etc/multitail.conf")

find_package(Curses REQUIRED)
include_directories(${CURSES_INCLUDE_DIR})

file(GLOB SOURCES "./*.c")

add_executable(multitail ${SOURCES})
target_link_libraries(multitail ${CURSES_LIBRARY})
target_link_libraries(multitail panel)
target_link_libraries(multitail util)
target_link_libraries(multitail m)

install(TARGETS multitail
        DESTINATION bin)
install(FILES multitail.1
        DESTINATION share/man/man1/multitail.1)
install(FILES multitail.conf
        DESTINATION /etc/multitail/)
