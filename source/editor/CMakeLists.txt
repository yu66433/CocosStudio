
set(PROJECT_NAME CocosStudioUI)
set(SRCS	main.cpp
			MyApp.h
			CocosGLCanvas.cpp
			CocosGLCanvas.h
			MainFrame.cpp
			MainFrame.h
			MainFrameWrapper.cpp
			MainFrameWrapper.h
			stdafx.h
			stdafx.cpp
			EditorScene3D.cpp
			EditorScene3D.h
			EditorGrid.cpp
			EditorGrid.h
			DrawNode3D.cpp
			DrawNode3D.h
			CustomDockArt.h
			CustomDockArt.cpp
			CustomTabArt.h
			CustomTabArt.cpp
			AbstractCocosGLCanvasController.h			
			CocosGLCanvas3DController.h
			CocosGLCanvas3DController.cpp
			CocosGLCanvas2DController.h
			CocosGLCanvas2DController.cpp)


include(${wxWidgets_USE_FILE})
add_executable(${PROJECT_NAME} ${SRCS})
set_target_properties(${PROJECT_NAME} PROPERTIES
                      ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
                      LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
                      RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
					  
					  
					  
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../LibEditorModel)
target_link_libraries(${PROJECT_NAME} LibEditorModel)
					  
if(WIN32)
	#set precompiled header 
	ADD_PRECOMPILED_HEADER(${PROJECT_NAME} stdafx.h [FORCEINCLUDE])
	
	set_target_properties (${PROJECT_NAME} PROPERTIES LINK_FLAGS "/SUBSYSTEM:windows")
	
	#copy cocos2d-x dll
	add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
									COMMAND ${CMAKE_COMMAND} -E copy_directory
									${COCOS2D_LIB_DIR} $<TARGET_FILE_DIR:${PROJECT_NAME}>)
									
	#copy wxwidgets dll
	add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
									COMMAND ${CMAKE_COMMAND} -E copy_directory
									${wxWidgets_LIB_DIR} $<TARGET_FILE_DIR:${PROJECT_NAME}>)
								

	
	target_link_libraries(${PROJECT_NAME} ${COCOS2D_LIBS} ${wxWidgets_LIBRARIES})
else()
	target_link_libraries(${PROJECT_NAME} ${COCOS2D_LIBS}
										${wxWidgets_LIBRARIES}
										${GLEW_LIBRARIES}
										${GLUT_LIBRARY}
										${OPENGL_LIBRARIES}
										${X11_X11_LIB}
										${X11_Xrandr_LIB}
										${X11_Xinerama_LIB}
										${X11_Xxf86vm_LIB}
										${X11_Xcursor_LIB}
										${PNG_LIBRARIES}
										-lfontconfig)
 endif()
 

 

#copy resources from resource folder for cocos view
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
                                   COMMAND ${CMAKE_COMMAND} -E copy_directory
                                   ${CocosResources} $<TARGET_FILE_DIR:${PROJECT_NAME}>/Resources)
								   
#copy resources from WxWidgetsResources folder for buttons icons and all this stuff
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
                                   COMMAND ${CMAKE_COMMAND} -E copy_directory
                                   ${WxWidgetsResources} $<TARGET_FILE_DIR:${PROJECT_NAME}>/wxWidgetsResources)
								   
								   



