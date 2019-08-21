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