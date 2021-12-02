# CMakeList.txt: proyecto de CMake para 3DPrintIndexer, incluya el origen y defina
# la lógica específica del proyecto aquí.
#
cmake_minimum_required (VERSION 3.8)

project("ImGui" VERSION 1.0)

option(IMGUI_WITH_BACKENDS "Compile backends" ON)

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
#		imgui_demo.cpp
		imgui_draw.cpp
		imgui_tables.cpp
		imgui_widgets.cpp)

source_group("Header Files" FILES ${IMGUI_HEADERS})
source_group("Source Files" FILES ${IMGUI_SOURCES})
source_group("CMake Files" FILES CMakeLists.txt)

add_library (ImGui STATIC ${IMGUI_SOURCES} ${IMGUI_HEADERS})

# TODO: Agregue pruebas y destinos de instalación si es necesario.

include(GNUInstallDirs)

install(FILES ${IMGUI_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/imgui)

if(BUILD_WITH_STATIC_LIBS)
	install(TARGETS ImGui_static
		EXPORT ImGui_static
		RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
		LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
		ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
		)
endif()

if(IMGUI_WITH_BACKENDS)
	add_subdirectory(backends)
endif()