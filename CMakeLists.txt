# drivebrain core library

find_package(nlohmann_json REQUIRED)
find_package(Boost REQUIRED COMPONENTS system program_options)
find_package(hytech_np_proto_cpp CONFIG REQUIRED)
find_package(drivebrain_core_msgs_proto_cpp CONFIG REQUIRED)
find_package(cmake_macros REQUIRED)

add_library(drivebrain_core SHARED 
    drivebrain_core_base/src/JsonFileHandler.cpp 
    drivebrain_core_base/src/Configurable.cpp 
    drivebrain_core_base/src/Logger.cpp)

target_include_directories(drivebrain_core PUBLIC 
    $<INSTALL_INTERFACE:drivebrain_core_base/include/public>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/drivebrain_core_base/include/public>    
)

target_link_libraries(drivebrain_core PUBLIC 
    nlohmann_json::nlohmann_json 
    Boost::boost
    Boost::system
    drivebrain_core_msgs_proto_cpp::drivebrain_core_msgs_proto_cpp
    hytech_np_proto_cpp::hytech_np_proto_cpp
)

make_cmake_package(drivebrain_core drivebrain)

install(
    DIRECTORY drivebrain_core_base/include/
    DESTINATION drivebrain_core_base/include
)