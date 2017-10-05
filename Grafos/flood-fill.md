Flood Fill
==========

Flood Fill, ou identificação de vértices (_labeling_), ou colorir vértices
(_coloring_). É um algoritmo que a partir de um vértice de cor `X`, todos os
vértices de cor `X` que estão diretamente conectados a ele são coloridos/identificados
com a cor `Y`. Em seguinda os vizinhos deste vértices, que também possuem a cor `X`,
são coloridos. Até que se encontre algum vértice de uma cor diferente de `X`.

Este é o algoritmos utilizado no "balde de tinta" de aplicações de pintura digital.
Ele colore todos os pixels que estão em volta e possuem a mesma cor.

É um algoritmo normalmente utilizado com grafos implícitos no formato de grade/matriz.
E facilmente implementado com algumas alterações feitas no [DFS](travessia.md).

```cpp
#define MAX_V 1000

char G[MAX_V][MAX_V];

int dx[] = { 1,  1,  1, -1, -1, -1,  0,  0};
int dy[] = {-1,  0,  1, -1,  0, -1, -1,  1};

int N, M; // Tamanho das colunas e linhas da matriz

int flood_fill(int x, int y, char color, char new_color) {

    if (x < 0 || y < 0 || x >= N || y >= M) return 0;
    if (G[x][y] != color) return 0;

    int vertice_count = 1;
    G[x][y] = new_color;

    for(int i = 0; i < 8; ++i)
        vertice_count += flood_fill(x + dx[i], y + dy[i], color, new_color);

    return vertice_count;
}
```

Este implementação, além de colorir os vértices, retornar o número de vértices
coloridos. E utiliza um pequeno truque para percorrer as 8 direções, ele usa os
arrays `dx` e `dy` para ir para todos as direções possíveis. Apenas incrementando
e decrementado as posições de x e y.

## Exercícios

1. UVA
1. URI

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
