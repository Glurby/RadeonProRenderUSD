project(hdRpr)

cmake_minimum_required(VERSION 3.12)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/defaults
                      ${CMAKE_SOURCE_DIR}/cmake/modules
                      ${CMAKE_SOURCE_DIR}/cmake/macros)

include(Options)

if(RPR_BUILD_AS_HOUDINI_PLUGIN)
    include(Houdini)
endif(RPR_BUILD_AS_HOUDINI_PLUGIN)

include(ProjectDefaults)
include(Packages)

# This has to be defined after Packages is included, because it relies on the
# discovered path to the python executable.
set(PXR_PYTHON_SHEBANG "${PYTHON_EXECUTABLE}"
    CACHE
    STRING
    "Replacement path for Python #! line."
)

# CXXDefaults will set a variety of variables for the project.
# Consume them here. This is an effort to keep the most common
# build files readable.
include(CXXDefaults)
add_definitions(${_PXR_CXX_DEFINITIONS})
set(CMAKE_CXX_FLAGS "${_PXR_CXX_FLAGS} ${CMAKE_CXX_FLAGS}")

include(Public)

set(CMAKE_CXX_STANDARD 14)

include_directories(${USD_INCLUDE_DIR})
link_directories(${USD_LIBRARY_DIR})
include_directories(${RPR_INCLUDE_DIR})
link_directories(${RPR_LIBRARY_DIR})

add_subdirectory(pxr/imaging/plugin/hdRpr)

# install(FILES README.md DESTINATION .)
# install(FILES LICENSE.md DESTINATION .)
