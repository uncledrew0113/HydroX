# Collect all source files in this module
file(GLOB_RECURSE CORE_SOURCES "*.cc" "*.cpp")
file(GLOB_RECURSE CORE_HEADERS "*.h" "*.hpp")

# Create library
add_library(${PROJECT_NAME}_core STATIC ${CORE_SOURCES} ${CORE_HEADERS})

# Set include directories
target_include_directories(${PROJECT_NAME}_core
    PUBLIC
        # Public headers from include directory
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        # Private headers (internal to this module)
        ${CMAKE_CURRENT_SOURCE_DIR}
)

# Link dependencies
target_link_libraries(${PROJECT_NAME}_core
    PUBLIC
        # Dependencies that are part of the public interface
        spdlog::spdlog
    PRIVATE
        # Internal dependencies
        nlohmann_json::nlohmann_json
)
