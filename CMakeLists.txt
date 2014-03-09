cmake_minimum_required (VERSION 2.8)

################################
# Coconut project
project (Coconut)

############
# Add GLEW 

set(GLEW_VERSION "glew-1.10.0") 
set(GLEW_DIRECTORY ${CMAKE_SOURCE_DIR}/external/${GLEW_VERSION} ) 

set(GLEW_INCLUDE_DIR	"${GLEW_DIRECTORY}/include")  
SET(GLEW_LIBRARY		"${GLEW_DIRECTORY}/lib/Release/Win32/glew32s.lib")

#include_directories( ${GLEW_INCLUDE_DIR} )

############
# Find GLFW

set(GLFW_VERSION "glfw-3.0.3") 
set( SEARCH_PATHS
	${GLFW_PATH}
	${GLFW_PATH}/include
	${GLFW_PATH}/lib 
	${CMAKE_SOURCE_DIR}/external/${GLFW_VERSION}/include 
	${CMAKE_SOURCE_DIR}/external/${GLFW_VERSION}/build_vs2012x86
	${CMAKE_SOURCE_DIR}/external/${GLFW_VERSION}/build_vs2012x86/Release
	)

FIND_PATH( GLFW_INCLUDE_DIR GLFW/glfw3.h PATHS ${SEARCH_PATHS} ) # cjren, glfw3
FIND_LIBRARY( GLFW_LIBRARY glfw3 PATHS ${SEARCH_PATHS} )
SET(GLFW_LIBRARY			"${CMAKE_SOURCE_DIR}/external/${GLFW_VERSION}/build_vs2012x86/Release/glfw3.lib") 


IF( GLFW_INCLUDE_DIR AND GLFW_LIBRARY )
	SET( GLFW_FOUND TRUE )
ENDIF( GLFW_INCLUDE_DIR AND GLFW_LIBRARY )

IF( GLFW_FOUND )
	MESSAGE (" Found GLFW ")
ELSE( GLFW_FOUND )
	MESSAGE( FATAL_ERROR "Could not find GLFW! Please provide the path \n"
						"containing the 'lib' and 'include' subdirectories \n"
						"that have the glfw header and static library via \n"
						"-DGLFW_PATH=\"path\"" )
ENDIF( GLFW_FOUND ) 


#include_directories( ${GLFW_INCLUDE_DIR} )

################################
# Coconut common 
set( CoconutCommon_LIBS CoconutCommon )
set( CoconutCommon_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/common )

include_directories( ${CoconutCommon_INCLUDE_DIR} )

################################
# 
add_subdirectory(common) 
add_subdirectory(projects) 

################################
# Add custom target to copy all data

# this will create a DATA_COPY project, for example in .vcproject, build it then it does the copying.
set(TARGET_DATA_COPY DATA_COPY)
add_custom_target(
	${TARGET_DATA_COPY}
	COMMAND cmake -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/data ${CMAKE_CURRENT_BINARY_DIR}/data	
)
