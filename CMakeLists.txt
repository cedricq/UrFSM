cmake_minimum_required(VERSION 3.14)
project(my_fsm_projects)

include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/refs/tags/v1.16.0.zip
)
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_COMPILER "g++" )
set(CMAKE_C_COMPILER "gcc" )
set(CMAKE_CXX_FLAGS "-isystem -mconsole -std=c++20 -fpermissive -pthread")

option(ENABLE_COVERAGE "Enable coverage reporting" OFF)

if(ENABLE_COVERAGE)
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
        message(STATUS "Coverage enabled with MinGW/GCC")
        add_compile_options(--coverage -O0 -g)
        add_link_options(--coverage)
    else()
        message(WARNING "Coverage only supported with MinGW/GCC on Windows")
    endif()
endif()

include(CTest)

include_directories("inc")
add_executable (fsm_test src/NameString.cpp src/StateMachine.cpp test/StateMachine_t.cpp)
target_link_libraries(fsm_test gtest_main)
if(ENABLE_COVERAGE)
    target_link_libraries(fsm_test gcov)
endif()
add_test(fsm_test fsm_test)
