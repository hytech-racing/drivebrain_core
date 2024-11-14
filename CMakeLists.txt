# drivebrain core library
cmake_minimum_required(VERSION 3.24)
project(drivebrain_core LANGUAGES CXX C)

find_package(nlohmann_json REQUIRED)
find_package(Boost REQUIRED COMPONENTS system program_options)
find_package(hytech_np_proto_cpp CONFIG REQUIRED)
find_package(drivebrain_core_msgs_proto_cpp CONFIG REQUIRED)
find_package(cmake_macros REQUIRED)
find_package(spdlog REQUIRED)
find_package(fmt REQUIRED)

add_library(drivebrain_core SHARED 
    src/JsonFileHandler.cpp 
    src/Configurable.cpp 
    src/Logger.cpp)

target_include_directories(drivebrain_core PUBLIC 
    $<INSTALL_INTERFACE:include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>    
)

target_link_libraries(drivebrain_core PUBLIC 
    nlohmann_json::nlohmann_json 
    Boost::boost
    Boost::system
    drivebrain_core_msgs_proto_cpp::drivebrain_core_msgs_proto_cpp
    hytech_np_proto_cpp::hytech_np_proto_cpp
)
include(create_package)

make_cmake_package(drivebrain_core drivebrain_core)

install(
    DIRECTORY include/
    DESTINATION ${CMAKE_INSTALL_PREFIX}/include
)a