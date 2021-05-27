cmake_minimum_required(VERSION 3.15)
project(learnOpenGLGlfwglad)

set(CMAKE_CXX_STANDARD 11)

# 如果安装多个版本glfw，这里可以指定版本号[version]
# find_package(glfw3 3.3 REQUIRED)
find_package(glfw3 REQUIRED)
#　添加源文件
set(SOURCE_FILES main.cpp glad.c)
#编译
add_executable(learnOpenGLGlfwglad ${SOURCE_FILES})
#指定了要连接的库文件
target_link_libraries(learnOpenGLGlfwglad glfw3 GL -lGL -lm -ldl -lXinerama -lXrandr -lXi -lXcursor -lX11 -lXxf86vm -lpthread)