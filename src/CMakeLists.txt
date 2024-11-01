# Get all header and source files.
file(GLOB SOURCE_LIST CONFIGURE_DEPENDS "${PROJECT_SOURCE_DIR}/src/*.cpp")

# Make an automatic library - will be static or dynamic based on user setting.
add_library(RBP ${SOURCE_LIST})

# Add this line:
add_library(RBP::RBP ALIAS RBP)

# We need this directory, and users of our library will need it too.
target_include_directories(RBP PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_link_libraries(RBP PUBLIC ${RLC_LIBRARY})

# Add this line to ensure RELIC's include directories are correctly passed
target_include_directories(RBP PUBLIC ${RLC_INCLUDE_DIRS})

# Add RPATH settings
set_target_properties(RBP PROPERTIES
    INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib"
    BUILD_WITH_INSTALL_RPATH TRUE
    INSTALL_RPATH_USE_LINK_PATH TRUE)
