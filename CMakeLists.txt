# Setup the XPlane SDK
cmake_minimum_required (VERSION 3.18)

# Define the xplm project as an interface
project(xplm VERSION 1.0)
add_library(xplm INTERFACE)

# Determine platform-specific items
if(WIN32)
    set(LIB_XPLANE_SDK_PATH "${CMAKE_CURRENT_SOURCE_DIR}/Libraries/Win")
    set(LIB_XPLANE_SDK_LIB_NAME "XPLM_64.lib")
    set(LIB_XPLANE_SDK_PLATFORM "IBM")
elseif(UNIX)
    set(LIB_XPLANE_SDK_PLATFORM "LIN")
else()
    message(FATAL_ERROR "Unknown platform provided")
endif()

# Find the Required XPLM Library

if(LIB_XPLANE_SDK_PATH)
    find_library(
        LIB_XPLM
        NAMES "${LIB_XPLANE_SDK_LIB_NAME}"
        PATHS "${LIB_XPLANE_SDK_PATH}"
        NO_DEFAULT_PATH
        REQUIRED
    )
    target_link_libraries(xplm INTERFACE "${LIB_XPLM}")
endif()

# Set interface directories and definitions
set_property(TARGET xplm APPEND PROPERTY INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/CHeaders/XPLM")
set_property(TARGET xplm APPEND PROPERTY INTERFACE_COMPILE_DEFINITIONS ${LIB_XPLANE_SDK_PLATFORM})
