# This file is part of Desktop App Toolkit,
# a set of libraries for developing nice desktop applications.
#
# For license and copyright information please follow this link:
# https://github.com/desktop-app/legal/blob/master/LEGAL

add_library(external_zlib INTERFACE IMPORTED GLOBAL)
add_library(desktop-app::external_zlib ALIAS external_zlib)

if (NOT DESKTOP_APP_USE_PACKAGED AND NOT LINUX)
    target_include_directories(external_zlib SYSTEM
    INTERFACE
        ${libs_loc}/zlib
    )
endif()

if (DESKTOP_APP_USE_PACKAGED)
    find_package(ZLIB REQUIRED)
    target_link_libraries(external_zlib INTERFACE ZLIB::ZLIB)
elseif (WIN32)
    target_compile_definitions(external_zlib INTERFACE ZLIB_WINAPI)

    set(zlib_lib_loc ${libs_loc}/zlib/contrib/vstudio/vc14/x86/ZlibStat$<IF:$<CONFIG:Debug>,Debug,ReleaseWithoutAsm>)

    target_link_libraries(external_zlib INTERFACE ${zlib_lib_loc}/zlibstat.lib)
elseif (APPLE AND NOT build_osx)
    target_link_libraries(external_zlib INTERFACE /usr/local/macos/lib/libz.a)
elseif (APPLE)
    target_link_libraries(external_zlib INTERFACE /usr/local/lib/libz.a)
else()
    target_link_libraries(external_zlib INTERFACE z)
endif()
