cmake_minimum_required(VERSION 3.5)

set(CMAKE_CXX_STANDARD 23)
set(EXPORT_COMPILE_COMMANDS ON)

project(PlantsVsZombies)

set(TARGET run)

file(GLOB_RECURSE SOURCES "source/*.cpp")
file(GLOB_RECURSE HEADERS "source/*.hpp")

find_program(CLANG_TIDY_EXE NAMES "clang-tidy")
if(CLANG_TIDY_EXE)
    set(CMAKE_CXX_CLANG_TIDY
            "${CLANG_TIDY_EXE}"
            "--config-file=${CMAKE_SOURCE_DIR}/.clang-tidy"  # Использует настройки из файла
    )
endif()

add_executable(${TARGET} ${SOURCES} ${HEADERS})

include_directories(source)

find_package(SFML 3 REQUIRED COMPONENTS Graphics Audio Network)

target_link_libraries(${TARGET} PRIVATE SFML::Graphics SFML::Audio SFML::Network)

add_compile_options(-fsanitize=address,alignment,undefined)
add_link_options(-fsanitize=address,alignment,undefined)