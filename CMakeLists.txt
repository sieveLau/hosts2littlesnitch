cmake_minimum_required(VERSION 3.15)

project(hosts2littlesnitch)

set(CMAKE_CXX_STANDARD 20)

ADD_SUBDIRECTORY(ext/curlcpp)
INCLUDE_DIRECTORIES(ext/curlcpp/include)
ADD_SUBDIRECTORY(ext/json)
INCLUDE_DIRECTORIES(ext/json/include)

add_library(downloader downloader.hpp downloader.cpp)
target_link_libraries(downloader curlcpp::curlcpp)

add_executable(hosts2ls main.cpp)

target_link_libraries(hosts2ls nlohmann_json::nlohmann_json downloader)

add_executable(hosts2dset one_per_line.cpp)

target_link_libraries(hosts2dset downloader)

file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/links.txt
     DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/keys_to_remove.txt
     DESTINATION ${CMAKE_CURRENT_BINARY_DIR})