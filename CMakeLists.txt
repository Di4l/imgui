﻿# CMakeList.txt: proyecto de CMake para 3DPrintIndexer, incluya el origen y defina
# la lógica específica del proyecto aquí.
#

cmake_minimum_required (VERSION 3.8)

project("ImGui" VERSION 1.0)

option(IMGUI_WITH_SDL "Compile SDL backend" ON)
option(IMGUI_WITH_DEMO "Compile with Demo Windows" OFF)

# Add sources to the project
file(GLOB IMGUI_HEADERS
		imconfig.h
		imgui.h
		imgui_internal.h
		imstb_rectpack.h
		imstb_textedit.h
		imstb_truetype.h)
file(GLOB IMGUI_SOURCES
		imgui.cpp
		imgui_draw.cpp
		imgui_tables.cpp
		imgui_widgets.cpp)
		
if(IMGUI_WITH_DEMO)
	list(APPEND IMGUI_SOURCES imgui_demo.cpp)
endif()

if(IMGUI_WITH_SDL)
	list(APPEND IMGUI_HEADERS
		backends/imgui_impl_sdl.h
		backends/imgui_impl_sdlrenderer.h)
	list(APPEND IMGUI_SOURCES
		backends/imgui_impl_sdl.cpp
		backends/imgui_impl_sdlrenderer.cpp)
endif()

source_group("Header Files" FILES ${IMGUI_HEADERS})
source_group("Source Files" FILES ${IMGUI_SOURCES})
source_group("CMake Files" FILES CMakeLists.txt)

add_library (ImGui STATIC ${IMGUI_SOURCES} ${IMGUI_HEADERS})
target_include_directories(ImGui PUBLIC
		"${PROJECT_SOURCE_DIR}"
		"${PROJECT_SOURCE_DIR}/backends"
		"${PROJECT_SOURCE_DIR}/../sdl2/include"
		"${PROJECT_SOURCE_DIR}/../sdl_image/include"
		"${CMAKE_INSTALL_PREFIX}/../../vendor/soci/include")
target_link_directories(ImGui PUBLIC
						"${CMAKE_INSTALL_LIBDIR}"
						"${PROJECT_SOURCE_DIR}/../sdl2/lib/${FT_LIB_PATH_PREFIX}"
						"${PROJECT_SOURCE_DIR}/../sdl_image/lib/${FT_LIB_PATH_PREFIX}"
						)
target_link_libraries(ImGui PRIVATE SDL2 SDL2_image)

if(IMGUI_WITH_DEMO)
	target_compile_definitions(ImGui PUBLIC _IMGUI_WITH_DEMO_)
endif()

file(GLOB SDL_BINARIES 
			"${PROJECT_SOURCE_DIR}/../sdl2/lib/${FT_LIB_PATH_PREFIX}/SDL2.dll"
			"${PROJECT_SOURCE_DIR}/../sdl_image/lib/${FT_LIB_PATH_PREFIX}/*.dll"
			)

include(GNUInstallDirs)

install(FILES ${IMGUI_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/imgui)
install(FILES ${SDL_BINARIES} DESTINATION ${CMAKE_INSTALL_BINDIR})

install(TARGETS ImGui
	EXPORT ImGui
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	)
#if(IMGUI_WITH_BACKENDS)
#	add_subdirectory(backends)
#endif()