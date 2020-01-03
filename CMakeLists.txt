cmake_minimum_required(VERSION 3.10)

set(CMAKE_CXX_STANDARD 14)
set(PYBIND11_CPP_STANDARD -std=c++14)

set(Boost_USE_MULTITHREADED      ON)
#set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} -fPIC)
#set(Boost_USE_STATIC_RUNTIME    OFF)
set(Boost_USE_STATIC_RUNTIME OFF)
set(BoostCOMP filesystem program_options)


FIND_PACKAGE( Boost 1.50 COMPONENTS ${BoostCOMP}
        REQUIRED )
#
include_directories(${Boost_INCLUDE_DIRS})
INCLUDE_DIRECTORIES( ${Boost_INCLUDE_DIR} )
link_libraries(${Boost_LIBRARIES})

#file(GLOB_RECURSE ms_srcs ./ *.cpp)
file(GLOB_RECURSE ms_hdrs ./ *.hpp)
include_directories(
        ${CMAKE_SOURCE_DIR}/maestro-dev/lib/include/abstract-hardware-model
        ${CMAKE_SOURCE_DIR}/maestro-dev/lib/include/cost-analysis
        ${CMAKE_SOURCE_DIR}/maestro-dev/lib/include/dataflow-analysis
        ${CMAKE_SOURCE_DIR}/maestro-dev/lib/include/dataflow-specification-language
        ${CMAKE_SOURCE_DIR}/maestro-dev/lib/include/design-space-exploration
        ${CMAKE_SOURCE_DIR}/maestro-dev/lib/include/tools
        ${CMAKE_SOURCE_DIR}/maestro-dev/lib/include/user-api
        ${CMAKE_SOURCE_DIR}/maestro-dev/lib/include
        ${CMAKE_SOURCE_DIR}/maestro-dev/lib/include/base
        )
set(ms_srcs  ./lib/src/maestro-marvel.cpp ./lib/src/BASE_base-objects.cpp)
message(${ms_hdrs})
add_library(maestro SHARED ${ms_srcs} ${ms_hdrs})
set_target_properties(maestro PROPERTIES POSITION_INDEPENDENT_CODE ON)
set_target_properties(maestro PROPERTIES LINKER_LANGUAGE CXX)

target_link_libraries(maestro Boost::program_options Boost::filesystem)