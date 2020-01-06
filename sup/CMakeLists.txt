set(sup-sources
    ioutil.cpp
    json_meter.cpp
    path.cpp
)

add_library(arbor-sup ${sup-sources})
add_library(arborsup-public-headers INTERFACE)
target_include_directories(arborsup-public-headers INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Compile sup library with the same optimization flags as libarbor.
target_compile_options(arbor-sup PRIVATE ${ARB_CXXOPT_ARCH})

# The sup library uses both the json library and libarbor
target_link_libraries(arbor-sup PUBLIC ext-json arbor arborsup-public-headers)

set_target_properties(arbor-sup PROPERTIES OUTPUT_NAME arborsup)

install(DIRECTORY include/sup
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        FILES_MATCHING PATTERN "*.hpp")

install(TARGETS arborsup-public-headers EXPORT arbor-targets)
install(TARGETS arbor-sup EXPORT arbor-targets ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})

