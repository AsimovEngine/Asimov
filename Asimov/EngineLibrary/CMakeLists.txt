file(#dont do this
	GLOB
	LIBRARY_SOURCES
	CONFIGURE_DEPENDS
	${CMAKE_CURRENT_SOURCE_DIR}/src/Core/*.cpp
)

#shared engine library, shared, not static
add_library(
	Library
	SHARED
	${LIBRARY_SOURCES}
)

#build type
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
	set(CMAKE_BUILD_TYPE Debug CACHE STRING "Build Type" FORCE)
	set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS Debug Release)
endif()

#definitions for spdlog
target_compile_definitions(
	Library
	PRIVATE
	$<$<CONFIG:Debug>:DEBUG_MODE_ENABLED>
	$<$<CONFIG:Release>:RELEASE_MODE_ENABLED>
)

#includes
target_include_directories(
	Library
	PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
)

#pch
target_precompile_headers(
	Library
	PRIVATE
	<cstdio>
	<memory>
	<unordered_map>
	<vector>
	<string>
)

add_subdirectory(vendor/imgui)


target_link_libraries(Library PRIVATE imgui_lib)

target_compile_features(Library PUBLIC cxx_std_20)
