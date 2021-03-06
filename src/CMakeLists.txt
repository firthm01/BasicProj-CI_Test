add_executable(basic-proj-exe)

target_sources(basic-proj-exe
    PRIVATE
		BasicProj.cpp
)

if(WIN32)
    file(TO_CMAKE_PATH "$ENV{programfiles}" PROG_FILES)
    file(TO_CMAKE_PATH "$ENV{appdata}" APPDATA)
endif()
if(APPLE)
    set(EPS_PLUGIN_INSTALL_PREFIX "$ENV{HOME}/Library/Audio/Plug-Ins/" CACHE STRING "Plugin path prefix, prepended onto plugin install directories")
    set(EPS_EXTENSION_INSTALL_PREFIX "$ENV{HOME}/Library/Application Support/REAPER/" CACHE STRING "Extension path prefix, prepended onto extension install directory")
endif()
if(WIN32)
    set(EPS_PLUGIN_INSTALL_PREFIX "${PROG_FILES}/Common Files/" CACHE STRING "Plugin path prefix, prepended onto plugin install directories")
    set(EPS_EXTENSION_INSTALL_PREFIX "${APPDATA}/REAPER/" CACHE STRING "Extension path prefix, prepended onto extension install directory")
endif()
set(win_install_dir "${EPS_EXTENSION_INSTALL_PREFIX}UserPlugins")
set(macos_install_dir "${EPS_EXTENSION_INSTALL_PREFIX}UserPlugins")
set(linux_install_dir "share/REAPER/UserPlugins")

install(TARGETS
    basic-proj-exe
    RUNTIME DESTINATION
        $<$<PLATFORM_ID:Windows>:${win_install_dir}>$<$<PLATFORM_ID:Darwin>:${macos_install_dir}>$<$<PLATFORM_ID:Linux>:${linux_install_dir}>
    )