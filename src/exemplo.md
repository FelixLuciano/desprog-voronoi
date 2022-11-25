Algoritmo de Fortune para Diagrama de Voronoi
======

Introdução
---------

Veremos hoje o Algoritmo de Fortuna. Este calcula um Diagrama de Voronoi apartir de uma lista de pontos, com isto podemos descobrir as areas de influencias de cada ponto no espaço.

Área de Influência
---------

Ao observar a imagem abaixo, vemos que existem regiões, as quais estão mais perto de um certo ponto, ou seja área de influência. Uma definição formal de **Área de influência** seria a seguinte: Dado um conjunto S de n pontos no plano queremos determinar para cada ponto p de S qual é a região dos pontos do plano que estão mais próximos de p do que de qualquer outro ponto em S. 
Deixando menos formal: Imagine uma região no espaço com pontos, a **Área de influência** de um ponto representa todo espaço que esta mais perto deste ponto do que todos os outros. Logo as regioes no espaço que estão mais perto de cada ponto.

Quando temos varios pontos no espaço, e desenhamos as areas de influença de todos eles, temos um diagrama de voronoi.

![](voronoi.png)

??? Exercício 1

Observando os pontos na imagem, tente desenhar as suas áreas de influência.

![](ex1.png)

::: Gabarito
![](ex1-gabarito.png)
:::

???

Diagrama de Voronoi
---------
O diagrama de Voronoi é uma estrutura de dados geométrica importante para a solução de problemas de proximidade. O diagrama teve seu primeiro registro de uso por volta de 1644 por Descartes, no intuito de demonstrar a disposição da matéria no sistema solar.

![](descartes.jpg)

??? Exercício 2

O que as linhas das áreas de influência representam?

![](voronoi.png)

::: Gabarito
As arestas do diagrama constituem um lugar onde dois pontos são equidistantes em relação à um local.
:::

???

Algoritmo de Fortuna
---------
Tá, mas como o calculamos isso? Oque é esse algoritmo de fortuna?

![](Fortunes-algorithm-slowed.gif)


Olhando a animação, da pra entender facinho né?. 

**Não.**

No momento pode parecer magica, mas voce vai ver que entendendo parte por parte, vai fazer sentido.



Parabolas
---------
Para entender como o algoritmo de fortuna calcula as areas de influencia, primeiro temos que dar um passo pra traz e entender parabolas.


??? Exercício 3
Imagine que temos um ponto P (que tambem vamos chamar de foco), e uma linha L, existe uma parabola que representa todos os pontos que estao equidistantes entre P e o ponto mais proximo em L.
Desenhe em seu caderno, um ponto P, uma linha L, e a parabola dos pontos equidistantes.

**Dica:** Use uma regua, um papel quadriculado, comece com uma linha diretamente em cima de P, que vai reto para L e marque seu meio, dai em diante mova 1 quadrado para esquerda (e direita) em L, faca uma linha normal a L, em direcao a P, vai existir um ponto que é equidistante a linha normal a L e P.


::: Gabarito

![](parabula.png)
:::

???

De agora em diante, vamos chamar essa linha L de linha de varredura, depois voce vai entender porque.

Beleza, temos uma parabola que marca a região equidistante entre a linha de varredura, e um ponto, mas pra que serve isso?

A utilidade de definir uma parabola desse jeito, vem no momento que temos mais de 1 ponto para a mesma linha de varredura. Pense, se temos 2 pontos para a mesma linha,
vai existir uma intersecção entre as duas parabolas. 

**Dica:** temos este desmos para ajudar: https://www.desmos.com/calculator/8ebv2gj7jm

??? Exercício 4
Oque temos nessa intersecção? 

::: Gabarito

Um ponto equidistante dos dois pontos P1 e P2.
:::

???

E é graças a esse detalhe que o algoritmo de fortuna funciona, imagine que vamos movendo a linha de varredura pelo plano, uma vez que duas parabolas se chocam, marcamos esse ponto e começamos a gerar uma linha, a linha sempre vai representar a regiao equidistante dos dois focos, e com isso, temos o começo de um diagram de vonoroi!


Algoritmo de Fortuna
---------
O Algoritmo de Fortuna calcula diagramas de Voronoi utilizando duas premissas diferentes: a linha de varredura e a linha de praia. Ja vimos a linha de varredura, mas oque é essa linha de praia? 
Nada alem das proprias parabolas que estamos calculando

![](linhas-do-fortune.png)

* {red}(Linha Vermelha) -> Linha de Varredura

* **Linha Azul** -> Linha de Praia

??? Exercício 3

Tente desenhar a linha de varredura e a linha de praia para os 3 primeiros pontos.




![](/fortune/fortune01.drawio.png)

::: Gabarito
:fortune
:::

???

Eventos
---------
Por mais que pareca com as animações, o algoritmo de fortuna nao funciona movendo a linha de varredura pixel a pixel pelo plano, ele funciona de evento em evento. Mas oque é um evento? um evento pode ser o proximo ponto na lista, ou a colisao entre parabulas, ja vimos como o choque entre 2 parabolas nos diz aonde comecar a desenhar as linhas do diagrama de vonoroi, mas como fazemos com 3 ou mais parabolas? Bem, toda intersecção entre arcos indica um ponto equidistante dos focos, e como vimos, conforme movemos a linha de varredura para longe de cada foco, o ponto equidistante se move em uma linha reta, mas no mometo em que 3 parabolas se chocam, uma das parabolas sera engolida pelas outras duas, gerando uma aresta entre 2 linhas, e solidificando a area de influencia de um ponto.

![](removal_before.png)


![](removal_after.png)


Com tudo isso, podemos entender a logica do algoritmo:

1.Encha a lista de eventos (uma lista lincada de prioridade) com cada ponto, em ordem.

2.Enquanto a lista de eventos não estiver vazia.

2.1 Ler o evento, se for um novo ponto, crie a parabola e adicione a colisao das parabolas na lista de eventos.

2.2 Processar o evento, se nao for um novo ponto, tem que ser uma colisao de parabolas, marque a intersecção, crie a linha, e remova a parabola engolida.





referencia: https://jacquesheunis.com/post/fortunes-algorithm/





#############################################################

Algoritmo de Fortuna, por traz dos panos
---------
Beleza, mas como isso funciona na prática? Essencialmente, os pontos são colocados em uma lista de prioridade, com esta tendo como prioridade a proximidade da linha da varredura, e as parábolas são colocadas em uma árvore binária de busca. Para cada item na lista, remove o item, calcula a parábola e adiciona as parábolas na árvore. Para cada uma se calcula aonde as intersecções vão ocorrer, e em seguida os pontos de intersecção entre circunferências são conectados por arestas, e estas geram as margens das áreas de influência do diagrama de Voronoi. Simples né? 

Implementando, fila de prioridade
---------

??? Exercício 3

Modifique a implementação da fila com lista ligada para que o struct _node tenha um parâmetro extra, a prioridade, e modifique put() para que ele receba um int priority, e adicione na fila baseado na prioridade. Vamos considerar que maior o número, maior a prioridade, e se mais de um número tiver a mesma prioridade, considere a ordem de inserção. 

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
Agora precisamos mudar para um mundo mais matemático para entender como as circunferências são calculadas. Primeiro vamos voltar as para as parábolas. Imagine a linha de varredura e um ponto, existe parábola, e todo ponto na parábola está equidistante da linha de varredura e do ponto.

![](parabola.png)

Para simplificar este handout, vamos pular a dedução e ir direto para a equação:

![](formula.png)

Foi feita uma análise interativa que pode ser vista através deste link: https://www.desmos.com/calculator/8ebv2gj7jm


??? Exercício 4
Implemente uma função que calcula a parábola baseada no ponto e a linha de varredura. Sua função deve receber um ponteiro para o struct do ponto e a linha de varredura, e retornar a parábola. 

``` c
//Essa questão foi possivel por conta da da explicação de https://jacquesheunis.com/post/fortunes-algorithm/. A fórmula foi tirada diretamente de lá, junto com as imagens.

void parabola(struct *point, int varredura) {
    int x;
    int y;
    int s;
}
```

::: Gabarito
``` c
int parabola(struct *point, int varredura) {

    int primeiro = 1 / ( 2*(point.y - varredura));

    int segundo = math.pow((varredura - point.x), 2);

    int terceiro = (point.y + varredura)/2

    return (primeiro*segundo) + terceiro
}
```
:::

???


Mas como o Voronoi funciona? 
--------
Ainda falta responder a questão, porque isso funciona?

A resposta esta...
//esta seção precisa ser traduzida, mas a ideia eh utilizar essa explicação, do site https://jacquesheunis.com/post/fortunes-algorithm/, precisamos traduzir ainda

The useful part of this definition is that it gives us a handle on the distances between things. Let us consider for a moment two curves, defined by two different points 

//falta 

Since this is true for all points pp at which the two curves intersect, we can find the boundary between the two cells by moving the directrix around and drawing a line consisting of the intersection points. This is the mathematical reasoning behind Fortune’s algorithm. Although the algorithm doesn’t quite function mechanically in quite the way explained here, this is the reason why the output is a valid Voronoi diagram.

One thing I should mention though, is that I have yet to show why this intersection will always be a straight line.


Árvore binária
---------
Árvore binária de busca é um diagrama que separa os nós, vendo se é maior ou menor que o nó anterior. Assuma que temos um nó qualquer N, todos os valores maiores que N serão inseridos à sua direita, e todos os valores menores que N serão inseridos a sua esquerda.


![](bst.png)

??? Exercício 5

Para as inputs dadas, crie uma árvore binária de busca. Para o primeiro input, considere a base da árvore.
* Inputs: 21, 14, 28, 11, 18, 25, 32, 5, 12, 15, 19, 23, 27, 30, 37.

::: Gabarito
![](bst_gabarito.png)
:::

???



Juntando Tudo
---------
Já criamos uma fila de prioridade, calculamos as parábolas e entendemos como a árvore binária de busca funciona. Agora vamos juntar tudo e lidar com os eventos. Como foi dito previamente, o Algoritmo de Fortuna lida com dois tipos de eventos:

 * pontos que passam pela linha de varredura
 * parábolas que se encontram. 

Para os pontos na linha de varredura, ou seja, para todo ponto na fila, deve-se calcular a parábola, e para toda parábola deve-se calcular os pontos de intersecção. 

??? Exercício 6

Crie a função que processa os eventos. Pode chamar a função de parábola para facilitar.

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