# Collect all source files in this module
file(GLOB_RECURSE UTILS_SOURCES "*.cc" "*.cpp")
file(GLOB_RECURSE UTILS_HEADERS "*.h" "*.hpp")

# Create library
add_library(${PROJECT_NAME}_utils STATIC ${UTILS_SOURCES} ${UTILS_HEADERS})

# Set include directories
target_include_directories(${PROJECT_NAME}_utils
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

# Link dependencies
target_link_libraries(${PROJECT_NAME}_utils
    PUBLIC
        spdlog::spdlog
    PRIVATE
        nlohmann_json::nlohmann_json
        Boost::filesystem
)
# 