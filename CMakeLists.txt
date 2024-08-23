# Set the C standard and compiler flags
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -Wall -fPIC")

# Include directories
include_directories(${CMAKE_SOURCE_DIR}/include)

# Source files
set(SRC_FILES src/plugin.c)

# Define the output name for the library
set(PLUGIN_NAME "plugin")

# Add the shared library
add_library(${PLUGIN_NAME} SHARED ${SRC_FILES})

# Set target properties based on the platform
if(UNIX)
    set_target_properties(${PLUGIN_NAME} PROPERTIES OUTPUT_NAME ${PLUGIN_NAME})
    set_target_properties(${PLUGIN_NAME} PROPERTIES SUFFIX ".so")
elseif(WIN32)
    # Falls auf Windows, kompiliere eine DLL
    set_target_properties(${PLUGIN_NAME} PROPERTIES OUTPUT_NAME ${PLUGIN_NAME})
    set_target_properties(${PLUGIN_NAME} PROPERTIES SUFFIX ".dll")
endif()

# Custom clean target
add_custom_target(clean
    COMMAND ${CMAKE_COMMAND} -E remove -f ${CMAKE_BINARY_DIR}/*.o ${CMAKE_BINARY_DIR}/${PLUGIN_NAME}.so ${CMAKE_BINARY_DIR}/${PLUGIN_NAME}.dll
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)
