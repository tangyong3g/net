## Window VS2019 配置openGL开发环境(GLFW+GlAD)   


1.    **准备需要的资源**
2.    **在VS中创建新项目**
3.    **在VS中配置包的目录和库引用和配置链接器附加依赖**
4.    **编写代码/把glad.c加入源码** 
5.    **运行**



#### 1. 准备需要的资源

 * [VS2019](https://visualstudio.microsoft.com/zh-hans/downloads/)
     * 安装VS2019即可


 * [GLFW](https://www.glfw.org/)
     *  **这里我们尽量选择Win32版本，目前看来更加的稳定（不管你是32位还是64位** ）
      ![c767435996c5d1fbe80609f0f21b3514.png](en-resource://database/7507:1)

 * [GLAD](https://glad.dav1d.de/)
    ![0b4033d766a4d1524a5b52c1e747eb65.png](en-resource://database/7503:1)
  
      * 解压可以看到include/Src 其中src中有glad.c include/glad include/KHR
      ![3f6a7c3786df47dca9a918ac81abc3bd.png](en-resource://database/7509:1)

  

  



#### 2. 在VS中创建新项目

* ###### 打开VS我们创建一个新的项目选择空项目
![9179f35c1af97a15d0a27a857ef8b882.png](en-resource://database/7511:1)





#### 3. 在VS中配置包的目录和库引用和配置链接器附加依赖

* ###### 建立文件夹把第一步下载的内容放入相应的文件（方便集中引用） openGLEvn/include  openGLEvn/lib  (openGlEvn)
    *  openGLEvn/include  中放入我们下载的glfw/include的文件夹和glad 中include文件
    *  openGLEvn/lib 中 把glfw中lib包拷贝过来 放入其中
    ![54216912851ca28ed740e6577f08dc62.png](en-resource://database/7513:1)
   
    ![cfb0fd58fe0f73e4c1fd98db35fa9830.png](en-resource://database/7515:1)
    
* ######  配置包含目录 右击项目--> 属性 --> VC++ 目录 --> 包含目录
     ![ae38ea599c32dcecda22a84c57a5fd45.png](en-resource://database/7517:1)

    ![b49b6f9b9cb32a9fcc8b6faf44d59644.png](en-resource://database/7519:1)


* ######  配置库目录（lib所在的目录）

    ![5451800bffbea768f5300e247b4c60c3.png](en-resource://database/7521:1)

    ![407d046124548b9893f4436c34b3cadc.png](en-resource://database/7523:2)


* ###### 配置连接器的输入  右击项目 --> 属性 --> 链接器 --> 输入 --> 附加依赖项  把刚才的lib 加入链接
    ![dbbb38c7ed7933e2c083a27d0ebde105.png](en-resource://database/7525:1)
     ![407d046124548b9893f4436c34b3cadc.png](en-resource://database/7523:2)




#### 4. 编写代码/把glad.c加入源码


* ###### 把glad.c加入到源码中 右击源文件 --> 添加现有项目 -->选择 glad.c 

    ![8445ba141d5acef31802e8e5b9320876.png](en-resource://database/7529:1)


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



#### 5. 运行


*  ######   Ctrl + F5   最后的结果如下，表示运行正常

    ![0d235616d8253f2d10ea5b55a275ffff.png](en-resource://database/7531:1)









  