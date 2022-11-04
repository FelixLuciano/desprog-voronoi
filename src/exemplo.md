Título
======

Subtítulo
---------

Para criar um parágrafo, basta escrever um texto contínuo, sem pular linhas.

Introdução
---------

Veremos hoje um algoritmo chamado de Algoritmo de Fortune. Este utiliza o Diagrama de Voronoi e uma lista de pontos, para verem as áreas de influência de um ponto.

Diagrama de Voronoi
---------
O diagrama de Voronoi é uma estrutura de dados geométrica importante para a solução de problemas de proximidade. O diagrama teve seu primeiro registro de uso por volta de 1644 por Descartes, no intuito de demonstrar a disposição da matéria no sistema solar.

![](descartes.jpg)

Ao observar a imagem, vemos que existem regiões, as quais estão mais perto de um certo ponto, ou seja área de influência. **Área de influência** neste caso seria onde um certo ponto seria o mais perto, em relação a um grupo de diversos pontos no plano. Ao analisar o plano inteiro, vai se formando uma figura, e que as arestas do diagrama constituem um lugar onde os pontos são equidistantes em relação à dois locais.

![](voronoi.png)


??? Exercício 1

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

??? Exercício 2

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

??? Exercício 3

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???







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
