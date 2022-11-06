Algoritmo de Fortune para Diagrama de Voronoi
======

Introdução
---------

Veremos hoje um algoritmo chamado de Algoritmo de Fortune. Este utiliza o Diagrama de Voronoi e uma lista de pontos, para verem as áreas de influência de um ponto.

Diagrama de Voronoi
---------
O diagrama de Voronoi é uma estrutura de dados geométrica importante para a solução de problemas de proximidade. O diagrama teve seu primeiro registro de uso por volta de 1644 por Descartes, no intuito de demonstrar a disposição da matéria no sistema solar.

![](descartes.jpg)

Ao observar a imagem, vemos que existem regiões, as quais estão mais perto de um certo ponto, ou seja área de influência. **Área de influência** neste caso seria onde um certo ponto seria o mais perto, em relação a um grupo de diversos pontos no plano. Ao analisar o plano inteiro, vai se formando uma figura, e o diagrama é formado.

![](voronoi.png)


??? Exercício 1

Quantas áreas de influência podem ser encontrados na imagem?

![](ex1.png)

::: Gabarito
16
:::

???

??? Exercício 2

O que as linhas da imagem representam?

::: Gabarito
As arestas do diagrama constituem um lugar onde dois pontos são equidistantes em relação à um local.
:::

???



Algoritmo de Fortuna
---------
O Algoritmo de Fortuna calcula diagramas de Voronoi utilizando duas premissas diferentes: a linha de varredura e a linha de praia. Imagine uma linha que vai varrendo o plano da esquerda para a direita, e que esta linha é reta e perpendicular com a base do plano. Isto seria a proposta da linha de varredura. Quando um ponto entra em contato com a linha de varredura, começa a se calcular uma circunferência, onde todos os pontos da circunferência estão equidistantes do ponto e da linha de varredura. Esta circunferência que vai sendo formada é a linha de praia.

![](linhas-do-fortune.png)

* {red}(Linha Vermelha) -> Linha de Varredura

* **Linha Azul** -> Linha de Praia




Algoritmo de Fortuna, por traz dos panos
---------
Beleza, mas como isso funciona na prática? Essencialmente, os pontos são colocados em uma lista de prioridade, com esta tendo como prioridade a proximidade da linha da varredura, e as parábolas são colocadas em uma árvore binária. Para cada item na lista, remove o item, calcula a parábola e adiciona as parábolas na árvore. Para cada uma se calcula aonde as intersecções vão ocorrer, e em seguida os pontos de intersecção entre circunferências são conectados por arestas, e estas geram as margens das áreas de influência do diagrama de Voronoi. Simples né? 




Implementando, fila de prioridade
---------

??? Exercício 3

Modifique a implementação da fila com lista ligada para que o struct _node tenha um parâmetro extra, a prioridade, e modifique put() para que ele receba um int priority, e adicione na fila baseado na prioridade, vamos considerar que maior o número, maior a prioridade, se mais de um número tiver a mesma prioridade, considere a ordem de inserção. 

``` c
struct _node {
    int value;
    struct _node *next;
};

typedef struct _node node;

void queue_int_put(queue_int *q, int value) {
    node *n = malloc(sizeof(node));
    n->value = value;
    n->next = NULL;
    if (q->last != NULL) {
        q->last->next = n;
    } else {
        q->first = n;
    }
    q->last = n;
}
```

::: Gabarito

``` c
struct _node {
    int value;
    int priority;
    struct _node *next;
};
//TODO - implementar a mudanca para que o gabarito esteja certo.
void queue_int_put(queue_int *q, int value, int priority) {
    node *n = malloc(sizeof(node));
    n->value = value;
    n->next = NULL;
    if (q->last != NULL) {
        q->last->next = n;
    } else {
        q->first = n;
    }
    q->last = n;
}
```
:::

???


Calculando as circunferências
---------
**MAGIA NEGRA ???**


??? Exercício 4

implemente uma funcao que calcule a parábola, baseado no ponto, e a linha de varredura
sua função deve receber um ponteiro para o struct do ponto, e a linha de varredura, e retornar? 

``` c
void parabula(struct *point, int varredura) {

}
```

::: Gabarito
``` c
void parabula(struct *point, int varredura) {
    //TODO - fazer o gabarito
}
```
:::

???


Arvore binaria
---------
Árvore binária é uma estrutura de dados caracterizada por:

- Ou não tem elemento algum (árvore vazia).
- Ou tem um elemento distinto, denominado raiz, com dois apontamentos para duas estruturas diferentes, denominadas sub-árvore esquerda e sub-árvore direita.


:fortune




Você também pode criar

1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
