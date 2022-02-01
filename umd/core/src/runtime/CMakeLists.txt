cmake_minimum_required(VERSION 3.5)

project(nvdlaruntime)

set(CMAKE_VERBOSE_MAKEFILE ON)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
	set(CMAKE_CXX_STANDARD 11)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
	add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(sources
	Emulator.cpp
	Runtime.cpp
	../common/Check.cpp
	../common/ErrorLogging.c
	../common/EMUInterface.cpp
	../common/EMUInterfaceA.cpp
	../common/Loadable.cpp
	../../../port/linux/nvdla.c
	../../../port/linux/nvdla_os.c
)

include_directories(
	/usr/include/drm
	include
	../common/include
	../../include
	../../../port/linux/include
	../../../external/include
)

add_compile_options(-W -Wno-multichar -Wno-unused-parameter)
add_compile_options(-Wno-unused-function)
add_compile_options(-fPIC -finline)
add_library(${PROJECT_NAME} SHARED ${sources})

target_compile_definitions(${PROJECT_NAME} PRIVATE
	"NVDLA_UTILS_ERROR_TAG=\"DLA_RUNTIME\""
)

install(TARGETS ${PROJECT_NAME} LIBRARY DESTINATION lib)
