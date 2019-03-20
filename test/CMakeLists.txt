
find_package (Boost REQUIRED COMPONENTS unit_test_framework )
message ("Boost: ${Boost_INCLUDE_DIRS}")
include_directories (${CMAKE_SOURCE_DIR}/include)
include_directories (${Boost_INCLUDE_DIRS})

foreach (BASE 
    1d_color_consistency
    1d_tests
    2d_color_consistency
    svg_test_2d_boxplot
    test_svg
    test_svg_xml
    )
  add_executable (${BASE} ${BASE}.cpp)
  target_link_libraries (${BASE} ${Boost_LIBRARIES})
  target_compile_definitions (${BASE} PUBLIC -DBOOST_TEST_MAIN)
endforeach()