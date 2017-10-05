Longest Increasing Subsequence (LIS)
====================================

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

## Reconstruindo a LIS

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

## Exercícios

1. UVA
    1. [231 - Testing the CATCHER](https://uva.onlinejudge.org/external/2/231.pdf)
    1. [437 - The Tower of Babylon](https://uva.onlinejudge.org/external/4/437.pdf)
    1. [1196 - Tiling Up Blocks](https://uva.onlinejudge.org/external/11/1196.pdf)
    1. [481 - What Goes Up](https://uva.onlinejudge.org/external/4/481.pdf)

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
