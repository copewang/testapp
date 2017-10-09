# Copyright (c) 2014, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 2.8.11)
project(testapp)

### Emulate toolchain
set(CMAKE_OSX_SYSROOT "iphoneos")
set(CMAKE_XCODE_EFFECTIVE_PLATFORMS "-iphoneos;-iphonesimulator")
set(CMAKE_DEBUG_POSTFIX d)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
### -- end

if(NOT XCODE_VERSION)
  message(FATAL_ERROR "Xcode only")
endif()

set(
    SOURCES
    sources/AppDelegate.hpp
    sources/AppDelegate.mm
    sources/ViewController.hpp
    sources/ViewController.mm
    sources/main.mm
)

set(
    IMAGES
    images/launch_640x960@2x.png
    images/launch_640x1136@2x.png
    images/launch_750x1334@2x.png
    images/launch_1242x2208@2x.png
)

set(
    ICONS
    icons/settings/Icon@2x.png # iPhone Retina, 58x58
    icons/settings/Icon-Small.png # iPad Non-Retina 29x29
)

set(
    STORYBOARDS
    storyboards/MainStoryboard_iPad.storyboard
    storyboards/MainStoryboard_iPhone.storyboard
)

add_executable(
    testapp
    ${ICONS}
    ${IMAGES}
    ${SOURCES}
    ${STORYBOARDS}
)

set_target_properties(
    testapp
    PROPERTIES
    MACOSX_BUNDLE YES
    MACOSX_BUNDLE_INFO_PLIST "${CMAKE_CURRENT_LIST_DIR}/plist.in"
    XCODE_ATTRIBUTE_CODE_SIGN_IDENTITY "iPhone Developer"
    XCODE_ATTRIBUTE_TARGETED_DEVICE_FAMILY "1,2"
    XCODE_ATTRIBUTE_CLANG_ENABLE_OBJC_ARC YES
    XCODE_ATTRIBUTE_INSTALL_PATH "${CMAKE_BINARY_DIR}/ProductRelease"
    XCODE_ATTRIBUTE_COMBINE_HIDPI_IMAGES "NO"
    RESOURCE "${IMAGES};${STORYBOARDS};${ICONS}"
)

set_target_properties(
    testapp
    PROPERTIES
    XCODE_ATTRIBUTE_PRODUCT_NAME
    "TestApp"
    XCODE_ATTRIBUTE_BUNDLE_IDENTIFIER
    "com.github.ruslo.TestApp"
)

set_target_properties(
    testapp
    PROPERTIES
    XCODE_ATTRIBUTE_PRODUCT_NAME[variant=Debug]
    "TestApp-Dbg"
    XCODE_ATTRIBUTE_BUNDLE_IDENTIFIER[variant=Debug]
    "com.github.ruslo.TestApp.debug"
)

target_link_libraries(
    testapp
    "-framework CoreGraphics"
    "-framework Foundation"
    "-framework UIKit"
)
