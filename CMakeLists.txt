CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
PROJECT(BACKGROUND_SPLIT)

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

FIND_PACKAGE(OpenCV REQUIRED)
LINK_DIRECTORIES(${PROJECT_BINARY_DIR}/lib)

INCLUDE_DIRECTORIES(./)
INCLUDE_DIRECTORIES(./src)

# GaussBGDifference，高斯背景差分法动态链接库生成
SET(LIB_GAUSSBGDIFF_SOURCE
	./src/GaussBGDifference/gaussbgdifference.h
	./src/GaussBGDifference/gaussbgdifference.cpp)
ADD_LIBRARY(gaussBG_diff SHARED ${LIB_GAUSSBGDIFF_SOURCE})
TARGET_LINK_LIBRARIES(gaussBG_diff
	${OpenCV_LIBS})

# ViBe动态链接库生成
SET(LIB_VIBE_SOURCE
	./src/ViBe/Vibe.h
	./src/ViBe/Vibe.cpp)
ADD_LIBRARY(vibe SHARED ${LIB_VIBE_SOURCE})
TARGET_LINK_LIBRARIES(vibe
	${OpenCV_LIBS})

# 生成FrameDifference测试程序
ADD_EXECUTABLE(FrameDifference_test ./src/FramesDifference/main.cpp)
TARGET_LINK_LIBRARIES(FrameDifference_test
	${OpenCV_LIBS})


# 生成GaussBGDifference测试程序
SET(LIB_GAUSSBGDIFF gaussBG_diff)
ADD_EXECUTABLE(gaussBG_diff_test ./src/GaussBGDifference/main.cpp)
TARGET_LINK_LIBRARIES(gaussBG_diff_test
	${LIB_GAUSSBGDIFF})

# 生成ViBe测试程序
SET(LIB_VIBE vibe)
ADD_EXECUTABLE(vibe_test ./src/ViBe/main.cpp)
TARGET_LINK_LIBRARIES(vibe_test
	${LIB_VIBE})


