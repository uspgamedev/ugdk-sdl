
cmake_minimum_required (VERSION 2.8.11)
# 2.8.11 for target_include_directories

project (ugdk-sdl C)

# Build from source!
if(ANDROID)
    # For android, using SDL's CMakeLists is not necessary (and doesn't work without too much effort)
    include(android-sdl2.cmake)

elseif()
    if(WIN32)
        find_package(OpenGL REQUIRED)
        list(APPEND EXTRA_LIBS ${OPENGL_LIBRARIES})
    endif()
    # Set some options for the SDL's CMakeLists.
    if(${BUILD_SHARED_LIBS})
        set(SDL_SHARED ON  CACHE BOOL "Build a shared version of the library" FORCE)
        set(SDL_STATIC OFF CACHE BOOL "Build a static version of the library" FORCE)
    else()
        set(SDL_SHARED OFF CACHE BOOL "Build a shared version of the library" FORCE)
        set(SDL_STATIC ON  CACHE BOOL "Build a static version of the library" FORCE)
    endif()
    set(DIRECTX    OFF CACHE BOOL "Use DirectX for Windows audio/video" FORCE)
    set(RENDER_D3D OFF CACHE BOOL "Enable the Direct3D render driver" FORCE)
    if (MSVC)
        set(LIBC   ON CACHE BOOL "Use the system C library" FORCE)
    endif(MSVC)
    add_subdirectory(SDL SDL-build)

    target_include_directories(SDL2 PUBLIC "${SDL2_SOURCE_DIR}/include" "${SDL2_BINARY_DIR}/include") # TODO: add 'SYSTEM' flag when we have cmake 2.8.12
endif()
