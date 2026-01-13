# Collect all source files in this module
file(GLOB_RECURSE MODEL_SOURCES "*.cc" "*.cpp")
file(GLOB_RECURSE MODEL_HEADERS "*.h" "*.hpp")

# Create library
add_library(${PROJECT_NAME}_model STATIC ${MODEL_SOURCES} ${MODEL_HEADERS})

# Set include directories
target_include_directories(${PROJECT_NAME}_model
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

# Link dependencies
target_link_libraries(${PROJECT_NAME}_model
    PUBLIC
        spdlog::spdlog
        nlohmann_json::nlohmann_json
    PRIVATE
        ${PROJECT_NAME}_utils  # If model depends on utils
)

