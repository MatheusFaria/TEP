Union Find Disjoint Set
=======================

Esta estrutura é feita para responder, de forma otimizada, perguntas sobre conjuntos.
Ela utiliza uma forma de árvore, que vai se alterando com o tempo, para guardar
quais elementos pertencem a um certo conjunto. Com isso você consegue saber
se dois elementos estão ou não no mesmo conjunto com complexidade
próxima de `O(1)`.

A UFDS pode responder, com muita eficiencia, perguntas de se dois nós de um
grafo estão conectados ou não. Ou operações de união entre conjuntos.

```cpp
class UFDS {
public:
    vector<int> parents, rank;

    UFDS(int N) {
        rank.assign(N, 0);
        parents.assign(N, 0);
        for(int i = 0; i < N; ++i) parents[i] = i;
    }

    int findSet(int i) {
        if (parents[i] == i) return i;
        return parents[i] = findSet(parents[i]);
    }

    bool isSameSet(int i, int j) {
        return findSet(i) == findSet(j);
    }

    void unionSet(int i, int j) {
        if (!isSameSet(i, j)) {
            int x = findSet(i), y = findSet(j);
            if (rank[x] > rank[y]) parents[y] = x;
            else {
                parents[x] = y;
                if (rank[x] == rank[y]) rank[y]++;
            }
        }
    }
};
```

## Exercícios

1. UVA
    1. [793 - Network Connections](https://uva.onlinejudge.org/external/7/793.pdf)
    1. [10583 - Ubiquitous Religions](https://uva.onlinejudge.org/external/105/10583.pdf)
    1. [10608 - Friends](https://uva.onlinejudge.org/external/106/10608.pdf)
    1. [11503 - Virtual Friends](https://uva.onlinejudge.org/external/115/11503.pdf)
1. CodeForces
    1. [501B - Misha and Changing Handles](https://codeforces.com/problemset/problem/501/B)

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
