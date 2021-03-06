
set(NAME gala-elementary-alt-tab)
cmake_minimum_required (VERSION 2.8)
cmake_policy (VERSION 2.8)

project(${NAME})
list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)

enable_testing()

find_package (Vala REQUIRED)
include (ValaVersion)
ensure_vala_version ("0.22" MINIMUM)
include (ValaPrecompile)

find_package (PkgConfig)
pkg_check_modules(DEPS REQUIRED gala granite libwnck-3.0)
add_definitions (${DEPS_CFLAGS})
add_definitions(-DWNCK_I_KNOW_THIS_IS_UNSTABLE)
link_directories (${DEPS_LIBRARY_DIRS})

vala_precompile(VALA_C
	src/Main.vala
	src/Widgets/RoundedActor.vala
	src/Widgets/RoundedText.vala
	src/Widgets/WindowPreviewActor.vala
	src/Settings.vala
	src/Utils.vala

PACKAGES
	gala
	granite
	xfixes-4.0
	libwnck-3.0
OPTIONS
	--vapidir=${CMAKE_CURRENT_SOURCE_DIR}/vapi
)

add_library(${NAME} SHARED ${VALA_C})
target_link_libraries(${NAME} ${DEPS_LIBRARY} m)

install(TARGETS ${NAME} DESTINATION lib/x86_64-linux-gnu/gala/plugins)

include(GSettings)
add_schema("data/org.pantheon.desktop.gala.plugins.elementary-alt-tab.gschema.xml")
