Algoritmo de Fortune para Diagrama de Voronoi
======

Introdução
---------

Veremos hoje o Algoritmo de Fortuna. Este calcula um Diagrama de Voronoi a partir de uma lista de pontos, com isto podemos descobrir as áreas de influências de cada ponto no espaço.

Área de Influência
---------

Ao observar a imagem abaixo, vemos que existem regiões, as quais estão mais perto de um certo ponto, ou seja área de influência. Uma definição formal de **Área de influência** seria a seguinte: Dado um conjunto S de n pontos no plano queremos determinar para cada ponto p de S qual é a região dos pontos do plano que estão mais próximos de p do que de qualquer outro ponto em S. 
Deixando menos formal: Imagine uma região no espaço com pontos, a **Área de influência** de um ponto representa todo espaço que esta mais perto deste ponto do que todos os outros. Logo as regiões no espaço que estão mais perto de cada ponto.

Quando temos vários pontos no espaço, e desenhamos as áreas de influência de todos eles, temos um diagrama de voronoi.

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
Tá, mas como o calculamos isso? O que é esse algoritmo de fortuna?

![](Fortunes-algorithm-slowed.gif)


Olhando a animação, facilmente conseguimos deduzir como fazer né?. 

**Não.**

No momento pode parecer mágica, mas você vai ver que entendendo parte por parte, vai fazer sentido.



Parábolas
---------
Para entender como o algoritmo de fortuna calcula as áreas de influência, primeiro temos que dar um passo para atrás e entender parábolas.


??? Exercício 3
Imagine que temos um ponto P (que chamaremos de foco) e uma linha L. Podemos afirmar que existe uma parábola, em que todos os pontos desta estão equidistantes entre o foco e o ponto mais próximo de L.
Desenhe em uma folha quadriculada, um ponto P, uma linha L, e a parábola dos pontos equidistantes.



**Dica:** Use uma régua, um papel quadriculado, comece com uma linha diretamente em cima de P que vai reto para L, e por fim marque seu meio. Em seguida, mova 1 quadrado para esquerda (ou direita) em L, faça uma linha perpendicular a L em direção a P. Vá fazendo estes pontos até perceber a formação da parábola.


::: Gabarito

![](parabula.png)
:::

???

De agora em diante, vamos chamar essa linha L de linha de varredura, depois você vai entender o porquê.

Beleza, temos uma parábola que marca a região equidistante entre a linha de varredura e um ponto, mas para que serve isso?

A utilidade de definir uma parábola desse jeito, vem no momento que temos mais de 1 ponto para a mesma linha de varredura. Pense, se temos 2 pontos para a mesma linha,
vai existir uma intersecção entre as duas parabolas. 

**Dica:** temos este desmos para ajudar: https://www.desmos.com/calculator/8ebv2gj7jm

??? Exercício 4
O que temos nessa intersecção? 

::: Gabarito

Um ponto equidistante dos dois pontos P1 e P2.
:::

???

E é graças a esse detalhe que o algoritmo de fortuna funciona, imagine que vamos movendo a linha de varredura pelo plano, uma vez que duas parábolas se chocam, marcamos esse ponto e começamos a gerar uma linha, a linha sempre vai representar a região equidistante dos dois focos, e com isso, temos o começo de um Diagrama de Voronoi!


Algoritmo de Fortuna
---------
O Algoritmo de Fortuna calcula diagramas de Voronoi utilizando duas premissas diferentes: a linha de varredura e a linha de praia. Já vimos a linha de varredura, mas o que é essa linha de praia? 
Nada além das próprias parábolas que estamos calculando.

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
Por mais que pareça com as animações, o algoritmo de fortuna não funciona movendo a linha de varredura pixel a pixel pelo plano, ele funciona de evento em evento. Mas o que é um evento? Um evento pode ser o próximo ponto na lista ou a colisão entre parábolas. Já vimos como o choque entre 2 parábolas nos diz onde comecar a desenhar as linhas do diagrama de vonoroi, mas como fazemos com 3 ou mais parábolas? 

![](removal_before.png)

 Bem, toda intersecção entre arcos indica um ponto equidistante dos focos, e como vimos, conforme movemos a linha de varredura para longe de cada foco, o ponto equidistante se move em uma linha reta. Mas no momento em que 3 parábolas se chocam, uma das parábolas será engolida pelas outras duas, gerando uma aresta entre 2 linhas, e solidificando a área de influência de um ponto.

![](removal_after.png)


Com tudo isso, podemos entender a logica do algoritmo:

1. Encha a lista de eventos (uma lista ligada de prioridade) com cada ponto, em ordem.

2. Enquanto a lista de eventos não estiver vazia:

    1. Ler o evento, se for um novo ponto, crie a parábola e adicione a colisão das parábolas na lista de eventos.

    2. Processar o evento. Se não for um novo ponto, tem que ser uma colisão de parábolas, marque a intersecção, crie a linha, e remova a parábola engolida.





referência: https://jacquesheunis.com/post/fortunes-algorithm/