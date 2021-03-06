cmake_minimum_required(VERSION 3.14.5)
# when increasing the minium required version, consider updating in examples as well

file(STRINGS "version.txt" __CPP_BINDGEN_VERSION)
project(cpp_bindgen VERSION ${__CPP_BINDGEN_VERSION} LANGUAGES CXX)
unset(__CPP_BINDGEN_VERSION)

# Switch default of NO_PACKAGE_REGISTRY. TODO: Can be removed with CMake 3.15+
set(CMAKE_EXPORT_NO_PACKAGE_REGISTRY ON CACHE BOOL "")
mark_as_advanced(CMAKE_EXPORT_NO_PACKAGE_REGISTRY)

set(REQUIRED_BOOST_VERSION 1.58)
find_package(Boost ${REQUIRED_BOOST_VERSION} REQUIRED)

# if used via FetchContent/add_subdirectory() we need to make the add_bindings_library() available here
include(${CMAKE_CURRENT_LIST_DIR}/cmake/bindings.cmake)

if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    include(CTest)
    if(BUILD_TESTING)
        add_subdirectory(tests)
    endif()
endif()

include(${CMAKE_CURRENT_LIST_DIR}/cmake/export.cmake)
