cmake_minimum_required(VERSION 3.16)

project(gitty VERSION 1.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# add_subdirectory(vendor/FTXUI)
add_subdirectory(vendor/libgit2)

set(EXECUTABLE_FILE "gitty")
add_executable(${EXECUTABLE_FILE}
    src/gitty.cpp
    src/events/event.cpp
)

target_precompile_headers(${EXECUTABLE_FILE}
    PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/gtpch.h
)

target_link_libraries(${EXECUTABLE_FILE}
    # PRIVATE ftxui::screen
    # PRIVATE ftxui::dom
    # PRIVATE ftxui::component
    PRIVATE git2
)

target_include_directories(${EXECUTABLE_FILE}
    PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/src
    PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src
    PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/vendor/color-console/include
    PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/vendor/color-console/include
    # PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/vendor/FTXUI/include
    # PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/vendor/FTXUI/include
    PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/vendor/libgit2/include
    PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/vendor/libgit2/include
)

if (MSVC)
else()
  target_compile_options(git-tui
    PRIVATE "-Wall"
    PRIVATE "-Wextra"
    PRIVATE "-pedantic"
    PRIVATE "-Werror"
    PRIVATE "-Wdeprecated"
    )
endif()

install(TARGETS ${EXECUTABLE_FILE} RUNTIME DESTINATION "bin")