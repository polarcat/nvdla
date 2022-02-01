cmake_minimum_required(VERSION 3.5)

project(nvdlacompiler)

set(CMAKE_VERBOSE_MAKEFILE ON)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
	set(CMAKE_CXX_STANDARD 11)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
	add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(sources
	caffe/CaffeParser.cpp
	caffe/ditcaffe/protobuf-2.6.1/ditcaffe.pb.cpp
	engine-ast/ActivationOp.cpp
	engine-ast/BatchNormOp.cpp
	engine-ast/BDMANode.cpp
	engine-ast/BDMASingleOp.cpp
	engine-ast/BDMAGroupOp.cpp
	engine-ast/BiasOp.cpp
	engine-ast/CDPLRNOp.cpp
	engine-ast/CDPNode.cpp
	engine-ast/ConcatOp.cpp
	engine-ast/ConvCoreNode.cpp
	engine-ast/ConvolutionOp.cpp
	engine-ast/CPUNode.cpp
	engine-ast/DeconvolutionOp.cpp
	engine-ast/EngineAST.cpp
	engine-ast/EngineEdge.cpp
	engine-ast/EngineGraph.cpp
	engine-ast/EngineNode.cpp
	engine-ast/EngineNodeFactory.cpp
	engine-ast/FullyConnectedOp.cpp
	engine-ast/MultiOpsNode.cpp
	engine-ast/NestedGraph.cpp
	engine-ast/PDPNode.cpp
	engine-ast/RubikNode.cpp
	engine-ast/ScaleOp.cpp
	engine-ast/SDPEltWiseOp.cpp
	engine-ast/SDPNode.cpp
	engine-ast/SDPNOP.cpp
	engine-ast/SDPSuperOp.cpp
	engine-ast/SoftMaxOp.cpp
	engine-ast/SplitOp.cpp
	AST.cpp
	CanonicalAST.cpp
	Check.cpp
	Compiler.cpp
	DlaPrototestInterface.pb.cpp
	DLAResourceManager.cpp
	DLAInterface.cpp
	DLAInterfaceA.cpp
	Layer.cpp
	LutManager.cpp
	Memory.cpp
	Network.cpp
	Profile.cpp
	Profiler.cpp
	Setup.cpp
	Surface.cpp
	TargetConfig.cpp
	Tensor.cpp
	TestPointParameter.cpp
	Wisdom.cpp
	WisdomContainer.cpp
	../common/EMUInterface.cpp
	../common/EMUInterfaceA.cpp
	../common/Loadable.cpp
	../../../utils/BitBinaryTree.c
	../../../utils/BuddyAlloc.c
	../../../utils/ErrorLogging.c
	../../../port/linux/nvdla_os.c
)

include_directories(
	include
	../common/include
	../../include
	../../../port/linux/include
	../../../external/include
	../../../external/protobuf-2.6/src
)

add_compile_options(-W -Wno-multichar -Wno-unused-parameter)
add_compile_options(-Wno-unused-function)
add_compile_options(-fPIC -finline)
add_library(${PROJECT_NAME} SHARED ${sources})

target_compile_definitions(${PROJECT_NAME} PRIVATE
	"NVDLA_UTILS_ERROR_TAG=\"DLA\""
)

target_link_libraries(${PROJECT_NAME} ${CMAKE_SOURCE_DIR}/libprotobuf.a)

install(TARGETS ${PROJECT_NAME} LIBRARY DESTINATION lib)
