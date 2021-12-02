# CMakeList.txt: proyecto de CMake para 3DPrintIndexer, incluya el origen y defina
# la lógica específica del proyecto aquí.
#
cmake_minimum_required (VERSION 3.8)

project("ImGuiBackends" VERSION 1.0)

option(IMGUI_WITH_SDL "Compile SDL backend" ON)
option(IMGUI_WITH_SDL_RENDERER "Compile with SDL Renderer" ON)

file(GLOB IMGUIBKND_HEADERS)
file(GLOB IMGUIBKND_SOURCES)

if(IMGUI_WITH_SDL)
	list (APPEND IMGUIBKND_HEADERS imgui_impl_sdl.h)
	list (APPEND IMGUIBKND_SOURCES imgui_impl_sdl.cpp)
endif()
if(IMGUI_WITH_SDL_RENDERER)
	list (APPEND IMGUIBKND_HEADERS imgui_impl_sdlrenderer.h)
	list (APPEND IMGUIBKND_SOURCES imgui_impl_sdlrenderer.cpp)
endif()

source_group("Header Files" FILES ${IMGUIBKND_HEADERS})
source_group("Source Files" FILES ${IMGUIBKND_SOURCES})
source_group("CMake Files" FILES CMakeLists.txt)

if(IMGUI_WITH_SDL)
	if(BUILD_WITH_SHARED_LIBS)
		set(IMGUIBKND_PRJ_NAME ImGuiSDL)
		add_library (${IMGUIBKND_PRJ_NAME} SHARED ${IMGUIBKND_SOURCES} ${IMGUIBKND_HEADERS})
		add_dependencies(${IMGUIBKND_PRJ_NAME} SDL2 ImGui)
		target_link_libraries(${IMGUIBKND_PRJ_NAME} PRIVATE SDL2 ImGui)
		target_include_directories(${IMGUIBKND_PRJ_NAME} PUBLIC
				"${PROJECT_SOURCE_DIR}"
				"${PROJECT_SOURCE_DIR}/..")
	endif()
	if(BUILD_WITH_STATIC_LIBS)
		set(IMGUIBKND_PRJ_NAME ImGuiSDL_static)
		add_library (${IMGUIBKND_PRJ_NAME} STATIC ${IMGUIBKND_SOURCES} ${IMGUIBKND_HEADERS})
		add_dependencies(${IMGUIBKND_PRJ_NAME} SDL2-static ImGui)
		target_link_libraries(${IMGUIBKND_PRJ_NAME} PRIVATE SDL2-static ImGui)
		target_include_directories(${IMGUIBKND_PRJ_NAME} PUBLIC
				"${PROJECT_SOURCE_DIR}"
				"${PROJECT_SOURCE_DIR}/..")
	endif()
endif()