Coin Change
===========

Coin Change, ou problema do Troco, é uma DP Clássica que resolve problemas do tipo:
dado um conjunto de moedas, qual o mínimo de moedas necessárias para formar um valor
`V` específico.

Por exemplo, se eu tenho as seguintes moedas disponíveis `{1, 3, 4, 5}`. Para obter
o valor `7`, existe a estratégia gulosa de escolher as maiores moedas possíveis.
Nesta estratégia as moedas escolhidas seriam `5 + 1 + 1`. Porém, a estrégia ótima
do Coin Change, selecionaria as moedas `3 + 4`, pois é o menor número de moedas
que formam `7`.

```cpp
#define MAX 1000010

int coins[MAX];
int memo[MAX];

int coin_change(int V, int N) {
    if (V == 0) return 0;
    if (V < 0) return numeric_limits<int>::max();

    if (memo[V] != -1) return memo[V];

    int n_coins = numeric_limits<int>::max();
    for(int i = 0; i < N; ++i) {
        n_coins = min(n_coins, coin_change(V - coins[i], N));
    }

    return memo[V] = n_coins + 1;
}

int main() {
    memset(memo, -1, sizeof memo);
}
```

## Coin Change: Número de possibilidades

Alguns problemas não querem saber apenas o número mínimo de elementos, moedas,
para formar um valor. Eles querem saber também de quantas maneiras distintas
é possível formar o valor. Para resolver isto, existe uma variação do Coin Change,
que conta de quantas maneirais possíveis o valor `V` é formado.

```cpp
#define MAX_COINS 11
#define MAX_VALUE 40010

int coins[MAX_COINS];
int memo[MAX_COINS][MAX_VALUE];

int coin_change_ways(int i, int V, int N) {
    if (V == 0) return 1;
    if (V < 0) return 0;
    if (i == N) return 0;

    if (memo[i][V] != -1) return memo[i][V];

    return memo[i][V] = coin_change_ways(i + 1, V, N) + coin_change_ways(i, V - coins[i], N);
}

int main() {
    memset(memo, -1, sizeof memo);
}
```

## Exercícios

1. UVA
1. URI

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
