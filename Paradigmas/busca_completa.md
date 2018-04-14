Busca Completa
==============

Busca Completa, ou Força Bruta, é um paradigmas de resolução de problemas que
explora todas as possibilidades de respostas a fim de encontrar a certa.
Como todas as possibilidades são exploradas, você tem certeza de que vai encontrar
a resposta. Porém, são algoritmos de alta complexidade que não servem para a maioria
dos contextos. Eles vão ser utilizados quando:

1. O espaço da solução é muito pequeno e a alta complexidade não vai gerar um TLE
1. Não há outra solução sem ser a força bruta, por exemplo, lista as permutações de
um grupo de números.
1. Para gerar uma solução __offline__ ou mais casos de teste.

Qualquer solução que explore todo o espaço solução do problema é considerado uma
solução por busca completa, mesmo que ocorra a atividade de __prune__. __Prune__
é quando caminhos do espaço solição são removidos da análise, pois não são
candidatos a solução. Por exemplo, o problema te pede quantas combinações dos
N números apresentados dão a soma S, caso chegue a uma parte da busca completa
que a soma total já ultrapassou S, você pode parar de processar esta opção.

Há várias formas de se implementar uma busca completa. Exitem formas iterativas,
onde você itera sobre o espaço das soluções buscando a resposta correta. Ao
realizar uma busca completa iterativa, é muito importante se atentar ao critérios
de parada e limites da iteração.

Outra forma de implementar a busca completa é a forma recursiva,
também conhecida como backtrack. Que lista todos os candidatos a solução,
e a cada candidato julga se ele é uma solução ou não. Caso seja uma solução,
aquela é contabilizada, caso contrário ele é utilizado pra a construção de
outros candidatos.


## Exercícios

1. UVA
    1. [725 - Division](https://uva.onlinejudge.org/external/7/725.pdf)
    1. [441 - Lotto](https://uva.onlinejudge.org/external/4/441.pdf)
    1. [11565 - Simple Equatins](https://uva.onlinejudge.org/external/115/11565.pdf)
    1. [11742 - Social Constraints](https://uva.onlinejudge.org/external/117/11742.pdf)
1. URI
    1. [1654 - Grocery Store](https://www.urionlinejudge.com.br/judge/en/problems/view/1654)

## Referências

HALIM, Steve; HALIM, Felix. [Competitive Programming 3](http://cpbook.net/), Lulu, 2013.
