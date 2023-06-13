cmake_minimum_required(VERSION 3.17.0)
# cmake_policy(SET CMP0048 NEW)

project(micro-service)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# Do not enable compiler specific extensions, for eg on GCC use -std=c++1z (=c++17) and not -std=gnu++17
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(fmt REQUIRED)
find_package(Boost REQUIRED)
find_package(cpprestsdk REQUIRED)


# Configure compiler options ...
# if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
# 
#     message("-- configuring clang options")
#     set(CMAKE_CXX_FLAGS "-arch x86_64 -std=c++11 -stdlib=libc++ -DBOOST_LOG_DYN_LINK -Wno-deprecated-declarations")
# 
# elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
# 
#     message("-- configuring gcc options")
# 
#     set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -DBOOST_LOG_DYN_LINK")
# 
# endif()

# Project construction ...

# source files ...
add_executable(${PROJECT_NAME}
  ./source/main.cpp
  ./source/microsvc_controller.cpp
  ./source/foundation/network_utils.cpp
  ./source/foundation/basic_controller.cpp
)

target_include_directories(${PROJECT_NAME} PRIVATE
  source/foundation/include
)

target_link_libraries(${PROJECT_NAME} PRIVATE
  fmt::fmt
  cpprestsdk::cpprestsdk
)

if (APPLE)
    target_link_libraries(${PROJECT_NAME} "-framework CoreFoundation")
    target_link_libraries(${PROJECT_NAME} "-framework Security")
    set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS "-W1, -F/Library/Frameworks")
endif()
