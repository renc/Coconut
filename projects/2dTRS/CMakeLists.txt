##########
# project 

set(NAME 2dTRS)

if( WIN32 )
	# set this to be window not console program. 
	# for vs Linker/System/...
	#set( CONSOLE_SYSTEM WIN32 )    
endif()

########

source_group(
	"Shader Files" 
	FILES 
	${SHADER_FILES})

if (WIN32)
  add_definitions(
      -D_USE_MATH_DEFINES  
      -D_UNICODE -D__UNICODE	
  )  
endif ()

add_executable("2dTRS"

	#${CONSOLE_SYSTEM}
    
	main.cpp 
	${SHADER_FILES}
)
	
include_directories( ${GLEW_INCLUDE_DIR} )
include_directories( ${GLFW_INCLUDE_DIR} ) 

target_link_libraries(
	${NAME} 

	"winmm.lib" # why need this? explain at the end.
	"OpenGL32"
	${GLEW_LIBRARY}
	${GLFW_LIBRARY}
	${CoconutCommon_LIBS}
)

###########
# to create a 2dTRS_DATA_COPY project, 
# this will copy the data folder into other place, when you build this project.
set(TARGET_DATA_COPY 2dTRS_DATA_COPY)
add_custom_target(
	${TARGET_DATA_COPY}
	COMMAND cmake -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/data ${CMAKE_BINARY_DIR}/data
) 