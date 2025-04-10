cmake_minimum_required(VERSION 3.5)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

project(PlantsVsZombies)

set(TARGET run)

file(GLOB_RECURSE SOURCES "source/*.cpp")
file(GLOB_RECURSE HEADERS "source/*.hpp")

find_program(CLANG_TIDY_EXE NAMES "clang-tidy")
if(CLANG_TIDY_EXE)
    set(CMAKE_CXX_CLANG_TIDY
            "${CLANG_TIDY_EXE}"
            "--config-file=${CMAKE_SOURCE_DIR}/.clang-tidy"
            "--extra-arg=-std=c++23"
    )
endif()

add_executable(${TARGET} ${SOURCES} ${HEADERS})

find_package(SFML 3 REQUIRED COMPONENTS Graphics Audio Network)

set_target_properties(SFML::Graphics SFML::Audio SFML::Network PROPERTIES
        CXX_CLANG_TIDY ""
)

target_include_directories(${TARGET}
        PRIVATE
        source
        SYSTEM ${SFML_INCLUDE_DIR}
)

target_link_libraries(${TARGET} PRIVATE
        SFML::Graphics
        SFML::Audio
        SFML::Network
)

add_compile_options(-fsanitize=address,alignment,undefined)
add_link_options(-fsanitize=address,alignment,undefined)