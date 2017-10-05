Knapsack
========

Knapsack Binário, ou Knapsack 0-1, ou Subset Sum, ou Problema da Mochila, é uma
DP Clássica que resolve problemas do tipo: dado `N` itens, cada um com valor `V`
e peso `W`, e uma mochila que consegue carregar no máximo `S` de peso, compute
o **valor máximo** de itens que posso carregar nesta mochila. Neste problema,
você escolhe entre carrega ou não carregar cada item, para que a soma dos valores
dos itens carregados, sejam a soma ótima dado os seus pesos e a capacidade da
mochila.

Por exemplo, se temos uma mochila com capacidade de `S = 12`. E os seguintes
itens.

| Item | Peso `W` | Valor `V` |
|:----:|:--------:|:---------:|
| 0    | 10       | 100       |
| 1    | 4        | 70        |
| 2    | 6        | 50        |
| 3    | 12       | 10        |

Caso utilizemos a estratégia gulosa de pegar o item de maior valor, o item `0`,
nenhum outro item cabera na mochila, pois o item `0` pesa `10`. Sendo assim
o valor máximo atingido com esta configuração é `100`. Porém não é o valor ótimo.

Se selecionarmos os itens `1` e `2`, atingimos o valor de `120`, que já é melhor
que a configuração anterior, e ambos os itens cabem na mochila, pois a soma de seus
pesos é `10`. Estes itens representam a escolha ótima que maximiza o valor que pode ser
carregado na mochila.

```cpp
#define MAX_W 1000
#define MAX_V 1000
#define MAX_ITENS 1000

// pesos e valores
int W[MAX_ITENS];
int V[MAX_ITENS];

// matriz de memoização
int memo[MAX_W][MAX_V];

int knapsack(int i, int w) {

    // Caso o item não exista ou não mais caiba na mochila
    // nenhum valor será agregado
    if (i < 0 || w <= 0) return 0;

    // Caso este estado já tenha sido resolvido
    if (memo[i][w] != -1) return memo[i][w];

    // Caso o item estoure a capacidade da mochila
    // O item não será carregado
    if (W[i] > w) return memo[i][w] = knapsack(i - 1, w);

    // Caso contrário o valor ótimo será o maior valor entre a decisão
    // de não carregar o item e carregar o item
    return memo[i][w] = max(knapsack(i - 1, w),
                            knapsack(i - 1, w - W[i]) + V[i]);
}

int main() {
    // Inicialização da matriz de memoização com -1
    memset(memo, -1, sizeof memo);
}
```

## Reconstruindo os itens

Caso o problema queira saber os itens que foram pegos, é necessário fazer algumas
alterações no código visto anteriormente. A cada vez que um item é pego, ele tem que
ser registrado como pego na tabela de itens pegos. Assim quando chegarmos ao final
do knapsack, conseguimos saber todos os itens pegos seguindo de volta os registros
dos itens pegos.

A função `reconstruct` faz todo o trabalho de separar os itens que foram pegos.
E imprime eles na ordem que eles apareceram no input da questão. Para fazer esta
reconstrução, ela checa se o item atual, inciando pelo último item, foi pego ou não.
Se este foi pego, então o peso da mochila é reduzido de acordo com o peso do item
pego. Ela faz isto para todos os itens.

```cpp
#define MAX_W 1000
#define MAX_V 1000
#define MAX_ITENS 1000

// pesos e valores
int W[MAX_ITENS];
int V[MAX_ITENS];

// matriz de memoização
int memo[MAX_W][MAX_V];
bool taken[MAX_W][MAX_V];

int knapsack(int i, int w) {

    // Caso o item não exista ou não mais caiba na mochila
    // nenhum valor será agregado
    if (i < 0 || w <= 0) return 0;

    // Caso este estado já tenha sido resolvido
    if (memo[i][w] != -1) return memo[i][w];

    // Caso o item estoure a capacidade da mochila
    // O item não será carregado
    if (W[i] > w) return memo[i][w] = knapsack(i - 1, w);

    // Caso contrário o valor ótimo será o maior valor entre a decisão
    // de não carregar o item e carregar o item

    auto not_take = knapsack(i - 1, w);
    auto     take = knapsack(i - 1, w - W[i]) + V[i];

    if (take > not_take) {
        taken[i][w] = true;
        return memo[i][w] = take;
    }
    else return memo[i][w] = not_take;
}

void reconstruct(int i, int w) {
    stack <int> itens;

    do {
        if (taken[i][w]) {
            w -= W[i];
            itens.push(i);
        }
    } while(i--);

    while(!itens.empty()) {
        printf("%d %d\n", W[itens.top()], V[itens.top()]);
        itens.pop();
    }
}

int main() {
    memset(memo, -1, sizeof memo);
    memset(taken, false, sizeof taken);
}
```

## Exercícios

1. UVA
    1. [562 - Dividing coins](https://uva.onlinejudge.org/external/5/562.pdf)
    1. [10819 - Trouble of 13-Dots](https://uva.onlinejudge.org/external/108/10819.pdf)
    1. [10130 - SuperSale](https://uva.onlinejudge.org/external/101/10130.pdf)
    1. [990 - Diving for gold](https://uva.onlinejudge.org/external/9/990.pdf)

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
