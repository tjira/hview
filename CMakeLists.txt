cmake_minimum_required(VERSION 3.16)
set(CMAKE_CXX_STANDARD 20)
include(FetchContent)
project(Hazel)

# if cmake version is greater than 3.24 set the policies
if (CMAKE_VERSION VERSION_GREATER_EQUAL "3.24")
    cmake_policy(SET CMP0135 NEW)
endif()

# set output flags
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# set compiler flags
set(CMAKE_CXX_FLAGS_DEBUG "-g -pg -Og -flarge-source-files -fprofile-arcs -ftest-coverage -Wall -Wextra")
set(CMAKE_CXX_FLAGS_RELEASE "-s -O3")

# static link if on Windows
if (WIN32)
    string(APPEND CMAKE_CXX_FLAGS_RELEASE " -static")
    string(APPEND CMAKE_CXX_FLAGS_DEBUG " -static")
endif()

# set glfw cmake flags
set(GLFW_BUILD_EXAMPLES OFF)
set(GLFW_BUILD_TESTS OFF)
set(GLFW_BUILD_DOCS OFF)

# declare library sources libraries
FetchContent_Declare(imdialog SYSTEM URL https://github.com/aiekick/ImGuiFileDialog/archive/refs/tags/v0.6.3.tar.gz)
FetchContent_Declare(argparse SYSTEM GIT_REPOSITORY https://github.com/p-ranav/argparse.git GIT_TAG v2.9)
FetchContent_Declare(implot SYSTEM GIT_REPOSITORY https://github.com/epezent/implot.git GIT_TAG v0.14)
FetchContent_Declare(imgui SYSTEM GIT_REPOSITORY https://github.com/ocornut/imgui.git GIT_TAG v1.88)
FetchContent_Declare(glad SYSTEM GIT_REPOSITORY https://github.com/Dav1dde/glad.git GIT_TAG v2.0.4)
FetchContent_Declare(glfw SYSTEM GIT_REPOSITORY https://github.com/glfw/glfw.git GIT_TAG 3.3.8)
FetchContent_Declare(glm SYSTEM GIT_REPOSITORY https://github.com/g-truc/glm GIT_TAG 0.9.9.8)
FetchContent_Declare(stb SYSTEM GIT_REPOSITORY https://github.com/nothings/stb.git)

# fetch the libraries
FetchContent_MakeAvailable(argparse glad glfw glm imdialog imgui implot stb)

# generate glad library
add_subdirectory(${glad_SOURCE_DIR}/cmake)
glad_add_library(glad API gl:core=4.2)

# create an alternate directory for glad loading (mainly used by libraries)
add_custom_command(TARGET glad POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy
    ${CMAKE_CURRENT_BINARY_DIR}/gladsources/glad/include/glad/gl.h
    ${CMAKE_CURRENT_BINARY_DIR}/gladsources/glad/include/GL/gl.h
)

# include libraries
include_directories(include ${argparse_SOURCE_DIR}/include ${imdialog_SOURCE_DIR} ${imgui_SOURCE_DIR} ${implot_SOURCE_DIR} ${stb_SOURCE_DIR})

# add hview executable
add_executable(hview src/hview.cpp 
    src/buffer.cpp
    src/geometry.cpp
    src/gui.cpp
    src/mesh.cpp
    src/trajectory.cpp
    src/shader.cpp
    src/ptable.cpp

    # imgui backends
    ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp
    ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp

    # imgui
    ${imgui_SOURCE_DIR}/imgui.cpp
    ${imgui_SOURCE_DIR}/imgui_demo.cpp
    ${imgui_SOURCE_DIR}/imgui_draw.cpp
    ${imgui_SOURCE_DIR}/imgui_tables.cpp
    ${imgui_SOURCE_DIR}/imgui_widgets.cpp

    # implot
    ${implot_SOURCE_DIR}/implot.cpp
    ${implot_SOURCE_DIR}/implot_demo.cpp
    ${implot_SOURCE_DIR}/implot_items.cpp

    # imdialog
    ${imdialog_SOURCE_DIR}/ImGuiFileDialog.cpp
)
target_link_libraries(hview glad glfw glm::glm)
if (WIN32)
    target_link_options(hview PRIVATE "-mwindows")
endif()
