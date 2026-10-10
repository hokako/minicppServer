cmake_minimum_required(VERSION 3.16)

project(cppServer VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(Threads REQUIRED)

add_library(cppserver STATIC
    base/currentThread.cpp
    tcp/Acceptor.cpp
    tcp/Buffer.cpp
    tcp/Channel.cpp
    tcp/Epoll.cpp
    tcp/EventLoop.cpp
    tcp/EventLoopThread.cpp
    tcp/EventLoopThreadPool.cpp
    tcp/TcpConnection.cpp
    tcp/TcpServer.cpp
    http/HttpContext.cpp
    http/HttpRequest.cpp
    http/HttpResponse.cpp
    http/HttpServer.cpp
)

target_include_directories(cppserver
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/base
        ${CMAKE_CURRENT_SOURCE_DIR}/tcp
        ${CMAKE_CURRENT_SOURCE_DIR}/http
)

target_link_libraries(cppserver PUBLIC Threads::Threads)

add_executable(echo_server test/echo_server.cpp)
target_link_libraries(echo_server PRIVATE cppserver)

add_executable(http_server test/http_server.cpp)
target_link_libraries(http_server PRIVATE cppserver)

add_executable(test_httpcontext test/test_httpcontext.cpp)
target_link_libraries(test_httpcontext PRIVATE cppserver)

include(CTest)
if(BUILD_TESTING)
    add_test(NAME httpcontext_parser COMMAND test_httpcontext)
endif()
