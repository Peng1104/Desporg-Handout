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
 

A função V calcula o maior valor possível, considerando todos os itens até **i** (incluindo o item **i**) e uma capacidade intermediária **j**. Essa capacidade intermediária pode ser interpretada como uma versão menor da mochila em questão. Como aprendemos anteriormente, a recursão busca reduzir o escopo do problema e, nesse caso, otimizamos a combinação de itens para mochilas menores até chegarmos no tamanho da mochila original.


!!! Atenção
O parâmetro  **i**, considera a combinação de todos os itens até **i**, incluindo **i**.  
!!!

Assim, a recursão se divide em dois cenários:

$$V(i, j) = \begin{cases} \textcolor{red}{V(i-1, j)}, & \textcolor{red}{Se  W_i > j} \\ \max(V(i-1, j), V(i-1, j-W_i) + Vi), & \text{Se } W_i \leq j \end{cases}$$

Quando o peso do item em questão ultrapassa a capacidade intermediária considerada **(Wi > j)**, devemos retornar a última combinação de itens, sem o item **i**, ou seja, **i-1** e com a mesma capacidade intermediária **j**.`


$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\\textcolor{red}{\max(V(i-1, j), V(i-1, j-W_i) + Vi),} & \textcolor{red}{Se  W_i \leq j}\end{cases}$$

Quando o peso do item em questão não ultrapassa a capacidade intermediária considerada **(Wi <= j)**, podemos adicioná-lo. Assim, temos que checar qual possibilidade nos traz um maior valor máximo, quando incluímos o item **i** ou quando não o fazemos.

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(\textcolor{blue}{V(i-1, j)}, \textcolor{green}{V(i-1, j-W_i) + Vi}), & \text{Se } W_i \leq j \end{cases}$$

Quando <span style="color:blue">não incluímos </span> o item **i**, consideramos o maior valor da combinação:
* imediatamente antes de adicionar **i**, ou seja, **i-1**.
* com a mesma capacidade **j**. 

Assim, <span style="color:blue">V(i-1 , j)</span>.

Quando <span style="color:green">incluímos</span> o item **i**, consideramos o maior valor da combinação:
* imediatamente antes de adicionar **i**, ou seja, **i-1**.
* com uma capacidade menor, pois, ao adicionar o item **i** a capacidade restante da mochila diminui, ou seja, **j-Wi**.

Encontrando o maior valor dessa combinação, como estamos adicionando o item **i**, adicionamos o seu valor **Vi**.

Assim, <span style="color:green">V(i-1 , j-Wi) + Vi</span>.

Dessa forma, o cenário que trouxer um maior valor entre <span style="color:blue">V(i-1 , j)</span> e <span style="color:green">V(i-1 , j-Wi) + Vi</span> será retornado.


A matriz da recursão
---------
 
Para entender um pouco melhor a recursão na prática, vamos utilizar matrizes. Considere os items de 1 a 4, com diferentes pesos Wi e valores Vi e uma mochila com capacidade `md C = 5`.

|**item** |1 |2 |3 |4 |
|--|--|--|--|--|
|**Wi**|3 | 1| 4| 2 |
|**Vi**|8 | 3| 10| 7|

Vamos aplicar o algoritmo nessa matriz que, suas linhas representam os diferentes itens e suas colunas, diferentes capacidades intermediárias:

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |    |    |    |    |    |    |
**1**  |    |    |    |    |    |    |
**2**  |    |    |    |    |    |    |
**3**  |    |    |    |    |    |    |
**4**  |    |    |    |    |    |    |

??? Checkpoint X

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

Quando a quantidade de itens é 0, `md i = 0`, o valor máximo também é 0, independentemente da capacidade da mochila.

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

Veja essa animação para entender como o algoritmo é exemplificado através da matriz.

:matrix

??? Exercício 1

Considere os seguintes itens de 1 a 4 com diferentes pesos e valores. Considere uma mochila com capacidade `md C = 4`.

|**item** |1 |2 |3 |4 |
|--|--|--|--|
|**Wi**|1 | 4| 2| 3 |
|**Vi**|120 | 350| 250| 300|

Preencha a matriz para descobrir a melhor combinação dentre os itens

i \ j  |0   | 1  | 2  | 3  | 4  |
 ----- |:--:|:--:|:--:|:--:|:--:|
**0**  |    |    |    |    |    |
**1**  |    |    |    |    |    |
**2**  |    |    |    |    |    |
**3**  |    |    |    |    |    |
**4**  |    |    |    |    |    |

???