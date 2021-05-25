## Mac Xcode 配置openGL开发环境 (glfw+glad)

1.  **准备需要的资源** 
2.  **在Xcode中创建新项目**
3.  **在xCode项目中配置头文件/lib** 
4.  **在xCode项目中配置连接库**
5.  **编写代码加入glad.c**




#### 1.  准备需要的资源

>  **GLFW** 是一个用于台式机上的OpenGL，OpenGL ES和Vulkan开发的开源，多平台库。 它提供了一个简单的API，用于创建窗口，上下文和表面，接收输入和事件。GLFW用C编写，并支持Windows，macOS，X11和Wayland。


>  **glad** 与glew作用类似，实现对底层OpenGL接口封装,详细说明见 https://blog.csdn.net/HHT0506/article/details/108919621 



*  ###### [GLFW](https://www.glfw.org/)
    *  ###### **使用源码编译GLFW** 
    +  **下载glfw源码**  
         
         > git clone --recursive git@github.com:glfw/glfw.git
    +  **编译** 
         > cd <glfw-root-dir>
         > camke .
         > makedir glfw-build
         > cd glfw-build 
         > cmake <glfw-root-dir>
    + **install glfw** 
        > cd glfw-build 
        > make install 安装后会生成libglfw3.a GLFW/头文件 （**也可可以手动拷贝过去**）
        > ![cb4111d802736d7267224a68020583a1.jpeg](en-resource://database/7551:1)
        
        
    
        
    
*  ######  [GLAD](https://glad.dav1d.de/) 
    *  ##### **这里我们需要glad.h glad.c 文件**
    *  ![b3dfa1a96acfe41271d7631c1a71c226.jpeg](en-resource://database/7552:1)




#### 2.  在Xcode中创建新项目

+ ######  **如下图我们选择Command Line Tool ** 
+ ![8c813b785767cb636b8bbd6c54ef7369.jpeg](en-resource://database/7550:1)




#### 3.  在xCode项目中配置头文件/lib

![2c0e094ad237efda57a214f46c3f94ab.jpeg](en-resource://database/7553:1)



#### 4.  在xCode项目中配置连接库


![42940190b83fb2d74a1e12a8ee1f8e50.jpeg](en-resource://database/7554:1)


#### 5.  编写代码加入glad.c


 ``` C++


#include <glad/glad.h>
#define GLFW_INCLUDE_NONE
#include <GLFW/glfw3.h>

#include "linmath.h"

#include <stdlib.h>
#include <stdio.h>

static const struct
{
   float x, y;
   float r, g, b;
} vertices[3] =
{
   { -0.6f, -0.4f, 1.f, 0.f, 0.f },
   {  0.6f, -0.4f, 0.f, 1.f, 0.f },
   {   0.f,  0.6f, 0.f, 0.f, 1.f }
};

static const char* vertex_shader_text =
"#version 110\n"
"uniform mat4 MVP;\n"
"attribute vec3 vCol;\n"
"attribute vec2 vPos;\n"
"varying vec3 color;\n"
"void main()\n"
"{\n"
"    gl_Position = MVP * vec4(vPos, 0.0, 1.0);\n"
"    color = vCol;\n"
"}\n";

static const char* fragment_shader_text =
"#version 110\n"
"varying vec3 color;\n"
"void main()\n"
"{\n"
"    gl_FragColor = vec4(color, 1.0);\n"
"}\n";

static void error_callback(int error, const char* description)
{
   fprintf(stderr, "Error: %s\n", description);
}

static void key_callback(GLFWwindow* window, int key, int scancode, int action, int mods)
{
   if (key == GLFW_KEY_ESCAPE && action == GLFW_PRESS)
       glfwSetWindowShouldClose(window, GLFW_TRUE);
}

int main(void)
{
   GLFWwindow* window;
   GLuint vertex_buffer, vertex_shader, fragment_shader, program;
   GLint mvp_location, vpos_location, vcol_location;

   glfwSetErrorCallback(error_callback);

   if (!glfwInit())
       exit(EXIT_FAILURE);

   glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 2);
   glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 0);

   window = glfwCreateWindow(640, 480, "Simple example", NULL, NULL);
   if (!window)
   {
       glfwTerminate();
       exit(EXIT_FAILURE);
   }

   glfwSetKeyCallback(window, key_callback);

   glfwMakeContextCurrent(window);
    if(!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)){
        return -1;
    }
   glfwSwapInterval(1);

   // NOTE: OpenGL error checks have been omitted for brevity

   glGenBuffers(1, &vertex_buffer);
   glBindBuffer(GL_ARRAY_BUFFER, vertex_buffer);
   glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

   vertex_shader = glCreateShader(GL_VERTEX_SHADER);
   glShaderSource(vertex_shader, 1, &vertex_shader_text, NULL);
   glCompileShader(vertex_shader);

   fragment_shader = glCreateShader(GL_FRAGMENT_SHADER);
   glShaderSource(fragment_shader, 1, &fragment_shader_text, NULL);
   glCompileShader(fragment_shader);

   program = glCreateProgram();
   glAttachShader(program, vertex_shader);
   glAttachShader(program, fragment_shader);
   glLinkProgram(program);

   mvp_location = glGetUniformLocation(program, "MVP");
   vpos_location = glGetAttribLocation(program, "vPos");
   vcol_location = glGetAttribLocation(program, "vCol");

   glEnableVertexAttribArray(vpos_location);
   glVertexAttribPointer(vpos_location, 2, GL_FLOAT, GL_FALSE,
                         sizeof(vertices[0]), (void*) 0);
   glEnableVertexAttribArray(vcol_location);
   glVertexAttribPointer(vcol_location, 3, GL_FLOAT, GL_FALSE,
                         sizeof(vertices[0]), (void*) (sizeof(float) * 2));

   while (!glfwWindowShouldClose(window))
   {
       float ratio;
       int width, height;
       mat4x4 m, p, mvp;

       glfwGetFramebufferSize(window, &width, &height);
       ratio = width / (float) height;

       glViewport(0, 0, width, height);
       glClear(GL_COLOR_BUFFER_BIT);

       mat4x4_identity(m);
       mat4x4_rotate_Z(m, m, (float) glfwGetTime());
       mat4x4_ortho(p, -ratio, ratio, -1.f, 1.f, 1.f, -1.f);
       mat4x4_mul(mvp, p, m);

       glUseProgram(program);
       glUniformMatrix4fv(mvp_location, 1, GL_FALSE, (const GLfloat*) mvp);
       glDrawArrays(GL_TRIANGLES, 0, 3);

       glfwSwapBuffers(window);
       glfwPollEvents();
   }

   glfwDestroyWindow(window);

   glfwTerminate();
   exit(EXIT_SUCCESS);
}


 ```




#### 6. 运行结果


![ebfd9af05837e037b5a7d9133961fa4f.jpeg](en-resource://database/7555:1)





**参考资料**

https://www.glfw.org/documentation.html