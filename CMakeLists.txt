cmake_minimum_required(VERSION 3.0)

project(plasma-volume-control)

set(PROJECT_VERSION "5.14.80")
set(PROJECT_VERSION_MAJOR 5)

set(QT_MIN_VERSION "5.11.0")
set(KF5_MIN_VERSION "5.50.0")

find_package(ECM ${KF5_MIN_VERSION} REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake/)

add_definitions(-DTRANSLATION_DOMAIN=\"kcm_pulseaudio\" -DQT_NO_KEYWORDS)

include(FeatureSummary)
include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDECompilerSettings NO_POLICY_SCOPE)
include(ECMOptionalAddSubdirectory)

include(FindPkgConfig)
pkg_check_modules(GCONF gconf-2.0)
pkg_check_modules(GOBJECT gobject-2.0)
if (GCONF_FOUND AND GOBJECT_FOUND)
    set(HAVE_GCONF TRUE)
endif()

find_package(Qt5 ${QT_MIN_VERSION} REQUIRED COMPONENTS
    Core
    Gui
    DBus
    Widgets
    Quick
)
find_package(KF5 ${KF5_MIN_VERSION} REQUIRED COMPONENTS
    CoreAddons
    Declarative
    DocTools
    GlobalAccel
    I18n
    Plasma
)
find_package(PulseAudio 5.0.0 REQUIRED)
find_package(Canberra REQUIRED)
find_package(GLIB2 REQUIRED)

configure_file(config.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/config.h)

add_subdirectory(applet)
add_subdirectory(src)
add_subdirectory(data)
add_subdirectory(doc)

feature_summary(WHAT ALL FATAL_ON_MISSING_REQUIRED_PACKAGES)
