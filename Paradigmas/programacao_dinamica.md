Programação Dinâmica
====================

De todos os paradigmas de resolução de problemas, Programação Dinâmica é o que é mais desafiador.
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

1. [Knapsack](knapsack.md)
1. [Longest Increasing Subsequence](lis.md)
1. [Coin Change](coin-change.md)
1. Max Range Sum
1. Traveling Salesman Problem

## Exercícios

1. UVA
    1. [11450 - Wedding Shopping](https://uva.onlinejudge.org/external/114/11450.pdf)

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
