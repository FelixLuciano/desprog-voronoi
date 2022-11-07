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
Beleza, mas como isso funciona na prática? Essencialmente, os pontos são colocados em uma lista de prioridade, com esta tendo como prioridade a proximidade da linha da varredura, e as parábolas são colocadas em uma árvore binária de busca. Para cada item na lista, remove o item, calcula a parábola e adiciona as parábolas na árvore. Para cada uma se calcula aonde as intersecções vão ocorrer, e em seguida os pontos de intersecção entre circunferências são conectados por arestas, e estas geram as margens das áreas de influência do diagrama de Voronoi. Simples né? 




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
 
// Function to push according to priority
//codigo retirado de https://www.geeksforgeeks.org/priority-queue-using-linked-list/
void push(Node* s, int value, int priority)
{
    Node* start = (*s);
 
    // Create new Node
    Node* temp = newNode(value, priority);
 
    // Special Case: The head of list has lesser
    // priority than new node. So insert new
    // node before head node and change head node.
    if ((*s)->priority > priority) {
 
        // Insert New Node before head
        temp->next = *s;
        (*s) = temp;
    }
    else {
 
        // Traverse the list and find a
        // position to insert new node
        while (start->next != NULL &&
            start->next->priority < priority) {
            start = start->next;
        }
 
        // Either at the ends of the list
        // or at required position
        temp->next = start->next;
        start->next = temp;
    }
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
Árvore binária de busca é uma arvore binaria com seguinte caracteristica: 
Assuma que temos um nó qualquer N, todos os valores maiores que N, serão inseridos a sua direita, e todos os valores menores que N serão inseridos a sua esquerda.


![](bst.png)

??? Exercício 5

Para as inputs dadas, crie uma arvore binaria de busca, considere a base da arvore a primeira input.
inputs: 21, 14, 28, 11, 18, 25, 32, 5, 12, 15, 19, 23, 27, 30, 37.

::: Gabarito
![](bst_gabarito.png)
:::

???



Juntando Tudo
---------
Ja criamos uma fila de prioridade, calculamos as parabulas e entendemos como a arvore binaria de busca funciona, agora vamos juntar tudo e lidar com os enventos. Como foi dito previamente, o algoritmo de fortuna lida com dois tipos de eventos, pontos que passam pela linha de varredura, e parabulas que se encontram. 
Para os pontos na linha de varredura, para todo ponto na fila, deve-se calcular a parabula, e para toda parabula deve-se calcular os pontos de intersecção. 

??? Exercício

Crie a funcao que processa os ventos, pode chamar a funcao de parabula para facilitar

::: Gabarito

``` c

//codigo retirado de: 
//https://www.cs.hmc.edu/~mbrubeck/voronoi.html
void process_event()
{
   // Get the next event from the queue.
   event *e = events.top();
   events.pop();

   if (e->valid) {
      // Start a new edge.
      seg *s = new seg(e->p);

      // Remove the associated arc from the front.
      arc *a = e->a;
      if (a->prev) {
         a->prev->next = a->next;
         a->prev->s1 = s;
      }
      if (a->next) {
         a->next->prev = a->prev;
         a->next->s0 = s;
      }

      // Finish the edges before and after a.
      if (a->s0) a->s0->finish(e->p);
      if (a->s1) a->s1->finish(e->p);

      // Recheck circle events on either side of p:
      if (a->prev) check_circle_event(a->prev, e->x);
      if (a->next) check_circle_event(a->next, e->x);
   }
   delete e;
}

// Look for a new circle event for arc i.
void check_circle_event(arc *i, double x0)
{
   // Invalidate any old event.
   if (i->e && i->e->x != x0)
      i->e->valid = false;
   i->e = NULL;

   if (!i->prev || !i->next)
      return;

   double x;
   point o;

   if (circle(i->prev->p, i->p, i->next->p, &x,&o) && x > x0) {
      // Create new event.
      i->e = new event(x, o, i);
      events.push(i->e);
   }
}

// Find the rightmost point on the circle through a,b,c.
bool circle(point a, point b, point c, double *x, point *o)
{
   // Check that bc is a "right turn" from ab.
   if ((b.x-a.x)*(c.y-a.y) - (c.x-a.x)*(b.y-a.y) > 0)
      return false;

   // Algorithm from O'Rourke 2ed p. 189.
   double A = b.x - a.x,  B = b.y - a.y,
          C = c.x - a.x,  D = c.y - a.y,
          E = A*(a.x+b.x) + B*(a.y+b.y),
          F = C*(a.x+c.x) + D*(a.y+c.y),
          G = 2*(A*(c.y-b.y) - B*(c.x-b.x));

   if (G == 0) return false;  // Points are co-linear.

   // Point o is the center of the circle.
   o->x = (D*E-B*F)/G;
   o->y = (A*F-C*E)/G;

   // o.x plus radius equals max x coordinate.
   *x = o->x + sqrt( pow(a.x - o->x, 2) + pow(a.y - o->y, 2) );
   return true;
}
```
`.
:::

???



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
