# Collect all source files in this module
file(GLOB_RECURSE DATA_SOURCES "*.cc" "*.cpp")
file(GLOB_RECURSE DATA_HEADERS "*.h" "*.hpp")

# Create library
add_library(${PROJECT_NAME}_data STATIC ${DATA_SOURCES} ${DATA_HEADERS})

# Set include directories
target_include_directories(${PROJECT_NAME}_data
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

# Link dependencies
target_link_libraries(${PROJECT_NAME}_data
    PUBLIC
        spdlog::spdlog
        SQLiteCpp
    PRIVATE
        nlohmann_json::nlohmann_json
        ${PROJECT_NAME}_utils  # If data depends on utils
)
