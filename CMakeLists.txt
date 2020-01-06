
cmake_minimum_required(VERSION 3.10)

set(CMAKE_C_COMPILER arm-apple-darwin11-clang)
project(tbd C)

set(VERSION_MAJOR 2)
set(VERSION_MINOR 0)
set(VERSION ${VERSION_MAJOR}.${VERSION_MINOR})

set(CMAKE_C_FLAGS "-std=gnu11 -Wshadow -Wwrite-strings")
set(CMAKE_C_FLAGS_DEBUG "-g3 -Og -pg -fsanitize=address -fno-omit-frame-pointer")
set(CMAKE_C_FLAGS_RELEASE "-Ofast -DNDEBUG -funroll-loops")


if (CMAKE_C_COMPILER_VERSION VERSION_LESS 4.0)
   message(FATAL_ERROR "Clang>=4.0 required. In case you are on Linux upgrade.")
endif ()
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wunused-parameter -Wno-attributes -Wno-nonnull")

file(GLOB SRC src/*.c)

include_directories(include/)
add_executable(tbd ${SRC})
add_custom_command(TARGET tbd COMMAND ldid -S${PROJECT_SOURCE_DIR}/ent.plist tbd)

install(TARGETS tbd RUNTIME DESTINATION bin)


include(cmake/package.cmake)
