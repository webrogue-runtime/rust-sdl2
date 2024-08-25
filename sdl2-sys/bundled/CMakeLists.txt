option(RUST_SDL_BUNDLED_SDL2 "Build SDL2" OFF)
option(RUST_SDL_BUNDLED_SDL2_TTF "Build SDL2_ttf" OFF)
option(RUST_SDL_BUNDLED_STATIC "Build static libraries instead of dynamic" OFF)



set(SDL_SHARED NOT ${RUST_SDL_BUNDLED_STATIC})
set(SDL_STATIC ${RUST_SDL_BUNDLED_STATIC})
set(BUILD_SHARED_LIBS ${RUST_SDL_BUNDLED_STATIC})
if(RUST_SDL_BUNDLED_STATIC)
    set(SDL_SHARED OFF)
    set(SDL_STATIC ON)
    set(BUILD_SHARED_LIBS OFF)
    # Prevent SDL to provide it own "main" which cause a conflict when this crate linked
    # to C/C++ program.
    set(SDL_MAIN_HANDLED ON)
else()
    set(SDL_SHARED ON)
    set(SDL_STATIC OFF)
    set(BUILD_SHARED_LIBS ON)
endif()

if(RUST_SDL_BUNDLED_SDL2)

    exec_program(git ${CMAKE_CURRENT_SOURCE_DIR}/../SDL ARGS submodule update --init)

    set(SDL2_DISABLE_INSTALL OFF CACHE BOOL "")
    set(SDL2_DISABLE_INSTALL OFF)
    
    set(SDL_OPENGL OFF CACHE BOOL "")
    set(SDL_OPENGL OFF)

    set(SDL_OPENGLES ON CACHE BOOL "")
    set(SDL_OPENGLES ON)
    add_subdirectory(../SDL sdl2)
endif()

if(RUST_SDL_BUNDLED_SDL2_TTF)
    exec_program(git ${CMAKE_CURRENT_SOURCE_DIR}/../SDL_ttf ARGS submodule update --init)

    set(SDL2TTF_VENDORED ON)
    set(SDL2TTF_INSTALL ON)
    add_subdirectory(../SDL_ttf sdl2_ttf)
endif()
