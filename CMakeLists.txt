cmake_minimum_required(VERSION 3.24)
project(CUnit C)

set(CMAKE_C_STANDARD 11)

# Add include dirs for libs
include_directories(.)
include_directories(./cunit/Sources)
include_directories(./cunit/Headers)

# Add library
add_library(cunittestfmk STATIC
			cunit/Sources/Framework/CUError.c
			cunit/Sources/Framework/MyMem.c
  			cunit/Sources/Framework/TestDB.c
  			cunit/Sources/Framework/TestRun.c
  			cunit/Sources/Framework/Util.c 
			)
set_target_properties(cunittestfmk
			PROPERTIES
			COMPILE_FLAGS -fPIC)
add_library(cunitbasic SHARED
			cunit/Sources/Basic/Basic.c
			)
target_link_libraries(cunitbasic
			cunittestfmk
			)
#add_library(cunitcurses SHARED
#			cunit/Sources/Curses/Curses.c
#			)
#add_library(cunitconsole SHARED
#			cunit/Sources/Console/Console.c
#			)
install(TARGETS cunitbasic
		DESTINATION ${TOP_BINARY_DIR})
install(TARGETS cunittestfmk
		DESTINATION ${TOP_BINARY_DIR})