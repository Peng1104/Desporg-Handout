Problema da mochila
======

O Problema da mochila binária.
---------

Antes de realizar a recursão precisamos compreender quais as situações que podem ocorrer com a mochila. Uma vez com todas as situações identificadas podemos definir quais são asa condições de parada e quais são valores retornados pela recursão.

Primeiramente precisamos de uma esturda que identifica cada Item dentro da mochila e como vimos cada Item possui um peso e um valor. Portanto temos a estrutura de um Item como:

``` c
typedef struct {
    int weight;
    int value;
} Item;
```

onde `md weight` é o peso do item e `md value` é o valor do item.

Feito isso precisamos analisar quais condições temos para a mochila e o item. O que nos leva a duas condições em relação a um Item,

A primeira sendo de **não** adicionar o Item

??? Como você calcula o valor da mochila sem o Item?
:::
Para poder saber o valor da mochila **sem** o Item precisamos apenas "pular o seu indice" na recursão e calcular o valor da mochila com os outros itens. `md V(i-1, j)`.
:::
???

E a segunda de **adicionar** o Item.

??? Como você calcula o valor da mochila com o Item?
:::
Para poder saber o valor da mochila **com** o Item precisamos calcular o valor de uma mochila intermediaria que possui um espaço igual ao espaço original menos o volume ocupado pelo Item e somar este resultado com o valor do Item. `md V(i-1, j-Wi) + Vi`.
:::
???

Primeiramente precisamos verificar se há espaço disponivel para adicionar um Item, ou seja, se o peso do item é menor ou igual ao espaço disponivel na mochila. $\text{Se } W_i \leq j$.

Caso contrario a resposta é trivial, não tem espaço para item portanto apenas existe a opção de não adicionar ele a mochila. $\text{Se } W_i > j$.

``` c
if (peso_do_item > espaco_disponivel) {
    return sem;
}
```

Se for o caso de haver espaço precisamos descobrir se vale a pena adicionar o Item. Para isso precisamos primeiro calcular qual seria o valor da mochila com o item.

Para calcular o valor da mochila com o Item precisamos criar uma sub mochila que possui como seu espaço disponivel o espaço da mochila atual menos o peso do Item $j-W_i$, o que repesenta o volume que seria ocupado pelo Item, calcular seu valor e somar este resultado com o valor do Item $V_i$.

``` c
int com = V(lista_de_items, numero_do_item - 1, espaco_disponivel - peso_do_item) + valor_do_item;
```

Feito isso precisamos calcular o valor da mochila sem o Item $V(i-1, j)$.

``` c
int sem = V(lista_de_items, numero_do_item - 1, espaco_disponivel);
```

E depois de calcular o valor da mochila sem o Item comparamos os valores e escolhemos o maior entre eles, já que o objetivo é maximisar a mochila. $\text{max}(V(i-1, j), V(i-1, j-W_i) + Vi)$.

``` c
if (sem > com) {
    return sem;
}
return com;
```

Além disso temos as condições relacionada a todos os itens e a mochila em si. Que são as condições de parada. 
A primeira é quando não há mais itens para adicionar na mochila, ou seja, $i = 0$.

``` c
if (numero_do_item == 0) {
    return 0;
}
```

E a segunda é quando não há mais espaço na mochila, ou seja, $j = 0$.

``` c
if (espaco_disponivel == 0) {
    return 0;
}
```

??? Porque o valor retornado é 0 nas condições de parada?
:::
O valor de zero representa que não há nenhum valor sendo adicionado a mochila.
:::
???

O que de forma matematica resulta em:

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(V(i-1, j), V(i-1, j-W_i) + Vi), & \text{Se } W_i \leq j \end{cases}$$

??? Como seria a função usando os juntando os codigos acima?
:::
``` c
int V(Item *lista_de_items, int numero_do_item, int espaco_disponivel) {
    if (numero_do_item == 0) {
        return 0;
    }
    if (espaco_disponivel == 0) {
        return 0;
    }
    int sem = V(lista_de_items, numero_do_item - 1, espaco_disponivel);

    int peso_do_item = lista_de_items[numero_do_item - 1].weight;

    if (peso_do_item > espaco_disponivel) {
        return sem;
    }
    int valor_do_item = lista_de_items[numero_do_item - 1].value;

    int com = V(lista_de_items, numero_do_item - 1, espaco_disponivel - peso_do_item) + valor_do_item;

    if (sem > com) {
        return sem;
    }
    return com;
}
```
:::
???

TESTE

:matrix

??? Exercício 1
Para entender um pouco melhor a recursão na prática, vamos fazer um exercício utilizando matrizes. Considere os items de 1 a 4, com diferentes pesos Wi e valores Vi e uma mochila com capacidade `md C = 5`.

|**item** |1 |2 |3 |4 |
|--|--|--|--|--|
|**Wi**|3 | 1| 4| 2 |
|**Vi**|8 | 3| 10| 7|


Vamos utilizar a definição da recursão para preencher a matriz, que possui colunas que indicam capacidades intermediárias da mochila e linhas que indicam os diferentes itens.
$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(V(i-1, j), V(i-1, j-W_i) + Vi), & \text{Se } W_i \leq j \end{cases}$$

O que podemos afirmar sobre a coluna `md j = 0` e a linha `md i = 0`?

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |    |    |    |    |    |    |
**1**  |    |    |    |    |    |    |
**2**  |    |    |    |    |    |    |
**3**  |    |    |    |    |    |    |
**4**  |    |    |    |    |    |    |

::: Gabarito
Quando `md j = 0` não é possível alocar nada dentro da mochila, dessa forma o valor máximo é 0 para todos os itens.

Quando a quantidade de itens é 0, o valor máximo também é 0, independentemente da capacidade da mochila.

Assim, podemos preencher a primeira linha e a primeira coluna com 0.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |0   |0   |0   |0   |0   |0   |
**1**  |0   |    |    |    |    |    |
**2**  |0   |    |    |    |    |    |
**3**  |0   |    |    |    |    |    |
**4**  |0   |    |    |    |    |    |
:::

???

??? Exercício 2

Vamos manter a tabela dos itens e a definição da recursão para facilitar no preenchimento da matriz.

|**item** |1 |2 |3 |4 |
|--|--|--|--|--|
|**Wi**|3 | 1| 4| 2 |
|**Vi**|8 | 3| 10| 7|

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(V(i-1, j), V(i-1, j-W_i)), & \text{Se } W_i \leq j \end{cases}$$

Termine de preencher a linha `md i = 1` utilizando a definição da recursão.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |0   |0   |0   |0   |0   |0   |
**1**  |0   |    |    |    |    |    |
**2**  |0   |    |    |    |    |    |
**3**  |0   |    |    |    |    |    |
**4**  |0   |    |    |    |    |    |

::: Gabarito

Para `md i = 1` e `md j = 1`, temos:

O peso do item 1 é W1 = 3. Olhando a condição da recursão temos que, W1 > 1. Assim, utilizaremos o primeiro caso da recursão.

$$\text{V}(1,1) = \text{V}(1-1, 1) = \text{V}(0,1)$$


V(0,1) já foi calculado anteriormente e basta acessar a matriz na posição 0,1.

`md Repare que o primeiro caso da recursão sempre leva a posição imediatamente acima na matriz`.

$$
\textbf{Assim, V}(1,1) = 0
$$



Esse comportamento é observado enquanto o peso do item 1 (`md Wi = 3`) for maior do que j.


Para `md i = 1` e `md j = 2`, temos:

$$
\textbf{V}(1,2) = \text{V}(0,2) = 0
$$


Para `md i = 1` e `md j = 3`, temos:

$$
W_1 \geq 3
$$

Assim, utilizamos o segundo caso da recursão.

$$
\text{V}(1, 3) = \max\left\{ \text{V}(1-1, 3); \text{V}(1-1, 3-3) + 8 \right\}
$$

O primeiro termo já está preenchido na matriz:

$$
\text{V}(0, 3) = 0
$$


O segundo termo acessa uma posição existente da matriz e soma 8.
$$
\text{V}(0,0) + 8 = 0 + 8 = 8
$$


O máximo entre 0 e 8 é 8.

$$
\textbf{Assim, V}(1,3) = 8
$$


Esse mesmo comportamento é observado para o resto da linha, visto que conforme o volume intermediário cresce (`md j = 4` e `md j = 5`), cairemos sempre no segundo caso da recursão.

Portanto, 

Para a posição 1,4 temos:

$$
\text{V}(1,4) = \max\left\{ \text{V}(1-1, 4); \text{V}(1-1, 4-3) + 8 \right\}
$$

$$
\text{V}(0,4) = 0
$$

$$
\text{V}(0,1) + 8 = 0 + 8 = 8
$$

$$
\textbf{Logo, V}(1,4) = 8
$$


Para a posição 1,5 temos:

$$
\text{V}(1,5) = \max\left\{ \text{V}(1-1, 5); \text{V}(1-1, 5-3) + 8 \right\}
$$

$$
\text{V}(0, 5) = 0
$$

$$
\text{V}(0, 2) + 8 = 0 + 8 = 8
$$

$$
\textbf{Logo, V}(1,5) = 8
$$


Assim, podemos adicionar na matriz os valores já calculados.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |0   |0   |0   |0   |0   |0   |
**1**  |0   |0   |0   |8   |8   |8   |
**2**  |0   |    |    |    |    |    |
**3**  |0   |    |    |    |    |    |
**4**  |0   |    |    |    |    |    |
:::

???

??? Exercício 3


|**item** |1 |2 |3 |4 |
|--|--|--|--|--|
|**Wi**|3 | 1| 4| 2 |
|**Vi**|8 | 3| 10| 7|

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(V(i-1, j), V(i-1, j-W_i)), & \text{Se } W_i \leq j \end{cases}$$

Termine de preencher a linha `md i = 2` utilizando a definição da recursão.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |0   |0   |0   |0   |0   |0   |
**1**  |0   |0   |0   |8   |8   |8   |
**2**  |0   |    |    |    |    |    |
**3**  |0   |    |    |    |    |    |
**4**  |0   |    |    |    |    |    |

::: Gabarito

Como o peso do item 2 é `md W2 = 1`, ele não é maior do que nenhum dos volumes intermediários `md j` a serem preenchidos. Dessa forma, todo o restante da linha 2 caiŕa no segundo caso da recursão.

Para `md i = 2` e `md j = 1`, temos:

$$
\text{V}(2,1) = \max\left\{ \text{V}(2-1, 1); \text{V}(2-1, 1-1) + 3 \right\}
$$

$$
\text{V}(1, 1) = 0
$$

$$
\text{V}(1, 0) + 3 = 0 + 3 = 3
$$

$$
\textbf{Logo, V}(2,1) = 3
$$

Para `md i = 2` e `md j = 2`, temos:

$$
\text{V}(2,2) = \max\left\{ \text{V}(2-1, 2); \text{V}(2-1, 2-1) + 3 \right\}
$$

$$
\text{V}(1, 2) = 0
$$

$$
\text{V}(1, 1) + 3 = 0 + 3 = 3
$$

$$
\textbf{Logo, V}(2,2) = 3
$$


Para `md i = 2` e `md j = 3`, temos:

$$
\text{V}(2,3) = \max\left\{ \text{V}(2-1, 3); \text{V}(2-1, 3-1) + 3 \right\}
$$

$$
\text{V}(1, 3) = 8
$$

$$
\text{V}(1, 2) + 3 = 0 + 3 = 3
$$

$$
\textbf{Logo, V}(2,3) = 8
$$


Para `md i = 2` e `md j = 4`, temos:

$$
\text{V}(2,4) = \max\left\{ \text{V}(2-1, 4); \text{V}(2-1, 4-1) + 3 \right\}
$$

$$
\text{V}(1, 4) = 8
$$

$$
\text{V}(1, 3) + 3 = 8 + 3 = 11
$$

$$
\textbf{Logo, V}(2,4) = 11
$$

Para `md i = 2` e `md j = 5`, temos:

$$
\text{V}(2,5) = \max\left\{ \text{V}(2-1, 5); \text{V}(2-1, 5-1) + 3 \right\}
$$

$$
\text{V}(1, 5) = 8
$$

$$
\text{V}(1, 4) + 3 = 8 + 3 = 11
$$

$$
\textbf{Logo, V}(2,5) = 11
$$

Assim, podemos adicionar na matriz os valores já calculados.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |0   |0   |0   |0   |0   |0   |
**1**  |0   |0   |0   |8   |8   |8   |
**2**  |0   |3   |3   |8   |11  |11  |
**3**  |0   |    |    |    |    |    |
**4**  |0   |    |    |    |    |    |
:::

???

??? Exercício 4


|**item** |1 |2 |3 |4 |
|--|--|--|--|--|
|**Wi**|3 | 1| 4| 2 |
|**Vi**|8 | 3| 10| 7|

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(V(i-1, j), V(i-1, j-W_i)), & \text{Se } W_i \leq j \end{cases}$$

Termine de preencher a linha `md i = 3` utilizando a definição da recursão.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |0   |0   |0   |0   |0   |0   |
**1**  |0   |0   |0   |8   |8   |8   |
**2**  |0   |3   |3   |8   |11  |11  |
**3**  |0   |    |    |    |    |    |
**4**  |0   |    |    |    |    |    |

::: Gabarito

Como o peso do item 3 é `md W3 = 4`, ele é maior do que os volumes intermediários  até `md j = 4`. Dessa forma, os valores da linha `md i = 3` caem no primeiro caso da recursão até `md j = 4`. Dessa forma, basta copiar o valor imediatamente acima para cada item.

Para `md i = 3` e `md j = 1`, temos:

$$\text{V}(3,1) = \text{V}(2, 1) = 3$$

Para `md i = 3` e `md j = 2`, temos:

$$\text{V}(3,2) = \text{V}(2, 2) = 3$$

Para `md i = 3` e `md j = 3`, temos:

$$\text{V}(3,3) = \text{V}(2, 3) = 8$$

A partir de `md j = 4`, caímos no segundo caso da recursão.

Para `md i = 3` e `md j = 4`, temos:

$$
\text{V}(3,4) = \max\left\{ \text{V}(3-1, 4); \text{V}(3-1, 4-4) + 10 \right\}
$$

$$
\text{V}(2, 4) = 11
$$

$$
\text{V}(2, 0) + 10 = 0 + 10 = 10
$$

$$
\textbf{Logo, V}(3,4) = 11
$$

Para `md i = 3` e `md j = 5`, temos:

$$
\text{V}(3,5) = \max\left\{ \text{V}(3-1, 5); \text{V}(3-1, 5-4) + 10 \right\}
$$

$$
\text{V}(2, 5) = 11
$$

$$
\text{V}(2, 1) + 10 = 3 + 10 = 13
$$

$$
\textbf{Logo, V}(3,5) = 13
$$

Assim, podemos adicionar na matriz os valores já calculados.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |0   |0   |0   |0   |0   |0   |
**1**  |0   |0   |0   |8   |8   |8   |
**2**  |0   |0   |0   |8   |11  |11  |
**3**  |0   |3   |3   |8   |11  |13  |
**4**  |0   |    |    |    |    |    |
:::

???

??? Exercício 5


|**item** |1 |2 |3 |4 |
|--|--|--|--|--|
|**Wi**|3 | 1| 4| 2 |
|**Vi**|8 | 3| 10| 7|

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(V(i-1, j), V(i-1, j-W_i)), & \text{Se } W_i \leq j \end{cases}$$

Termine de preencher a linha `md i = 4` utilizando a definição da recursão.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |0   |0   |0   |0   |0   |0   |
**1**  |0   |0   |0   |8   |8   |8   |
**2**  |0   |0   |0   |8   |11  |11  |
**3**  |0   |3   |3   |8   |11  |13  |
**4**  |0   |    |    |    |    |    |

::: Gabarito

Como o peso do item 3 é `md W4 = 2`, ele é maior do que os volumes intermediários até `md j = 1`. Dessa forma, para `md j = 1` cairemos no primeiro caso da recursão.

Para `md i = 4` e `md j = 1`, temos:

$$\text{V}(4,1) = \text{V}(3, 1) = 3$$

A partir de `md j = 2`, caímos no segundo caso da recursão.

Para `md i = 4` e `md j = 2`, temos:

$$
\text{V}(4,2) = \max\left\{ \text{V}(4-1, 2); \text{V}(4-1, 2-2) + 7 \right\}
$$

$$
\text{V}(3, 2) = 3
$$

$$
\text{V}(3, 0) + 7 = 0 + 7 = 7
$$

$$
\textbf{Logo, V}(4,2) = 7
$$

Para `md i = 4` e `md j = 3`, temos:

$$
\text{V}(4,3) = \max\left\{ \text{V}(4-1, 3); \text{V}(4-1, 3-2) + 7 \right\}
$$

$$
\text{V}(3, 3) = 8
$$

$$
\text{V}(3, 1) + 7 = 3 + 7 = 10
$$

$$
\textbf{Logo, V}(4,3) = 10
$$

Para `md i = 4` e `md j = 4`, temos:

$$
\text{V}(4,4) = \max\left\{ \text{V}(4-1, 4); \text{V}(4-1, 4-2) + 7 \right\}
$$

$$
\text{V}(3, 4) = 11
$$

$$
\text{V}(3, 2) + 7 = 3 + 7 = 10
$$

$$
\textbf{Logo, V}(4,4) = 11
$$

Para `md i = 4` e `md j = 5`, temos:

$$
\text{V}(4,5) = \max\left\{ \text{V}(4-1, 5); \text{V}(4-1, 5-2) + 7 \right\}
$$

$$
\text{V}(3, 5) = 13
$$

$$
\text{V}(3, 3) + 7 = 8 + 7 = 15
$$

$$
\textbf{Logo, V}(4,5) = 15
$$

Assim, podemos adicionar na matriz os valores já calculados.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |0   |0   |0   |0   |0   |0   |
**1**  |0   |0   |0   |8   |8   |8   |
**2**  |0   |0   |0   |8   |11  |11  |
**3**  |0   |0   |0   |8   |11  |11  |
**4**  |0   |0   |7   |8   |11  |15  |

Assim, conclui-se que a combinação que leva ao maior valor é do item 4 com o item 1.
:::

???
