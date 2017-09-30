Programação Dinâmica
====================

De todos os paradigmas de programação, Programação Dinâmica é o que é mais desafiador.
Ela procura responder aos problemas da maneira mais otimizada o possível, a maneira
ótima, e em tempo hábil. Diferente de outros paradigmas, como a Busca Completa, que
encontra todas as respostas em um tempo não otimizado.

A programação dinâmina, ou Dynamic Programming (sigla DP), necessita de dois elementos
para ser realizada:

1. Um estado: um conjunto de parâmetros que descrevam o problema naquele momento
1. Transições: como eu consigo ir de um estado ao outro

Com estes dois elementos é possível pensar em uma estratégia de DP para resolver
o problema.

Os problemas que envolvem uma solução com programação dinâmica, normalmente, querem
saber coisas como: "estratégia ótima", "minimizar o custo", "maximizar o ganho" e
"contar as maneiras de se fazer algo". Estes utilizam estratégias que são conhecidas
como problemas clássicos, ou DP Clássica, cujo a solução é conhecida, e tanto o estado
e a transição, são fáceis de deduzir. Porém, existe uma categoria de problemas,
que também envolvem estes contextos citados, que não são resolvidos via uma DP Clássica.
Estes devem ser resolvidos com uma DP Não Clássica, a qual você deverá deduzir a DP
por completo: estado e transição.

## Memoização

Memoização é o ato de memorizar uma solução. DP é um técnica tabular, ou seja,
ela utiliza uma tabela de apoio para guardar as soluções já encontradas. As dimensões
desta tabela dependem de quantas variáveis estão envolvidas no estado da DP.
Dado um conjunto destas variáveis, é possível saber se o problema já foi resolvido
para elas, ou não, através da tabela. Isto evita com que a DP faça processamento
repetido, porém aumenta muito o consumo de memória de uma DP. Com isso, quanto
mais estados uma DP analisar, mais rápida ela fica. O que causa um fator de amortização
no tempo dela. Fazendo ela um bom paradigma para casos de testes sucessivos.

### Dicas de memoização

A matriz de memoização costuma ser muito grande, dependendo do contexto. Por isso
uma dica é: sempre declarar esta matriz como global. Pois quando você declara
uma matriz global, o espaço em memória dela é reservado em tempo de compilação, sem
influenciar o tempo de execução do seu programa. Além disso, em um escopo global,
o seu programa pode acessar muito mais memória, pois ela é reservada em tempo de compilação.
Isto permite que sua matriz possa ser muito maior do que o permitido em um escopo local.

Outra dica, muitas DPs vão exigir que sua matriz de memoização seja inicializada com
um valor específico. O mais comum é que esse valor seja `0` ou `-1`. Caso isso aconteça,
o jeito mais rápido de se fazer isso é utilizando a função `memset`, que vai inicializar
todos os campos da sua matriz com `0` ou `1`.

```cpp
#define MAX 10000
int memo[MAX][MAX];

int main () {
    memset(memo, 0, sizeof memo); // Inicializando com 0
    memset(memo, -1, sizeof memo); // Inicializando com -1

    return 0;
}
```

Isto irá funcionar apenas com `0` e `-1`, com outros valores não é possível utilizar
o `memset`.

## DPs Clássicas

DPs Clássicas são DPs cujo o estado e transição já foram pensados e definidos.
Com elas, você precisa pensar só em como utiliza-las no seu contexto. As principais
são:

1. [Knapsack](#knapsack)
1. [Longest Increasing Subsequence](#longest-increasing-subsequence)
1. [Coin Change](#coin-change)
1. [Max Range Sum](#max-range-sum)
1. [Traveling Salesman Problem](#traveling-salesma-problem)

### Knapsack

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

#### Reconstruindo os itens

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

### Longest Increasing Subsequence

Longest Increasing Subsequence, ou LIS, procura resolver o problema de identificar
a maior subsequencia não contígua crescente. Por exemplo, dada a sequência
`{-7, 10, 9, 2, 3, 8, 8, 1}`, a LIS é `{-7, 2, 3, 8}`. Cada elemento da subsequencia
tem que ser maior que o elemento anterior.

O seguinte código apresenta uma implementação de LIS com complexidade quadrática.
Ela checa com quais números, anteriores ao atual. é possível fazer uma LIS maior.
Quando encontra, registra que a nova LIS tem o tamanho da LIS do número encontrado
`+ 1`.

```cpp
#define MAX 100

// Sequencia original
int sequence[MAX];

// Maior LIS formada pelo i-ésimo termo
int lis_n[MAX];

int lis(int N) {

    // Registra a maior LIS encontrada
    int max_lis = 0;

    for (int i = 0; i < N; ++i) {
        // Caso base, a menor LIS possível é 1, pois é formada
        // pelo próprio número
        lis_n[i] = 1;

        // Varrendo todos os elementos anteriores
        for (int j = i - 1; j >= 0; --j)

            // Caso o elemento seja menor que o atual
            // A LIS do i-ésimo termo é a maior entre a LIS já conhecida pra ele
            // e a LIS do j-ésimo termo + 1 (por adicionar o termo i a LIS)
            if (sequence[j] < sequence[i])
                lis_n[i] = max(lis_n[i], lis_n[j] + 1);

        max_lis = max(max_lis, lis_n[i]);
    }

    return max_lis;
}
```

O seguinte código apresenta uma implementação de LIS `O(n log n)`. Ela constroi
um array com a provavel LIS até o momento, se o número atual pode compor a LIS,
ele entra ao final do array. Se o número não compõem a LIS, mas forma uma menor,
ele é adicionado na posição da LIS que ele forma. Ao fim do algoritmo, o tamanho
do array é o tamanho da LIS.

```cpp
#define MAX 100

// Sequencia original
int sequence[MAX];

// Registra a LIS (que sempre está ordenada)
int lis_seq[MAX];

int lis(int N) {

    // Tamanho do lis_seq, inicialmente está vazio
    int lis_size = 0;

    for (int i = 0; i < N; ++i) {

        // Encontra a menor posição que é possível inserir o i-ésimo termo na LIS
        int p = lower_bound(lis_seq, lis_seq + lis_size, sequence[i]) - lis_seq;

        lis_seq[p] = sequence[i];
        lis_size = max(lis_size, p + 1);
    }

    return lis_size;
}
```

#### Reconstruindo a LIS

Caso o problema não queira saber apenas o tamanho da LIS, mas também quais são
os elementos dela, é necessário fazer adaptações em cada um dos métodos. Porém,
nem sempre é simples dizer os elementos da LIS, pois uma sequencia pode ter
várias LIS do mesmo tamanho. Normalmente, quando isso acontece, o problema
impõe algumas restrições para você saber qual LIS escolher.

A implementação a seguir mostra como capturar a primeira LIS, ela adapta o código
da solução `O (n log n)`. O método `reconstruct` faz o trabalho de imprimir a LIS
na ordem correta, enquanto o método `lis` descobre a LIS e registra os indices
dos números que geraram ela.

```cpp
#define MAX 100

// Sequencia original
int sequence[MAX];

// Registra a LIS (que sempre está ordenada)
int lis_seq[MAX];

// Guarda os idicies da atual LIS
int lis_seq_i[MAX];

// Guarda a referencia para o elemento anterior a i-ésimo elemento na LIS
// O primeiro elemento da LIS possui referencia para -1
int lis_parents[MAX];

// O último indice do ultimo elemento da LIS
int lis_max_i = -1;

int lis(int N) {

    // Tamanho do lis_seq, inicialmente está vazio
    int lis_size = 0;

    for (int i = 0; i < N; ++i) {

        // Encontra a menor posição que é possível inserir o i-ésimo termo na LIS
        int p = lower_bound(lis_seq, lis_seq + lis_size, sequence[i]) - lis_seq;

        lis_seq[p] = sequence[i];
        lis_seq_i[p] = i;

        // Caso p seja 0, quer dizer que este elemento não tem pai, pois é o
        // primeiro da LIS, então o pai dele é registrado como -1
        // Caso contrário, o pai é o indice do elemento anterior da LIS
        lis_parents[i] = (p ? lis_seq_i[p - 1] : -1);

        if (p + 1 > lis_size) {
            lis_size = p + 1;
            lis_max_i = i;
        }
    }

    return lis_size;
}

void reconstruct() {
    // É necessário o uso de uma stack na reconstrução da LIS, pois as referências
    // são guardadadas de maneira inversa
    stack<int> s;
    for(int i = lis_max_i; i != -1; i = lis_parents[i]) s.push(sequence[i]);

    printf("%d", s.top());
    while(s.pop(), !s.empty()) printf(" %d", s.top());
    printf("\n");
}
```

### Coin Change

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

#### Coin Change: Número de possibilidades

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

### Max Range Sum

#### Max 1D Range Sum

#### Max 2D Range Sum

### Traveling Salesman Problem

## Exercícios

1. UVA
    1. [11450 - Wedding Shopping](https://uva.onlinejudge.org/external/114/11450.pdf)
    1. [562 - Dividing coins](https://uva.onlinejudge.org/external/5/562.pdf)
    1. [10819 - Trouble of 13-Dots](https://uva.onlinejudge.org/external/108/10819.pdf)
    1. [10130 - SuperSale](https://uva.onlinejudge.org/external/101/10130.pdf)
    1. [990 - Diving for gold](https://uva.onlinejudge.org/external/9/990.pdf)
    1. [231 - Testing the CATCHER](https://uva.onlinejudge.org/external/2/231.pdf)
    1. [437 - The Tower of Babylon](https://uva.onlinejudge.org/external/4/437.pdf)
    1. [1196 - Tiling Up Blocks](https://uva.onlinejudge.org/external/11/1196.pdf)
    1. [481 - What Goes Up](https://uva.onlinejudge.org/external/4/481.pdf)

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
