set(FCITX_GTK3_IM_MODULE_SOURCES
  fcitxim.c
  fcitximcontext.cpp
  )

pkg_check_modules(Gtk3 REQUIRED IMPORTED_TARGET "gtk+-3.0")
pkg_check_modules(Gdk3 REQUIRED IMPORTED_TARGET "gdk-3.0")
pkg_check_modules(Gdk3X11 REQUIRED IMPORTED_TARGET "gdk-x11-3.0")
pkg_get_variable(GTK3_BINARY_VERSION "gtk+-3.0" "gtk_binary_version")

if (NOT DEFINED GTK3_IM_MODULEDIR)
    set(GTK3_IM_MODULEDIR "${CMAKE_INSTALL_LIBDIR}/gtk-3.0/${GTK3_BINARY_VERSION}/immodules" CACHE PATH "Gtk3 im module directory")
endif()

set(FCITX_GTK3_IM_MODULE_SOURCES ${FCITX_GTK3_IM_MODULE_SOURCES})

add_library(im-fcitx-gtk3 MODULE ${FCITX_GTK3_IM_MODULE_SOURCES})
set_target_properties( im-fcitx-gtk3 PROPERTIES PREFIX "" OUTPUT_NAME "im-fcitx5"
  COMPILE_FLAGS "-fvisibility=hidden -fno-exceptions" LINK_FLAGS "-Wl,--no-undefined")
target_include_directories(im-fcitx5 PRIVATE ${X11_X11_INCLUDE_PATH})
target_link_libraries(im-fcitx-gtk3 XKBCommon::XKBCommon PkgConfig::Gtk3 PkgConfig::Gdk3 PkgConfig::Gdk3X11 Fcitx5::GClient Fcitx5::Utils ${X11_LIBRARIES})
install(TARGETS im-fcitx-gtk3 DESTINATION "${GTK3_IM_MODULEDIR}")
