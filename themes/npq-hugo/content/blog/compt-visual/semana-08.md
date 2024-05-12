---
title: "Semana 08"
date: 2024-05-12
draft: false
description: "Atividade 8 APIs gráficas"
ShowToc: true
---
# **Semana 3: Formatos de imagem**
|                   Nome                  |    RA    |
|:---------------------------------------:|:--------:|
| Ricardo Gabriel Marques dos Santos Ruiz | 10389321 |
| Gabriel Augusto Ribeiro Gomes           | 10389313 |
| Caio Cezar Oliveira Filardi do Carmo    | 10391053 |
| Lucas Toneto Pires                      | 10338193 |

# Enunciado
Faça uma pesquisa sobre APIs gráficas e elabore um texto descrevendo os resultados da pesquisa.

O seu texto deve conter:

- Uma breve descrição da API gráfica que você selecionou para a pesquisa;
- Como a pipeline é documentada pelos desenvolvedores da API;
- Quais linguagens de shading (shaders) são suportadas pela API;
- Um exemplo de código que demonstra o uso da API (pode ser um "Hello, World!" gráfico – renderizar um triângulo na tela);
- Um exemplo de código de shader suportado pela API;
- A descrição de um exemplo de aplicação que usa a API.

Lembre-se de incluir todas as referências consultadas (livros, links de artigos, vídeos, etc.) e identificar todas as pessoas do grupo.

Publique o resultado dessa atividade no blog.

# Resultado

## OpenGL
OpenGL é uma especificação que lista os contratos que podem ser feitos com a API, que, através da pipeline de renderização (feita para realizar os "desenhos" na tela), realiza as chamadas referentes para a GPU, que por sua vez contém a implementação da renderização. Uma grande vantagem do OpenGl é o fato da mesma ser multiplataforma, e portanto rodar em diversos dispositivos distintos.


## Pipeline
A pipeline do OpenGl pode ser descrita nas seguintes etapas:
1- Processamento de vértice
2- Pós processamento de vértice
3- Conversão de scan e interpolação de parâmetros primitivos (rasterização)
4- Processamento de Fragmentos
5- Processamento por amostra

### 0 - Início da Pipeline
A pipeline gráfica é iniciada quando uma operação de renderização é realizada, necessitando de um objeto de array de vértices definido corretamente e um "Program Object" ou "Program Pipeline Object" vinculado que fornece os shaders para os estágios programáveis da pipeline.
### 1- Processamento de vértice
O processamento do vértice realiza um "processamento básico" de cada vértice individual, que por sua vez recebe os parâmetros de entrada do vértice, e converte em um único vértice de saída, definido pelo próprio programa (shader).
### 2- Pós processamento de vértice
Após o shader de vértices, os vértices passam por etapas de processamento fixo, como Transform Feedback, onde os dados podem ser escritos em buffer objects para uso posterior.
exemplos de etapas de processamento fixos:
Transform Feedback, Clipping, Rasterização e Per-Sample Operations
## 3- Conversão de scan e interpolação de parâmetros primitivos (rasterização)
As primitivas são convertidas em uma sequência de fragmentos, que são conjuntos de estado usados para calcular os dados finais para um pixel no framebuffer de saída.
Esse conjunto de dados é computado pela interpolação dos valores de dados nos vértices para o framento, onde o estilo da interpolação é definido pelo shader que produziu esse output.
## 4- Processamento de Fragmentos:
Cada fragmento é processado por um Fragment Shader, que determina as cores, valores de profundidade e estêncil.
Fragmentos de shaders, por sua vez são opcionais. Caso seja renderizado sem eles, a profundidade e estêncil assumem por sua vez seus valores padrões
## 5- Processamento por amostra
Os dados dos fragmentos passam por uma série de testes de culling e operações de mistura antes de serem escritos no framebuffer.

## Quais linguagens de shading (shaders) são suportadas pela API;
A principal linguagem de shader suportada pelo OpenGl é a OpenGL Shading Language

A GLSL é uma linguagem de estilo C que compartilha o modelo de depreciação do OpenGL. Ela possui um modelo de compilação único, supervisionado por vários tipos de objetos que não seguem o paradigma padrão de Objetos OpenGL. Devido a esse modelo de compilação único, a GLSL utiliza uma terminologia própria. Por exemplo, um shader é apenas um conjunto compilado de strings para um estágio programável específico e pode não conter o código completo para esse estágio. Um programa é um programa totalmente vinculado que cobre múltiplos estágios programáveis.


## Um exemplo de código que demonstra o uso da API (pode ser um "Hello, World!" gráfico – renderizar um triângulo na tela)
```c++
int main()
{
    GLFWindow* window;
    if (!glfwInit())
        return -1;

    window = glfwCreateWindow(680, 480, "Hello World!", NULL, NULL);
    if (!window)
    {
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);

    if (glewInit != GLEW_OK)
        std::cout << "ERROR!" << std::endl;

    // Loop até o usuário fechar a janela
    while (!glfwWindowShouldClose(window))
    {
        glClear(GL_COLOR_BUFFER_BIT);
        glBegin(GL_TRIANGLES);
        
        glVertex2f(-0.5f, -0.5f);
        glVertex2f(0.0f, 0.5f);
        glVertex2f(0.5f, 0.5f);

        glEnd();
        glfwPollEvents();
    } 
}
```
## Um exemplo de código de shader suportado pela API
```c++

#ifndef SHADER_H
#define SHADER_H

#include <glad/glad.h> // include glad to get all the required OpenGL headers
  
#include <string>
#include <fstream>
#include <sstream>
#include <iostream>
  

class Shader
{
public:
    // the program ID
    unsigned int ID;
  
    // constructor reads and builds the shader
    Shader(const char* vertexPath, const char* fragmentPath);
    // use/activate the shader
    void use();
    // utility uniform functions
    void setBool(const std::string &name, bool value) const;  
    void setInt(const std::string &name, int value) const;   
    void setFloat(const std::string &name, float value) const;
};

while(!glfwWindowShouldClose(window))
{
    // input
    processInput(window);

    // render
    // clear the colorbuffer
    glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
    glClear(GL_COLOR_BUFFER_BIT);

    // be sure to activate the shader
    glUseProgram(shaderProgram);
  
    // update the uniform color
    float timeValue = glfwGetTime();
    float greenValue = sin(timeValue) / 2.0f + 0.5f;
    int vertexColorLocation = glGetUniformLocation(shaderProgram, "ourColor");
    glUniform4f(vertexColorLocation, 0.0f, greenValue, 0.0f, 1.0f);

    // now render the triangle
    glBindVertexArray(VAO);
    glDrawArrays(GL_TRIANGLES, 0, 3);
  
    // swap buffers and poll IO events
    glfwSwapBuffers(window);
    glfwPollEvents();
}
```

## A descrição de um exemplo de aplicação que usa a API.
Exemplo: Unity
A unity é uma game engine, que dentre das apis gráficas suportadas, cabe citar o open gl
A Unity é uma plataforma de desenvolvimento de jogos que permite a criação de jogos em 2D e 3D para várias plataformas, incluindo PC, consoles, dispositivos móveis e web. Lançada em 2005, a Unity se tornou uma das engines de jogos mais populares no mundo, conhecida por sua facilidade de uso e flexibilidade.

Ela oferece um editor visual e a possibilidade de programação através de scripting, com ferramentas profissionais que atendem aos requisitos de qualquer tipo de jogo. Além disso, a Unity é utilizada em indústrias fora do desenvolvimento de jogos, como cinema, automotiva, arquitetura, engenharia, construção e até pelas Forças Armadas dos Estados Unidos.

Com a Unity, desenvolvedores podem criar e crescer jogos em tempo real 3D, aplicativos e experiências para entretenimento, cinema, automotivo, arquitetura e muito mais. A Unity também é conhecida por suportar uma ampla gama de plataformas, o que a torna uma escolha versátil para desenvolvedores em todo o mundo.

### Referências
- https://www.khronos.org/opengl/wiki/Rendering_Pipeline_Overview
- https://www.youtube.com/watch?v=W3gAzLwfIP0
- https://www.khronos.org/opengl/wiki/OpenGL_Shading_Language
- https://learnopengl.com/Getting-started/Shaders
- https://www.youtube.com/watch?v=71BLZwRGUJE&list=PLlrATfBNZ98foTJPJ_Ev03o2oq3-GGOS2&index=7
- https://unity.com/pt
- https://docs.unity3d.com/Manual/OpenGLCoreDetails.html