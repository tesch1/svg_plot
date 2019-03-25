project (svg_plot CXX)
cmake_minimum_required (VERSION 3.0)
set (CMAKE_CXX_STANDARD 11)

if (NOT WIN32)
  set (DOWNLOAD_DIR "$ENV{HOME}/Downloads")
else (NOT WIN32)
  set (DOWNLOAD_DIR "C:/Downloads")
endif (NOT WIN32)

include_directories (${CMAKE_SOURCE_DIR}/include)

# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #
# Dependencies
# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #

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

# boost
find_package (Boost)

if (NOT FOUND_Boost)
  ExternalProject_Add (boostX
    PREFIX boostX
    URL "https://dl.bintray.com/boostorg/release/1.69.0/source/boost_1_69_0.tar.bz2"
    URL_HASH SHA256=8f32d4617390d1c2d16f26a27ab60d97807b35440d45891fa340fc2648b04406
    DOWNLOAD_DIR ${DOWNLOAD_DIR}
    UPDATE_COMMAND ""
    PATCH_COMMAND ""
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ""
    INSTALL_COMMAND ""
    LOG_DOWNLOAD ON
    )
  ExternalProject_Get_Property (boostX source_dir)
  set (Boost_INCLUDE_DIRS "${source_dir}")
endif (NOT FOUND_Boost)

message ("Boost: ${Boost_INCLUDE_DIRS}")
include_directories (${Boost_INCLUDE_DIRS})

# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #
# 
#

add_subdirectory (example)
#add_subdirectory (test) # TODO

#
# etags
#
find_program (ETAGS etags)
if (ETAGS)
  add_custom_target (tags
#    COMMAND find ${CMAKE_SOURCE_DIR} -name '*.[ch]pp' -exec etags --language=c++ "{}" "\;"
    COMMAND etags --language=c++ --append `find ${CMAKE_SOURCE_DIR} -name '*.[ch]*'`
    )
  add_custom_target (etags DEPENDS tags)
endif (ETAGS)
