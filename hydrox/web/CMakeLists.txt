# Collect all source files in this module
file(GLOB_RECURSE WEB_SOURCES "*.cc" "*.cpp")
file(GLOB_RECURSE WEB_HEADERS "*.h" "*. hpp")

# Create library
add_library(${PROJECT_NAME}_web STATIC ${WEB_SOURCES} ${WEB_HEADERS})

# Set include directories
target_include_directories(${PROJECT_NAME}_web
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

# Link dependencies
target_link_libraries(${PROJECT_NAME}_web
    PUBLIC
        spdlog::spdlog
        Boost::filesystem  # Assuming Boost. Beast or Boost.Asio for web
        PahoMqttCpp::paho-mqttpp3-static
    PRIVATE
        nlohmann_json::nlohmann_json
        ${PROJECT_NAME}_utils  # If web depends on utils
)

