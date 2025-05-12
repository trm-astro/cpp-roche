cmake_minimum_required(VERSION 3.23)
project(cpp-roche)

include(ExternalProject)

message("CMakeLists.txt for cpp-roche")
message("PROJECT_SOURCE_DIR: ${PROJECT_SOURCE_DIR}")
message("CMAKE_BINARY_DIR: ${CMAKE_BINARY_DIR}")

set(PACKAGE_NAME roche)

############################################################
# Define sources
############################################################

set(SOURCES
    src/blink.cc
    src/hits.cc
    src/rpot.cc
    src/gsint.cc
    src/rdot.cc
    src/strinit.cc
    src/rlobe_eggleton.cc
    src/irrad.cc
    src/rocacc.cc
    src/xl1.cc
    src/xl2.cc
    src/xl3.cc
    src/lobe1.cc
    src/lobe2.cc
    src/flobe1.cc
    src/streamr.cc
    src/stradv.cc
    src/strmnx.cc
    src/drpot.cc
    src/vtrans.cc
    src/vlobe1.cc
    src/vlobe2.cc
    src/zeta_rlobe_eggleton.cc
    src/contact.cc
    src/vstream.cc
    src/sphere_eclipse.cc
    src/ref_sphere.cc
    src/pot_min.cc
    src/rpot_val.cc
    src/rpot_grad.cc
    src/rpot_val_grad.cc
    src/fblink.cc
    src/ingress_egress.cc
    src/face.cc
    src/set_earth.cc
    src/roche_shadow.cc
    src/disc_eclipse.cc
    src/stream.cc
    src/vstrreg.cc
    src/jacobi.cc
    src/xl11.cc
    src/xl12.cc
)

############################################################
# Define the library
############################################################
add_library(${PACKAGE_NAME} SHARED 
    ${SOURCES}
)

target_include_directories(${PACKAGE_NAME}
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

############################################################
# Check for and install dependencies
############################################################
# Configure the PLPLOT library
if(NOT DEFINED CMAKE_COMMON)
    set(CMAKE_COMMON ${PROJECT_SOURCE_DIR}/../../cmake_common)
endif()
if(NOT DEFINED PLPLOT_FOUND)
    include("${CMAKE_COMMON}/plplot_install.cmake")
endif()

# find_package(BINARY REQUIRED)

# if (NOT BINARY_FOUND)
#     message(FATAL_ERROR "BINARY library not found.")
# endif()

target_include_directories(${PACKAGE_NAME}
    PUBLIC 
        $<BUILD_INTERFACE:${PLPLOT_INCLUDE_DIR}>
)
target_link_libraries(${PACKAGE_NAME} 
    PUBLIC 
        binary
)

# Set RPATH properties
set_target_properties(${PACKAGE_NAME} PROPERTIES
    INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib"
    BUILD_WITH_INSTALL_RPATH TRUE
    INSTALL_RPATH_USE_LINK_PATH TRUE
    BUILD_RPATH_USE_ORIGIN TRUE
)

############################################################
# Create an executable
############################################################

# No executable in this code, this is a library

############################################################
# Install
############################################################
include("${CMAKE_COMMON}/install_lib.cmake") # Get the generic install function

install_lib(${PACKAGE_NAME})