find_package(OpenGL)
if (NOT OPENGL_FOUND)
    return()
endif()

if (CMAKE_SYSTEM_NAME STREQUAL Linux)
    find_package(X11 REQUIRED)
    if (NOT X11_Xi_FOUND)
        message(FATAL_ERROR "X11 Xi library is required")
    endif ()
endif ()

if (NOT TARGET glad)
    CPMAddPackage(NAME glad GIT_REPOSITORY https://github.com/Dav1dde/glad GIT_TAG df8e9e16110b305479a875399cee13daa0ccadd9
        VERSION 0.1.33)
endif()

if (NOT TARGET glfw)
    CPMAddPackage(NAME glfw GIT_REPOSITORY https://github.com/glfw/glfw VERSION 3.3.2 GIT_TAG 3.3.2 OPTIONS "GLFW_BUILD_DOCS OFF" "GLFW_BUILD_EXAMPLES OFF" "GLFW_BUILD_TESTS OFF" "GLFW_INSTALL OFF")
endif()

CPMAddPackage(NAME imgui GIT_REPOSITORY https://github.com/ocornut/imgui VERSION 1.77 DOWNLOAD_ONLY TRUE)
set(IMGUI_INCLUDE_DIR ${imgui_SOURCE_DIR}/)
file(GLOB IMGUI_SOURCES ${imgui_SOURCE_DIR}/*.cpp)
file(GLOB IMGUI_HEADERS ${imgui_SOURCE_DIR}/*.h)
add_library(imgui STATIC ${IMGUI_SOURCES} ${IMGUI_SOURCES} ${imgui_SOURCE_DIR}/examples/imgui_impl_glfw.cpp ${imgui_SOURCE_DIR}/examples/imgui_impl_opengl3.cpp)
add_definitions(-DIMGUI_IMPL_OPENGL_LOADER_GLAD)
target_include_directories(imgui PUBLIC ${IMGUI_INCLUDE_DIR} ${OPENGL_INCLUDE_DIR} ${GLFW_INCLUDE_DIR} ${GLAD_INCLUDE_DIR})
target_link_libraries(imgui ${OPENGL_LIBRARIES} glfw glad)

add_executable(imgui_hello imgui_hello.cpp)
target_link_libraries(imgui_hello PUBLIC imgui)