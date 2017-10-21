Componentes Fortemente Conectados
=================================

Componentes Fortemente Conectados, ou _Strongly Connected Components (SCC)_, são
partes do grafo tal que de qualquer vértice `u` eu consigo chegar em `v` e de
`v` eu consigo chegar em `u`. O grafo não-direcionado é um componente fortemente
conectado, porém em grafos direcionados é possível identificar mais de um componente
fortemente conectado. Pois dada a direção das arestas existem pares de vértices
`(u, v)`, onde é possível checar em `v` a partir de `u`, mas não o contrário.

Estes componentes podem ser vistos como pequenos grafos não direcionados,
dentro de um grande grafo direcionado. Com eles é possível reduzir o escopo
de alguns problemas de grafo que exigem grafos não direcionados.

## Tarjan

Um dos algoritmos para encontrar SCC é o algoritmo de
[Tarjan](https://en.wikipedia.org/wiki/Robert_Tarjan). Que utiliza uma
modificação do DFS para encontrar todos os componentes fortemente conectados.
Este algoritmo consegue contar quantos SCC existem e elencar quais vértices
pertencem a quais SCC. Para fazer isto, o algoritmo utiliza 2 marcações adicionais
no DFS, além do array de visitados. A primeira marcação `dfs_num`, diz em qual
chamada do DFS, indo de 1 a N, aquele vértice foi visitado. A segunda `dfs_low`,
diz a qual SCC aquele vértice pertence.

```cpp
#define UNVISITED -1
#define MAX_V 1000

vector<int> G[MAX_V];

int dfs_num[MAX_V];
int dfs_low[MAX_V];
bool visited[MAX_V];

vector<int> scc;

int dfs_count = 0;
int scc_count = 0;

void tarjan_scc(int v) {
    dfs_num[v] = dfs_low[v] = dfs_count++;
    scc.push_back(v);
    visited[v] = true;

    for(auto i: G[v]) {
        if (dfs_num[i] == UNVISITED) tarjan_scc(i);
        if (visited[i])
            dfs_low[v] = min(dfs_low[v], dfs_low[i]);
    }

    if (dfs_low[v] == dfs_num[v]) {
        printf("SCC %d:", ++scc_count);

        auto i = scc.back();
        do {
            i = scc.back();
            scc.pop_back();
            visited[i] = false;

            printf(" %d", i);
        } while(i != v);

        printf("\n");
    }
}

int main() {
    memset(dfs_low,  0, sizeof dfs_low);
    memset(dfs_num, UNVISITED, sizeof dfs_num);
    memset(visited, false, sizeof visited);

    dfs_count = scc_count = 0;

    int V = 0; // Número de vértices

    // ...

    for(int i = 0; i < V; i++)
        if(dfs_num[i] == UNVISITED)
            tarjan_scc(i);

    return 0;
}
```

## Exercícios

1. UVA
    1. [247 - Calling Circles](https://uva.onlinejudge.org/external/2/247.pdf)
    1. [11838 - Come and Go](https://uva.onlinejudge.org/external/118/11838.pdf)
    1. [10731 - Test](https://uva.onlinejudge.org/external/107/10731.pdf)
    1. [11709 - Trust groups](https://uva.onlinejudge.org/external/117/11709.pdf)

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
