Single Source Shortest Path
===========================

Algoritmos de menor caminho com fonte única, ou
_Single Source Shortest Path (SSSP)_, são algoritmos utilizados em grafos ponderados
para descobrir qual o menor caminho de um nó específico para os outros nós. Se o
grafos não for ponderado, a resposta pode ser encontrada com uma travessia simples,
como o [BFS ou DFS](travessia.md). Mas quando há peso nas arestas, ou seja, quando
o grafo é ponderado, é necessário uma estratégia diferente, pois nem sempre o
menor número de arestas te devolve o caminho com o menor peso. Sendo que o peso,
ou custo, do caminho é determinado pela soma dos pesos das arestas envolvidas
naquele caminho.

## Dijkstra

O algoritmo de menor caminho mais conhecido é o algoritmo de
[Dijkstra](https://en.wikipedia.org/wiki/Edsger_W._Dijkstra). Ele utiliza uma
estratégia gulosa com o BFS para explorar o grafo encontrando o menor caminho
de um nó fonte (_source_) para qualquer outro nó conectado a este.

No início do algoritmo todos os vértices estão a uma distância infinita da fonte,
mesmo a própria fonte, que está a uma distância `0` dela mesma. A partir da fonte,
o grafo começa a ser explorado utilizando uma fila de prioridades, que guarda
pares de distância e nó, onde são priorizadas as menores distâncias.

No loop de travessia o par que ocupa o topo da fila de prioridades possui a menor
distância da fonte, se esta distância `d` for maior do que a distância registrada
no vetor de `distances`, então já se pode descartar aquele par, pois aquela distância
já não é a menor possível.

Caso a distância `d` seja menor, então é possível explorar os vizinhos daquele nó,
pois ele faz parte do menor caminho até então. Os vizinhos que serão explorados
são aqueles cujo a distancia do pai mais o peso da aresta não ultrapassa a menor
distancia conhecida. Caso isso aconteça aquele vizinho é adicionado na fila de
prioridade com esta nova menor distância.

```cpp
// Maximo de vertices
#define MAX_V 1000

// Numero grande para representar a pior distancia
#define INF_NUM 100000000

using ii = pair<int, int>;

vector<ii> G[MAX_V];
int distances[MAX_V];

int sssp_dijkstra(int source, int destiny, int N) {
    fill(distances, distances + N + 1, INF_NUM);

    priority_queue< ii, vector<ii>, greater<ii> > to_visit;

    distances[source] = 0;
    to_visit.push(ii(distances[source], source));

    while(!to_visit.empty()) {
        // Distancia d :: Veritice v
        auto d = to_visit.top().first;
        auto v = to_visit.top().second;
        to_visit.pop();

        // Caso a distancia seja maior do que a distancia já registrada
        if (d > distances[v]) continue;

        for(auto i: G[v]) {
            auto i_v = i.first;
            auto i_d = i.second;

            if (distances[v] + i_d < distances[i_v]) {
                distances[i_v]  = distances[v] + i_d;
                to_visit.push(ii(distances[i_v], i_v));
            }
        }
    }

    return distances[destiny];
}
```

## Exercícios

1. UVA
    1. [10986 - Sending email](https://uva.onlinejudge.org/external/109/10986.pdf)
    1. [929 - Number Maze](https://uva.onlinejudge.org/external/9/929.pdf)
    1. [10389 - Subway](https://uva.onlinejudge.org/external/103/10389.pdf)

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
