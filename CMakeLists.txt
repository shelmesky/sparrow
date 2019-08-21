cmake_minimum_required(VERSION 3.9)
project(sparrow)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_VERBOSE_MAKEFILE OFF)

add_compile_options(-Wall)
add_compile_options(-fgnu89-inline)
add_compile_options(-Wstrict-prototypes)
add_compile_options(-Wmissing-prototypes)
add_compile_options(-Wsystem-headers)

aux_source_directory(. DIRSRCS)

include_directories(cli compiler gc include object parser vm)

add_subdirectory(object)
add_subdirectory(cli)
add_subdirectory(include)
add_subdirectory(compiler)
add_subdirectory(gc)

add_subdirectory(parser)
add_subdirectory(vm)

add_executable(spr cli/cli.c)
target_link_libraries(spr
        -Wl,--start-group
        object
        vm
        include
        cli
        compiler
        parser
        gc
        -Wl,--end-group
        )
target_link_libraries(spr m)

#[[

cmake_minimum_required(VERSION 3.9)
project(sparrow)

set(CMAKE_VERBOSE_MAKEFILE OFF)

include_directories(. object vm compiler parser include cli gc)
aux_source_directory(. SRC_LIST)

file(GLOB_RECURSE SOURCES_COMPILER RELATIVE ${CMAKE_SOURCE_DIR} "compiler/*.c")
file(GLOB_RECURSE SOURCES_GC RELATIVE ${CMAKE_SOURCE_DIR} "gc/*.c")
file(GLOB_RECURSE SOURCES_INCLUDE RELATIVE ${CMAKE_SOURCE_DIR} "include/*.c")
file(GLOB_RECURSE SOURCES_OBJECT RELATIVE ${CMAKE_SOURCE_DIR} "object/*.c")
file(GLOB_RECURSE SOURCES_PARSER RELATIVE ${CMAKE_SOURCE_DIR} "parser/*.c")
file(GLOB_RECURSE SOURCES_VM RELATIVE ${CMAKE_SOURCE_DIR} "vm/*.c")

add_executable(spr ${SRC_LIST} ${SOURCES_OBJECT} ${SOURCES_INCLUDE}  ${SOURCES_COMPILER} ${SOURCES_PARSER}  ${SOURCES_VM} ${SOURCES_GC})
target_link_libraries(spr m)
target_compile_options(spr PRIVATE -W -Wstrict-prototypes -Wmissing-prototypes -Wsystem-headers -fgnu89-inline)]]
