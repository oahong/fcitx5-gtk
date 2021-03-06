cmake_minimum_required(VERSION 3.6)
set(FCITX5_GTK_VERSION 4.99.0)
project(fcitx5-gtk VERSION ${FCITX5_GTK_VERSION})

find_package(ECM REQUIRED 1.0.0)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})

include(FindPkgConfig)
include(ECMSetupVersion)
include(GenerateExportHeader)
include(FeatureSummary)
include(GNUInstallDirs)
include(FcitxCompilerSettings)
include(ECMUninstallTarget)

option(ENABLE_GIR "GObject Introspection" ON)
option(ENABLE_GTK2_IM_MODULE "Enable GTK2 IM Module" ON)
option(ENABLE_GTK3_IM_MODULE "Enable GTK3 IM Module" ON)
option(ENABLE_SNOOPER "Enable Key Snooper for gtk app" ON)

if(NOT NO_SNOOPER_APPS)
  set(NO_SNOOPER_APPS ".*chrome.*,.*chromium.*,firefox.*,Do.*"
    CACHE STRING "Disable Key Snooper for following app")
endif()

configure_file(config.h.in "${CMAKE_CURRENT_BINARY_DIR}/config.h")
include_directories("${CMAKE_CURRENT_BINARY_DIR}")
find_package(PkgConfig)
find_package(XKBCommon)
find_package(Fcitx5Utils)
pkg_check_modules(GLib2 REQUIRED IMPORTED_TARGET "glib-2.0")
pkg_check_modules(Gio2 REQUIRED IMPORTED_TARGET "gio-2.0")
pkg_check_modules(GObject2 REQUIRED IMPORTED_TARGET "gobject-2.0")
find_package(X11 REQUIRED)
add_subdirectory(fcitx-gclient)

if (ENABLE_GTK2_IM_MODULE)
add_subdirectory(gtk2)
endif()

if (ENABLE_GTK3_IM_MODULE)
add_subdirectory(gtk3)
endif()

feature_summary(WHAT ALL FATAL_ON_MISSING_REQUIRED_PACKAGES)
