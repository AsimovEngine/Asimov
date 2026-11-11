# static build
set(imgui_lib_path "${CMAKE_CURRENT_SOURCE_DIR}/staticlib/libimgui_lib.a")

# check for static build, and use that
if(EXISTS "${imgui_lib_path}")
	# message("Using prebuilt imgui")

	add_library(
		imgui_lib
		STATIC
		IMPORTED
		GLOBAL
	)

	set_target_properties(
		imgui_lib
		PROPERTIES
		IMPORTED_LOCATION "${imgui_lib_path}"
		INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/include"
		INTERFACE_COMPILE_FEATURES cxx_std_20
	)

# build from source
else()
	# message("Building imgui sources")

	file(
		GLOB
		IMGUI_SOURCES
		CONFIGURE_DEPENDS
		${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp
	)

	add_library(
		imgui_lib
		STATIC
		${IMGUI_SOURCES}
	)

	if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
		set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
	endif()

	target_include_directories(
		imgui_lib
		PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include/imgui
	)

	target_compile_features(imgui_lib PUBLIC cxx_std_20)

endif()

