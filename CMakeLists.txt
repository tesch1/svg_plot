project (svg_plot CXX)
cmake_minimum_required (VERSION 3.0)
set (CMAKE_CXX_STANDARD 14)

option (SVGP_WITH_QUAN "Get the quan library." OFF)

if (NOT WIN32)
  set (DOWNLOAD_DIR "$ENV{HOME}/Downloads")
else (NOT WIN32)
  set (DOWNLOAD_DIR "C:/Downloads")
endif (NOT WIN32)

find_package (Boost REQUIRED)
message ("Boost: ${Boost_INCLUDE_DIRS}")
include_directories (${CMAKE_SOURCE_DIR}/include)
include_directories (${Boost_INCLUDE_DIRS})

#if (SVGP_WITH_QUAN)
  include (ExternalProject)
  ExternalProject_Add (quanX
    PREFIX quanX
    URL "https://github.com/pabristow/quan/zipball/7835a41c294333e9f27f83a3336eb4e7c5e90e90"
    #URL_HASH MD5=
    DOWNLOAD_DIR ${DOWNLOAD_DIR}
    UPDATE_COMMAND ""
    PATCH_COMMAND ""
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ""
    INSTALL_COMMAND ""
    LOG_DOWNLOAD ON
    )
  ExternalProject_Get_Property (quanX source_dir)
  include_directories ("${source_dir}/include")
  add_definitions (-DHAVE_QUAN)
#endif (SVGP_WITH_QUAN)

add_subdirectory (example)
add_subdirectory (test)

