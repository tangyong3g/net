## Ubuntu vscode 配置openGL开发环境(GLFW+GlAD)   


1.    **准备需要的资源**
2.    **配置项目目录和相应的文件**
3.    **编写代码/把glad.c加入源码** 
4.    **使用cmake/make编译链接程序**
5.    **结果**



#### 1. 准备需要的资源

 * [VSCODE](https://code.visualstudio.com/)


 * [GLFW](https://www.glfw.org/)
   >  这里我们也可以使用命令安装
   >  sudo apt-get install libglfw3-dev


 * [GLAD](https://glad.dav1d.de/)
    ![0b4033d766a4d1524a5b52c1e747eb65.png](en-resource://database/7588:1)

  > 下在文件解压移动到系统库文件中
  > cd glad/include
  > sudo mv glad/ /usr/local/include/
  > sudo mv KHR/ /usr/local/include


​     
​     

  



#### 2. 在VS中创建新项目

+ 创建项目文件 mkdir projectName
+ 创建源文件  main.cpp 
+ 把glad.c加入到源文件 glad.c
+ 创建      CmakeList.txt 

``` Cmake
    cmake_minimum_required(VERSION 3.15)
    project(learnOpenGLGlfwglad)
    set(CMAKE_CXX_STANDARD 11)
    # find_package(glfw3 3.3 REQUIRED)
    find_package(glfw3 REQUIRED)
    #　添加源文件
    set(SOURCE_FILES main.cpp glad.c)
    #编译
    add_executable(learnOpenGLGlfwglad ${SOURCE_FILES})
    #指定了要连接的库文件
    target_link_libraries(learnOpenGLGlfwglad glfw3 GL -lGL -lm -ldl -lXinerama -lXrandr -lXi -lXcursor -lX11 -lXxf86vm -lpthread)
```

 




#### 4. 编写代码/把glad.c加入源码


* ###### 把glad.c加入到源码中 右击源文件 --> 添加现有项目 -->选择 glad.c 

    ![8445ba141d5acef31802e8e5b9320876.png](en-resource://database/7589:1)


*    ###### 这里我们选择GLFW的官网一段代码 

        ``` C++ 
        #include <glad/glad.h>
        #include <GLFW/glfw3.h>
        int main(void){
            GLFWwindow* window;
     
            /* Initialize the library */
            if (!glfwInit())
                return -1;
     
            /* Create a windowed mode window and its OpenGL context */
            window = glfwCreateWindow(640, 480, "Hello World", NULL, NULL);
            if (!window)
            {
                glfwTerminate();
                return -1;
            }
     
            /* Make the window's context current */
            glfwMakeContextCurrent(window);
     
            /* Loop until the user closes the window */
            while (!glfwWindowShouldClose(window))
            {
                /* Render here */
                glClear(GL_COLOR_BUFFER_BIT);
     
                glClearColor(1.0,0.5f,0.0f,1.0f);


                /* Swap front and back buffers */
                glfwSwapBuffers(window);
    
                /* Poll for and process events */
                glfwPollEvents();
            }
    
            glfwTerminate();
            return 0;}
    
        ```


#### 4. 利用Cmake编译文件链接生成可执行文件


>   cd Project_root
>   cmake ./
>   make 
>   ![a000ada243ba3d89b6e1f7507fa2f289.png](en-resource://database/7590:1)











