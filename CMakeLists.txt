cmake_minimum_required(VERSION 3.16)
project(RayTracer C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

if(MSVC)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /O2 /fp:fast")
    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} /O2 /fp:fast")
    add_compile_options(/openmp)
else()
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 -ffast-math -fopenmp -march=native")
    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -O3 -ffast-math")
endif()

set(SDL2_VERSION "2.30.0")
set(SDL2_DIR "${CMAKE_SOURCE_DIR}/external/SDL2-${SDL2_VERSION}")

if(NOT EXISTS "${SDL2_DIR}")
    message(STATUS "Downloading SDL2...")
    file(DOWNLOAD 
        "https://github.com/libsdl-org/SDL/releases/download/release-${SDL2_VERSION}/SDL2-devel-${SDL2_VERSION}-VC.zip"
        "${CMAKE_SOURCE_DIR}/external/sdl2.zip"
        SHOW_PROGRESS
        STATUS DOWNLOAD_STATUS
    )
    list(GET DOWNLOAD_STATUS 0 STATUS_CODE)
    if(NOT STATUS_CODE EQUAL 0)
        message(FATAL_ERROR "Failed to download SDL2. Please download manually from https://github.com/libsdl-org/SDL/releases")
    endif()
    
    file(ARCHIVE_EXTRACT INPUT "${CMAKE_SOURCE_DIR}/external/sdl2.zip" DESTINATION "${CMAKE_SOURCE_DIR}/external")
    file(REMOVE "${CMAKE_SOURCE_DIR}/external/sdl2.zip")
endif()

set(SDL2_INCLUDE_DIR "${SDL2_DIR}/include")
set(SDL2_LIBRARY "${SDL2_DIR}/lib/x64/SDL2.lib")
set(SDL2_MAIN_LIBRARY "${SDL2_DIR}/lib/x64/SDL2main.lib")
set(SDL2_DLL "${SDL2_DIR}/lib/x64/SDL2.dll")

set(SOURCES
    src/main.c
    src/math/vector.c
    src/core/ray.c
    src/geometry/object.c
    src/geometry/sphere.c
    src/geometry/box.c
    src/geometry/bvh.c
    src/material/material.c
    src/light/light.c
    src/scene/camera.c
    src/scene/scene.c
    src/render/framebuffer.c
    src/render/renderer.c
    src/ui/window.c
    src/ui/input.c
    src/ui/ui.c
)

add_executable(raytracer ${SOURCES})

target_include_directories(raytracer PRIVATE 
    ${CMAKE_SOURCE_DIR}/src
    ${SDL2_INCLUDE_DIR}
)

target_link_libraries(raytracer PRIVATE ${SDL2_LIBRARY} ${SDL2_MAIN_LIBRARY})

if(WIN32)
    add_custom_command(TARGET raytracer POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${SDL2_DLL}"
        $<TARGET_FILE_DIR:raytracer>
    )
endif()

add_custom_command(TARGET raytracer POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${CMAKE_SOURCE_DIR}/scenes $<TARGET_FILE_DIR:raytracer>/scenes
)
