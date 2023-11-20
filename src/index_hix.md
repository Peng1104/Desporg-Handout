Problema da mochila
======

O Problema da mochila binária.
---------

Portanto temos a estrutura de um item como:

``` c
typedef struct {
    int weight;
    int value;
} Item;
```

onde `md weight` é o peso do item e `md value` é o valor do item.

E como a equação é:

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(V(i-1, j), V(i-1, j-W_i) + Vi), & \text{Se } W_i \leq j \end{cases}$$

Temos que a função em c para realiazar a recursão é:

``` c
int value(Item *items, int item_number, int space_available) {
    if (item_number == 0) {
        return 0;
    }
    if (space_available == 0) {
        return 0;
    }
    int without = value(items, item_number - 1, space_available);

    int weight = items[item_number - 1].weight;

    if (weight > space_available) {
        return without;
    }
    int item_value = items[item_number - 1].value;

    int with = item_value + value(items, item_number - 1, space_available - weight);

    return with > without ? with : without;
}
```

Em que:

``` c
    if (item_number == 0) {
        return 0;
    }
```

é a primeira condição de parada pois ao chegar neste ponto não existe mais item a ser adicionado na mochila. Além disso temos a condição de parada para quando todo o espaço na mochila foi ocupado e não há mais espaço para adicionar qualquer novo item.

``` c
    if (space_available == 0) {
        return 0;
    }
```

Caso nenhuma das condições de parada seja satisfeita, ou seja, ainda existe espaço na mochila e existem items fora dela precisamos realizar a recursão. Primeiramente obtemos o valor da mochila **sem** o item adicionado `md V(i-1, j)`:

``` c
    int without = value(items, item_number - 1, space_available);
```

Feito isso verificamos se existe espaço disponivel na mochila para adicionar o item, caso não tenha retornamos o valor sem o item. `md w > C`

``` c
    int weight = items[item_number - 1].weight;

    if (weight > space_available) {
        return without;
    }
```

Caso tenha espaço calculamos valor da mochila **com** o item `md V(i-1, j-w)` que é a soma do valor do item mais a recursão sem o item de uma mochila nova intermediaria. Está mochila intermediaria tem sua capacidade reduzida pelo peso do item. 

``` c
    int item_value = items[item_number - 1].value;

    int with = item_value + value(items, item_number - 1, space_available - weight);
```

e retornamos o maior valor `md max{V(i-1, j), V(i-1, j-w)}`.

``` c
    return with > without ? with : without;
```

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

