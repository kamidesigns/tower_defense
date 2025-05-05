cmake_minimum_required(VERSION 3.25)
project(tower_defense LANGUAGES CXX C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(ASSETS_SOURCE_DIR "${CMAKE_SOURCE_DIR}/etc/assets")
set(ASSETS_BINARY_DIR "${CMAKE_CURRENT_BINARY_DIR}/etc/assets")

include_directories("include" "include/tower_defense")

file(GLOB_RECURSE SRC deps/*.c deps/*.h src/*.cpp src/*.h)
add_executable(${PROJECT_NAME} ${SRC})

add_custom_target(copy_assets ALL
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${ASSETS_SOURCE_DIR}
    ${ASSETS_BINARY_DIR}
    COMMENT "Copying assets into binary directory")

add_dependencies(${PROJECT_NAME} copy_assets)

find_package(X11 REQUIRED)
find_package(OpenGL REQUIRED)

set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,--copy-dt-needed-entries")


target_link_libraries(${PROJECT_NAME}
    PRIVATE
        ${X11_Xinerama_LIB}
        ${X11_Xrandr_LIB}
        ${X11_Xcursor_LIB}
        ${X11_Xi_LIB}
        ${OPENGL_LIBRARIES}
)
