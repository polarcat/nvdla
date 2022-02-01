cmake_minimum_required(VERSION 3.5)

project(nvdla-compiler)

set(CMAKE_VERBOSE_MAKEFILE ON)

if(NOT CMAKE_CXX_STANDARD)
	set(CMAKE_CXX_STANDARD 11)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
	add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(sources
	main.cpp
	CompileTest.cpp
	GenerateTest.cpp
	ParseTest.cpp
)

include_directories(
	include
	../../core/include
	../../port/linux/include
	../../external/include
)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../../core/src/compiler
	${CMAKE_CURRENT_BINARY_DIR}/lib
)
add_compile_options(-W -Wno-multichar -Wno-unused-parameter)
add_compile_options(-Wno-unused-function)
add_compile_options(-fPIC -finline)
add_executable(${PROJECT_NAME} ${sources})

target_compile_definitions(${PROJECT_NAME} PRIVATE
	"NVDLA_UTILS_ERROR_TAG=\"DLA\""
)

target_link_libraries(${PROJECT_NAME}
	nvdlacompiler
	pthread
	${CMAKE_SOURCE_DIR}/libprotobuf.a
)

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)
