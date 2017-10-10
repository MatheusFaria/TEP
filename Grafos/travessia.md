Travessia
=========

A trevessia de um grafo é a operação de passar por todos os seus vértices realizando
alguma operação específica. Esta operação pode ser tanto a impressão da informação
que do vértice, quanto algo mais complexo.

Existem dois modos de se fazer a travessia de um grafo:

1. Por Profundidade
1. Por Largura

## Por Profundidade (DFS - Depth First Search)

A travessia por profundidade, ou _Depth First Seach (DFS)_, é um modo de travessia
de grafos em que a partir de um vértice é seguindo o caminho para o próximo vertice,
e do próximo vai para próximo, até chegar ao fim, ou "fundo", do grafo. Ao chegar no
fim, a travessia volta para o vértice anterior, e segue pelo próximo caminho possível
até o fundo. Isto é feito até que todos os vértices tenham sido explorados.

```cpp
#define MAX_V 1000

vector<int> G[MAX_V];
bool visited[MAX_V];

void dfs(int v) {
    visited[v] = true;

    for(auto i: G[v])
        if (!visited[i])
            dfs(i);
}

int main() {
    memset(visited, 0, sizeof visited);
}
```

Nesta travessia, o array `visited` é utilizado para marcar quais vértices já foram
visitados, ou seja, quais véritces já passaram na travessia. Por isso, é importante
inicializar este array com valores `false` ou `0`, representado que incialmente
nenhum vértice foi visitado. Esta estratégia, de utilizar o array de visitados
é muito comum nos algoritmos de grafos, ela evita que um vértice seja processado
mais de uma vez, evitando loops infinitos.

Esta traverssia não passará por todos os vértices se o grafo for disconectado ou
direcionado. Pois nestes grafos, nem sempre há um caminho de qualquer vértice `A`
para qualquer vértice `B`, o que faz com que a travessia não prossiga ou não chegue
a estes vértices.

## Por Largura (BFS - Breadth First Search)

A travessia por largura, ou _Breadth First Search (BFS)_, é um modo de travessia
de grafos em que a partir de um vértice todos os seus vizinhos, vértices ligados
a ele, são visitados, então todos os vizinhos dos vizinhos são visitados. Isto
é feito até que todos os vértices tenham sido visitados. É uma travessia que
vai por camadas.

```cpp
#define MAX_V 1000

vector<int> G[MAX_V];
bool visited[MAX_V];

void bfs() {
    memset(visited, 0, sizeof visited);

    int initial_vertice = 0;

    queue<int> to_visit;
    to_visit.push(initial_vertice);
    visited[initial_vertice] = true;

    while(!to_visit.empty()) {
        auto v = to_visit.front();
        to_visit.pop();

        for (auto i: G[v]) {
            if (!visited[i]) {
                visited[i] = true;
                to_visit.push(i);
            }
        }
    }
}
```

Assim como o DFS, esta travessia não passará por todos os vértices, se o grafo for
direcionado ou disconectado. Vai depender do vértice inicial.

### Distância entre vértices

Esta traverssia, diferente to DFS, não é recursiva, e pode sofrer modificações
simples para contar a distância do vértice inicial, `initial_vertice`, para qualquer
outro vértice.

```cpp
#define MAX_V 1000

vector<int> G[MAX_V];
int distance[MAX_V];

void bfs_distance() {
    memset(distance, -1, sizeof distance);

    int initial_vertice = 0;

    queue<int> to_visit;
    to_visit.push(initial_vertice);
    distance[initial_vertice] = 0;

    while(!to_visit.empty()) {
        auto v = to_visit.front();
        to_visit.pop();

        for (auto i: G[v]) {
            if (distance[i] == -1) {
                distance[i] = distance[v] + 1;
                to_visit.push(i);
            }
        }
    }
}
```

## Exercícios

1. UVA
    1. [118 - Mutant Flatworld Explorers](https://uva.onlinejudge.org/external/1/118.pdf)
    1. [280 - Vertex](https://uva.onlinejudge.org/external/2/280.pdf)
    1. [11831 -  Sticker Collector Robots](https://uva.onlinejudge.org/external/118/11831.pdf)
    1. [11902 - Dominator](https://uva.onlinejudge.org/external/119/11902.pdf)
    1. [12442 - Forwarding Emails](https://uva.onlinejudge.org/external/124/12442.pdf)
1. URI
    1. [1621 - Labyrinth](https://www.urionlinejudge.com.br/judge/problems/view/1621)
    1. [1550 - Inversion](https://www.urionlinejudge.com.br/judge/problems/view/1550)
    1. [1469 - Boss](https://www.urionlinejudge.com.br/judge/problems/view/1469)
    1. [1928 - Memory Game](https://www.urionlinejudge.com.br/judge/problems/view/1928)
    1. [1317 - I Hate SPAM, But Some People Love It](https://www.urionlinejudge.com.br/judge/problems/view/1317)

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
