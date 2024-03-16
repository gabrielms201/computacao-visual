---
title: "Semana 03"
date: 2024-03-16
draft: false
description: "Atividade 3 (Formatos de imagem) "
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
Atividade em grupo de até 4 pessoas.

Nessa semana, vimos que os processos de amostragem e quantização são realizados para converter dados contínuos (capturados por sensores de aquisição de imagens) em dados discretos, resultando na geração de imagens digitais.

A discretização converte uma função contínua f(s, t) em uma matriz bidimensional f(x, y) com MxN elementos, sendo:

- M: Número de linhas da imagem.
- N: Número de colunas da imagem.
- (x, y): Coordenadas discretas.
- x = 0, 1, 2, …, M – 1.
- y = 0, 1, 2, …, N – 1.
Cada elemento da matriz bidimensional (e da imagem digital) corresponde a um pixel.

No entanto, quando trabalhamos com arquivos de imagem (isto é, o conteúdo desses arquivos é interpretado como a representação de uma imagem digital), eles raramente armazenam apenas as informações dos pixels que formam a imagem.

Sabendo disso, escolha um formato de arquivo de imagem e descreva como o conteúdo do arquivo é organizado. Na descrição, detalhe as principais seções do formato de arquivo de imagem escolhido.

Lembre-se de incluir todas as referências consultadas (livros, links de artigos, vídeos, etc.) e identificar todas as pessoas do grupo.

Publique o resultado dessa atividade no blog.

Dica:
O livro “Encyclopedia of Graphics File Formats” (Murray e vanRyper, 1996) é uma ótima referência e está disponível online sob licença Creative Commons.

# Resultado

## Visão geral
O formato de imagem escolhido foi o Bitmap. Escolhemos esse formato de se tratar do formato de imagem mais primitivo, e por conta disso, acreditamos que seja o melhor formato a ser debatido para desenvolvermos uma base sobre o tema.

Podemos resumir o bitmap como um formato de arquivo composto em: headers (cabeçalhos), dados e demais informações (como por exemplo palhetas de cor e outros tipos de dados).
Podemos apresentar a estrutura do arquivo de diversas maneiras, dentre elas:

Em casos de apenas dados e informações de rodapé:
| Arquivo.bmp      |
|----------------  |
| Cabeçalho        |
| Dados            |
| Rodapé           |

Em casos de apenas dados e palhetas de cor, que por sua vez podem aparecer antes ou depois dos dados da imagem:
- Arquivo.bmp
  - Cabeçalho
    - Palheta
  - Dados
-   Rodapé (ou, opcionalmente, Palheta)

Ou também dentro ou fora do header:

1. Dentro do Header:
    - Arquivo.bmp
      - \>------------------
      - Cabeçalho
          - Palheta
      - \>------------------
      - Dados
      - \>------------------
      - Rodapé

2. Fora do Header:
    - Arquivo.bmp
      - \>------------------
      - Cabeçalho
      - \>------------------
      - Palheta
      - \>------------------
      - Dados
      - \>------------------
      - Rodapé

Podemos também apresentar dados de correção de cor. Também antes ou depois dos dados da imagem:

- Arquivo.bmp
  - \>------------------
  - Cabeçalho
    - Palheta
  - \>------------------
  - Tabela de correção de cores **(opcional)**
  - \>------------------
  - Dados
  - \>------------------
  - Tabela de correção de cores **(opcional)**
  - \>------------------
  - Rodapé

Um fato interessante desse formato é que, se um arquivo de imagem for capaz de armazenar diversas imagens, teríamos um index, e mais de uma "sessão" para dados:

- Arquivo.bmp
  - \>------------------
  - Cabeçalho
    - Index
  - \>------------------
  - Palheta 1
  - Dados da imagem 1
  - \>------------------
  - Palheta 2
  - Dados da imagem 2
  - \>------------------
  - Palheta 3
  - Dados da imagem 3
  - \>------------------
  - ...
  - \>------------------
  - Palheta n
  - Dados da imagem n
  - \>------------------
  - Rodapé

Agora que a estrutura geral do formato bitmap foi apresentado, vamos nos adentrar em cada uma dessas "sessões" apresentadas nesse formato de arquivo:

## Header
Um header (ou cabeçalho) é nada mais nada menos do que uma sessão combinada de dados binários ou [ASCII](https://pt.wikipedia.org/wiki/ASCII) apresentado no começo do arquivo, onde tais dados são compostos de campos fixos (porém nem todos necessários)
Podemos dizer que todos os arquivos BITMAP possuem algum header, podendo variar de arquivo para arquivo, e, de acordo com o que foi dito anteriormente, seus respectivos campos também podem variar. A grosso modo, podemos listar os seguintes campos mais utilizados comumente:

1. Cabeçalho
2. Palheta
3. Index
4. Palheta (1)
5. Identificador do arquivo
6. Versão do arquivo
7. Número de linhas por imagem
8. Número de bits por pixel
9. Número de planos de cores
10. Eixo X origem da imagem
11. Eixo Y origem da imagem
12. Descrição de texto
13. Espaço não utilizado

Um exemplo clássico de bit header em formato de uma estrutura de dados na linguagem de programação C pode ser descrito da seguinte maneira:
``` c
typedef struct _WinHeaderlx
{
    WORD Type; 
    WORD Width;
    WORD Height;
    WORD Width;
    BYTE Planes;
    BYTE BitsPixel;
} OLDWINHEAD;
```
## Dados de Bitmap
Após a definição do cabeçalho, podemos encontrar os dados de uma arquivo bitmap, que está presente após um **offset** definido anteriormente.

Em relação a maneira na qual os dados são escritos no arquivo, podemos simplificar do seguinte modo:
Primeiramente, os dados da imagem são acoplados em um ou mais blocos de memória.
Após a realização desse acoplamento, dois métodos de organização distintos podem ser utilizados: "Scan-Line Data" ou "Planar Data"

### Scan Line Data
O primeiro e mais simples método de organização apresentado.
Os pixels da imagem são então organizados em linhas (ou scan lines), onde caso cada imagem seja composta de uma ou mais scan line, os dados de pixel que descrevem 
a imagem serão uma série de conjuntos de valores, onde cada conjunto representaria uma linha da imagem:

- ScanLine 1: { 100000000000100000000000, 100000000000000000000000, 000000000110000000000000, ...}
- ScanLine 2: { 111111111111111111111111, 100000000011110000000000, 011111111110000000000000, ...}
- ...
- ScanLine N: { (1|0)+ }

## Planar Data
O segundo método de organização apresentado.
O método consiste na separação dos dados da imagem em um ou mais "planos".

Uma imagem composta (isso é, composta de várias cores) é então representada por três blocos de dados, onde cada bloco contém apenas uma das cores componente que formam a imagem (quebradas por filtros que realizam a separação entre conjuntos de componentes de cores). Um exemplo prático pode ser descrito da seguinte maneira.
Supomos que tenhamos uma imagem de 24 bits de dimensão 2x3 representada da seguinte maneira:

|              |       RGB    |              |
| -------------|--------------|--------------|
| (00, 01, 02) | (03, 04, 05) | (06, 07, 08) |
| (09, 10, 12) | (13, 14, 15) | (16, 17, 18) |

Podemos então dividir cada cor do RGB (RED GREEN BLUE) em um plano dividido por RGB (Red Green Blue):

| \( | [R] | [G] | [B] | \)| |\(| [R] | [G] | [B] | \)| |\(| [R] | [G] | [B] | \) |
|:--:|-----|-------|------|---|-|--|:---:|-------|------|---|-|--|:---:|-------|------|---|
|    | 00  | 01    | 02   |   | |  |  03 |  04   | 05   |   | |  | 06  |   07  |  08  |   |
|    | 09  | 10    | 11   |   | |  |  12 |  13   | 14   |   | |  | 15  |   16  |  17  |   |

Tendo a imagem acima como exemplo, a divisão de planos seria algo como o seguinte:
- Plano Vermelho [R]:
    - (00) (03) (06)
    - (09) (12) (15)
- Plano Verde [G]:
    - (01) (04) (07)
    - (10) (13) (16)
- Plano Azul [B]:
    - (02) (05) (08)
    - (11) (14) (17)

Mantendo assim, uma organização do mapa de cores seguindo a estrutura de planos

## Finalização
Bom, espero que tenham aprendido algo após essa breve introdução da forma na qual um arquivo bitmap é armazenado.
Claro que esse o formato bitmap possui muitas nuâncias que não caberiam ser descritas em um artigo só. Para os mais interessados, recomendamos a leitura do livro "Encyclopedia of Graphics File Formats, Murray e vanRyper, 1996", que explica a estrutura do formato bitmap e de diversos outros formatos de arquivo


### Referências
1. Encyclopedia of Graphics File Formats, Murray e vanRyper, 1996

