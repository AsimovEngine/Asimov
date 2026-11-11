add_executable(
	Editor
	src/Editor.cpp
)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
	set(CMAKE_BUILD_TYPE Debug CACHE STRING "Build Type" FORCE)
	set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS Debug Release)
endif()

target_compile_definitions(Editor PRIVATE
	$<$<CONFIG:Debug>:DEBUG_MODE_ENABLED>
	$<$<CONFIG:Release>:RELEASE_MODE_ENABLED>
)

target_link_libraries(Editor PRIVATE Library)
