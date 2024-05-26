---
title: "Semana 09"
date: 2024-05-26
draft: false
description: "Atividade 9 OpenGl"
ShowToc: true
---
# **Atividade 9 (Transformações)**
Analise o exemplo de código C que usa SDL, OpenGL e GLSL para renderizar um triângulo, disponível em: https://github.com/profkishimoto/HelloGL.

O que deve ser alterado no código original para que o programa exiba um cubo de cores que fica girando em um ou mais eixos sem parar?

No "cubo de cores", a posição (x, y, z) de um vértice é mapeada para uma cor (r, g, b). Assim, um vértice na posição xyz(0, 0, 0) tem a cor preta rgb(0, 0, 0), um vértice na posição xyz(1, 0, 0) tem a cor vermelha rgb(1, 0, 0), e assim por diante.

Como referência do que o programa alterado deve fazer, veja o vídeo disponível no repositório acima.

Descreva as alterações que devem ser realizadas e publique o resultado dessa atividade no blog.

Lembre-se de incluir todas as referências consultadas (livros, links de artigos, vídeos, etc.) e identificar todas as pessoas do grupo.

# Resultado
## Primeiro Passo:
O blog descreve uma implementação realizada pelo grupo que sana o problema descrito (ou seja, fizemos o desafio opcional)
O que deve ser feito:
Primeiramente, devemos realizar a renderização do cubo, ao invés do triângulo. Trocando vértices e cores:
```c
    GLfloat vertices[] = {
        -1.0f,-1.0f,-1.0f, // triangle 1 : begin
        -1.0f,-1.0f, 1.0f,
        -1.0f, 1.0f, 1.0f, // triangle 1 : end

        1.0f, 1.0f,-1.0f, // triangle 2 : begin
        -1.0f,-1.0f,-1.0f,
        -1.0f, 1.0f,-1.0f, // triangle 2 : end

        1.0f,-1.0f, 1.0f,
        -1.0f,-1.0f,-1.0f,
        1.0f,-1.0f,-1.0f,

        1.0f, 1.0f,-1.0f,
        1.0f,-1.0f,-1.0f,
        -1.0f,-1.0f,-1.0f,

        -1.0f,-1.0f,-1.0f,
        -1.0f, 1.0f, 1.0f,
        -1.0f, 1.0f,-1.0f,

        1.0f,-1.0f, 1.0f,
        -1.0f,-1.0f, 1.0f,
        -1.0f,-1.0f,-1.0f,

        -1.0f, 1.0f, 1.0f,
        -1.0f,-1.0f, 1.0f,
        1.0f,-1.0f, 1.0f,

        1.0f, 1.0f, 1.0f,
        1.0f,-1.0f,-1.0f,
        1.0f, 1.0f,-1.0f,
    
        1.0f,-1.0f,-1.0f,
        1.0f, 1.0f, 1.0f,
        1.0f,-1.0f, 1.0f,

        1.0f, 1.0f, 1.0f,
        1.0f, 1.0f,-1.0f,
        -1.0f, 1.0f,-1.0f,

        1.0f, 1.0f, 1.0f,
        -1.0f, 1.0f,-1.0f,
        -1.0f, 1.0f, 1.0f,

        1.0f, 1.0f, 1.0f,
        -1.0f, 1.0f, 1.0f,
        1.0f,-1.0f, 1.0f
    };

GLfloat colors[] = {
        // 1 Relacionado com o 5 OK
        0.0f, 1.0f, 0.0f,  // Verde 
        1.0f, 0.0f, 0.0f,  // Vermelho 
        0.0f, 0.0f, 1.0f,  // Azul // test

        // 2 Relacionado com o 4 OK
        0.0f, 1.0f, 0.0f,  // Verde 
        0.0f, 0.0f, 1.0f,  // Azul
        1.0f, 0.0f, 0.0f,  // Vermelho 

        // 3 Relacionado com 6 OK
        0.0f, 1.0f, 0.0f,  // Verde 
        0.0f, 0.0f, 1.0f,  // Azul
        1.0f, 0.0f, 0.0f,  // Vermelho 

        // 4 Relacionado com o 2 OK
        0.0f, 1.0f, 0.0f,  // Verde 
        1.0f, 0.0f, 0.0f,  // Vermelho 
        0.0f, 0.0f, 1.0f,  // Azul 

        // 5 Relacionado com o 1 OK
        0.0f, 1.0f, 0.0f,  // Verde 
        0.0f, 0.0f, 1.0f,  // Azul // test
        1.0f, 0.0f, 0.0f,  // Vermelho 

        // 6 relacionado com o 3 OK
        0.0f, 1.0f, 0.0f,  // Verde 
        1.0f, 0.0f, 0.0f,  // Vermelho 
        0.0f, 0.0f, 1.0f,  // Azul // test

        // 7 relacionado com o 12 OK
        0.0f, 1.0f, 0.0f,  // Verde 
        0.0f, 0.0f, 1.0f,  // Azul
        1.0f, 0.0f, 0.0f,  // Vermelho 

        // 8 relacionado com o 9
        0.0f, 1.0f, 0.0f,  // Verde 
        0.0f, 0.0f, 1.0f,  // Azul
        1.0f, 0.0f, 0.0f,  // Vermelho 

        // 9 relacionado com o 9
        0.0f, 1.0f, 0.0f,  // Verde 
        1.0f, 0.0f, 0.0f,  // Vermelho 
        0.0f, 0.0f, 1.0f,  // Azul // test

        // 10 relacionado com o 11
        0.0f, 1.0f, 0.0f,  // Verde 
        1.0f, 0.0f, 0.0f,  // Vermelho 
        0.0f, 0.0f, 1.0f,  // Azul // test

        // 11 relacionado com o 10
        0.0f, 1.0f, 0.0f,  // Verde 
        0.0f, 0.0f, 1.0f,  // Azul
        1.0f, 0.0f, 0.0f,  // Vermelho 
        
        // 12 Relacionado com o 7
        0.0f, 1.0f, 0.0f,  // Verde 
        1.0f, 0.0f, 0.0f,  // Vermelho 
        0.0f, 0.0f, 1.0f,  // Azul // test

    };
```

## Segundo passo: Rotacionar
A abordagem realizada pelo grupo consiste em renderizar a rotação com um incrementador para a horizontal, e em seguida, com a finalização da rotação na horizontal, realizar para a vertical.
O código de translação descrito abaixo realiza isso, e é basicamente uma soma de um "passo" na hora de realizar o glm_translate e glm_rotate.
A grosso modo, para resolver o problema basta chamar as funções de transformação já escritas anteriormente, porém com os eixos de rotação incrementados.
```c
typedef struct 
{
    double rx;
    double ry;
    double rz;
} rotation_weights;

bool rotate(
    int* steps,
    SDL_Window* window,
    GLuint VAO,
    GLuint u_MVPMatrix,
    double maxRotations,
    rotation_weights r_weights) // Peso para cada rotação. Feito para não causar repetição de código
{
    const int WINDOW_WIDTH = 800;
    const int WINDOW_HEIGHT = 600;

    if (*steps >= maxRotations) // Se a quantidade de passos for igual a 100, paramos de rotacionar
    {
        *steps = 0;
        return true;
    }
    
    vec3 cameraPos = {0.0f, 2.0f, 5.0f};
    vec3 cameraTarget = {0.0f, 0.0f, 0.0f};
    vec3 cameraUp = {0.0f, 1.0f, 0.0f};

    glClearColor(0.0f, 0.0f, 0.1f, 1.0f);
    glClear(GL_COLOR_BUFFER_BIT);

    mat4 modelMatrix, viewMatrix, projectionMatrix;

    float tx = 0.0f, ty = 0.0f, tz = 0.0f;

    //  Esse calculo maluco foi feito apenas para facilitar na chamada do método quais rotações serão feitas
    float rx = 0.0f + ((*steps) * r_weights.rx);
    float ry = 0.0f + ((*steps) * r_weights.ry);
    float rz = 0.0f + ((*steps) * r_weights.rz);

    float sx = 1.0f, sy = 1.0f, sz = 1.0f;
    glm_mat4_identity(modelMatrix);
    glm_translate(modelMatrix, (vec3){ tx, ty, tz });

    glm_rotate(modelMatrix, glm_rad(rx), (vec3){ 1.0f, 0.0f, 0.0f });
    glm_rotate(modelMatrix, glm_rad(ry), (vec3){ 0.0f, 1.0f, 0.0f });
    glm_rotate(modelMatrix, glm_rad(rz), (vec3){ 0.0f, 0.0f, 1.0f });
    glm_scale(modelMatrix, (vec3){ sx, sy, sz });
    glm_lookat(cameraPos, cameraTarget, cameraUp, viewMatrix);

    float fov = glm_rad(60.0f);
    float aspect = WINDOW_WIDTH * 1.0f / WINDOW_HEIGHT * 1.0f;
    float nearPlane = 0.1f;
    float farPlane = 100.0f;
    glm_perspective(fov, aspect, nearPlane, farPlane, projectionMatrix);
    *steps += STEP_INCREMENT_VALUE;

    mat4 MVPMatrix;
    glm_mat4_mul(projectionMatrix, viewMatrix, MVPMatrix);
    glm_mat4_mul(MVPMatrix, modelMatrix, MVPMatrix);

    // Envio da matriz MVP para o vertex shader.
    glUniformMatrix4fv(u_MVPMatrix, 1, GL_FALSE, (const GLfloat *)MVPMatrix);
    glDrawArrays(GL_TRIANGLES, 0, 12*3); // 12*3 indices starting at 0 -> 12 triangles -> 6 squares
    glBindVertexArray(VAO);
    SDL_GL_SwapWindow(window);

    if ((*steps) % 100 == 1) // Feito apenas por fins de debuggings preguiçosos
        printf("%d\n", *steps);
    return false;
}
```
A função rotate() descrita acima representa uma chamada flexivel, que de acordo com o peso passado, incrementará a posição do eixo desejado.
Por conta disso, cada frame renderizado deverá chamar a função rotate para os seus pesos desejados de rx, ry, rz. Onde o step é simplesmente um contador que aumenta conforme o cubo é rotacionado.
As funções que realizam isso são descritas da seguinte forma:
```c
bool rotate_horizontal(int* steps, SDL_Window* window, GLuint VAO, GLuint u_MVPMatrix)
{
    rotation_weights r_weights = {rx: 0.0, ry: 1, rz: 0.0};
    return rotate(steps, window, VAO, u_MVPMatrix, MAX_STEPS_FOR_RY_ROTATION, r_weights);
}
bool rotate_vertical(int* steps, SDL_Window* window, GLuint VAO, GLuint u_MVPMatrix)
{
    rotation_weights r_weights = {rx: 1.0, ry: 0.0, rz: 0.0};
    return rotate(steps, window, VAO, u_MVPMatrix, MAX_STEPS_FOR_RY_ROTATION, r_weights);
}
```
Dito isso, chamamos ambas durante o while do código principal, fazendo que a rotação horizontal seja feita primeiro, depois a vertical:
```c
while (isRunning)
    {
        while (SDL_PollEvent(&event))
        {
            if (event.type == SDL_QUIT)
            {
                isRunning = false;
                break;
            }
        }
        // Rotacao Horizontal
        if (!finished_horizontal)
        {
            finished_horizontal = rotate_horizontal(&steps, window, VAO, u_MVPMatrix);
        }

        // Rotacao Vertical
        if (finished_horizontal)
        {
            finished_vertical = rotate_vertical(&steps, window, VAO, u_MVPMatrix);
        }

        if (finished_horizontal && finished_vertical)
        {
            finished_horizontal = false;
            finished_vertical = false;
        }

        // Limpa o buffer (tela) com a cor preta.
        glClearColor(0.0f, 0.0f, 0.1f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        // Renderização do triângulo.
        glDrawArrays(GL_TRIANGLES, 0, 12*3); // 12*3 indices starting at 0 -> 12 triangles -> 6 squares
        
        // Atualização da janela.
        SDL_GL_SwapWindow(window);
    }
```
O código acima faz com que a rotação vertical só aconteca caso a horizontal acabe e vice versa. Após o término das ambas, os contadores são resetados.

## Resultado:
![Rotacao](https://raw.githubusercontent.com/gabrielms201/computacao-visual/master/themes/npq-hugo/images/Rotacao.gif)

O código da implementação acima pode ser encontrada no github, no seguinte fork do repositório: (https://github.com/gabrielms201/computacao-visual-atividade-cubo)





# Referencias
- https://www.youtube.com/watch?v=LxEFn-cGdE0
- https://github.com/nigels-com/glew
- https://github.com/recp/cglm
- https://launchpad.net/ubuntu/+source/cglm
- http://www.opengl-tutorial.org/beginners-tutorials/tutorial-4-a-colored-cube/
- https://github.com/lszl84/wx_gl_cube_tutorial/blob/main/src/cube.h
- https://github.com/c2d7fa/opengl-cube
- Rotação: Tentativa e erro e conteúdo das aulas