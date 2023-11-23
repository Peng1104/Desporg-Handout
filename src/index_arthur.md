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

$$V(i, j) = \begin{cases} \textcolor{red}{V(i-1, j)}, & \textcolor{red}{Se}{ }\textcolor{red}{W_i > j} \\ \max(V(i-1, j), V(i-1, j-W_i) + Vi), & \text{Se } W_i \leq j \end{cases}$$

Quando o peso do item em questão ultrapassa a capacidade intermediária considerada **(Wi > j)**, devemos retornar a última combinação de itens, sem o item **i**, ou seja, **i-1** e com a mesma capacidade intermediária **j**. Assim, ficamos com todos os itens dessa mochila anteriores ao item **i**.


$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\\textcolor{red}{\max(V(i-1, j), V(i-1, j-W_i) + Vi),} & \textcolor{red}{Se  W_i \leq j}\end{cases}$$

Quando o peso do item em questão não ultrapassa a capacidade intermediária considerada **(Wi <= j)**, podemos adicioná-lo. Assim, temos que checar qual possibilidade nos traz um maior valor máximo, quando incluímos o item **i** ou quando não o fazemos.

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(\textcolor{blue}{V(i-1, j)}, \textcolor{green}{V(i-1, j-W_i) + Vi}), & \text{Se } W_i \leq j \end{cases}$$

Quando <span style="color:blue">não incluímos </span> o item **i**, consideramos o maior valor da combinação imediatamente antes de adicionar **i**, ou seja, **i-1** e com a mesma capacidade **j**. Assim, <span style="color:blue">V(i-1 , j)</span>.

Quando <span style="color:green">incluímos</span> o item **i**, consideramos o maior valor da combinação imediatamente antes de adicionar **i**, ou seja, **i-1**, e com uma capacidade menor, pois, ao adicionar o item **i** a capacidade restante da mochila diminui, ou seja, **j-Wi**.

Encontrando o maior valor dessa combinação, como estamos adicionando o item **i**, adicionamos o seu valor **Vi**.

Assim, <span style="color:green">V(i-1 , j-Wi) + Vi</span>.

Dessa forma, o cenário que trouxer um maior valor entre <span style="color:blue">V(i-1 , j)</span> e <span style="color:green">V(i-1 , j-Wi) + Vi</span> será retornado.


A matriz da recursão
---------

Para entender melhor o funcionamento do algoritmo, podemos utilizar uma matriz para representar a recursão. Assim, cada linha da matriz representa um item **i** e cada coluna representa uma capacidade intermediária **j** (pode ser interpretada como uma versão menor da mochila).
Os valores a serem preenchidos na matriz são os valores máximos **V** para cada combinação de itens e capacidades intermediárias. 

Por exemplo, a posição `md (3,2)` será preenchida pelo resultado de `md V(3, 2)`.

i \ j  |0   | 1  | 2  | 3  | 4  | 5  |
 ----- |:--:|:--:|:--:|:--:|:--:|:--:|
**0**  |    |    |    |    |    |    |
**1**  |    |    |    |    |    |    |
**2**  |    |    |    |    |    |    |
**3**  |    |    |    |    |    |    |
**4**  |    |    |    |    |    |    |

??? Checkpoint

A posição destacada em vermelho na matriz deve ser preenchida por qual valor?

:checkpoint1

::: Gabarito

A posição destacada em vermelho está na linha 4 e na coluna 1, ou seja, posição `md (4,1)` da matriz. Assim, para preencher essa posição, devemos calcular `md V(4,1)`.
:::

???


Na linha `md i = 1`, estamos considerando um cenário em que apenas o `md item 1` pode ser alocado na mochila. Já na linha `md i = 2`, estamos considerando um cenário em que apenas os `md itens 1 e 2` podem ser alocados na mochila. E assim por diante. Dessa forma, a **solução** para o problema será encontrada na **última linha** da matriz, onde todos os itens podem ser alocados na mochila.

O mesmo raciocínio é feito nas colunas. Na coluna `md j = 1`, estamos considerando um cenário em que temos uma mochila menor de capacidade 1. Já na coluna `md j = 2`, estamos considerando um cenário em que a mochila menor tem capacidade 2. E assim por diante. Dessa forma, a **solução** para o problema será encontrada na **última coluna** da matriz, onde o a capacidade da mochila menor se iguala a capacidade total da mochila real.

??? Checkpoint

Para um conjunto de `md 2 itens`e uma mochila de capacidade `md C = 3`, qual a posição na matriz que contém  a solução?

:checkpoint2

::: Gabarito

A posição que calcula o valor máximo para 2 itens e uma mochila de capacidade 3 é a posição `md (2,3)`.


:gabcheckpoint2

A solução sempre estará na última linha e na última coluna da matriz.

:::

???

??? Exercício 1

Considerando uma mochila de capacidade `md C = 5` e os seguintes itens:

|**item** |1 |2 |3 |4 |
|--|--|--|--|--|
|**Wi**|3 | 1| 4| 2 |
|**Vi**|8 | 3| 10| 7|


O que podemos afirmar sobre os valores da coluna `md j = 0` e da linha `md i = 0`?

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

Essa é a base da recursão.

:::

???

Veja essa animação para entender como a matriz executa o algoritmo:

:matrix

??? Exercício 2

Considere os seguintes itens de 1 a 4 com diferentes pesos e valores. Considere uma mochila com capacidade `md C = 4`.

|**item** |1 |2 |3 |4 |
|--|--|--|--|
|**Wi**|1 | 4| 2| 3 |
|**Vi**|120 | 350| 250| 300|

Preencha a posição destacada da matriz utilizando a recursão.

:ex2

::: Gabarito

:gabex2

:::

???

??? Exercício 3


|**item** |1 |2 |3 |4 |
|--|--|--|--|
|**Wi**|1 | 4| 2| 3 |
|**Vi**|120 | 350| 250| 300|

Preencha a última posição referente à soução do problema.

:ex3

::: Gabarito

:gabex3

:::

???