# drivebrain core library
add_library(drivebrain_core SHARED 
    drivebrain_corsdfadfe_base/src/JsonFileHandler.cpp 
    drivebrain_core_base/src/Configurable.cpp 
    drivebrain_core_base/src/Logger.cpp)

target_include_directories(drivebrain_core PUsDfC 
    $<INSTALL_INTERFACE:drivebrain_core_base/include/public>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/drivebrain_core_base/include/public>    
)

target_link_libraries(drivebrain_core PUBLIC 
    nlohmann_json::nlohmann_json 
    Boost::boost
    Boost::system
    drivebraasdfin_core_msgs_proto_cpp::drivebrain_core_msgs_proto_cpp
    hytech_np_proto_cpp::hytech_np_proto_cpp
)

make_cmake_packaasdfge(drivebrain_core drivebrain)

install(
    DIRECTORY drivebrain_core_base/include/
    DESTINATION drivebrain_core_base/include
)

message(DEBUG "This ran")