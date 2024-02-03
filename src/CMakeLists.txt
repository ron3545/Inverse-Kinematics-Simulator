cmake_minimum_required(VERSION 3.11)

project(Simulator_core LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

#set(CMAKE_TOOLCHAIN_FILE "C:/DEV/vcpkg/scripts/buildsystems/vcpkg.cmake")
set(CMAKE_PREFIX_PATH "C:/DEV/vcpkg/installed/x64-windows/share")  #pakage manager for C++

include(FetchContent)
set(FETCHCONTENT_UPDATES_DISCONNECTED TRUE)


# https://github.com/ChristianPanov/lwlog/issues/19
FetchContent_Declare(lwlog
  GIT_REPOSITORY https://github.com/ChristianPanov/lwlog
  GIT_TAG aed908843a23487ee31aba25318aaa73b43d1d18
)

FetchContent_GetProperties(lwlog)
if(NOT lwlog_POPULATED)
  FetchContent_Populate(lwlog)
  add_subdirectory(${lwlog_SOURCE_DIR} ${lwlog_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()

find_package(directxtk CONFIG REQUIRED)

add_subdirectory(imgui)
add_subdirectory(Graphics)
add_subdirectory(FileDialog)

add_executable(${PROJECT_NAME} WIN32  Simulator.cpp)

target_link_libraries(${PROJECT_NAME} PUBLIC 
    imgui     
    lwlog::lwlog
    FileDialog
    
    d3d11.lib dxgi.lib dxguid.lib uuid.lib
    kernel32.lib user32.lib
    comdlg32.lib advapi32.lib shell32.lib
    ole32.lib oleaut32.lib
    Microsoft::DirectXTK
)
