O Problema da Mochila
======

Introdução
---------

O problema da mochila é um desafio fundamental de otimização combinatória.  Ele apresenta uma questão simples, porém complexa: Dado um conjunto de itens, cada um com um peso e um valor, como podemos maximizar o valor total desses itens em uma mochila sem exceder seu limite de peso?

Formalização do Problema
---------

Imagine que em uma distante terra de conflitos, um jovem soldado chamado Lucas estava se preparando para uma missão crítica. Ele tinha à sua disposição uma mochila militar, mas que podia carregar no máximo 40kg. Antes de partir, Lucas enfrentou um dilema estratégico: escolher entre uma variedade de itens essenciais, cada um com seu próprio peso e valor. O valor de cada item representava sua importância para a sobrevivência e o sucesso na missão.

**Itens Disponíveis:**

1. Item 1: Um Kit de Primeiros Socorros, valor {green}(500), mas pesando {red}(30kg).

2. Item 2: Ração de Combate (MRE), valor {green}(200), com peso de {red}(10kg).

3. Item 3: Dispositivo de Comunicação, valor {green}(300), pesando {red}(20kg).

4. Item 4: Pistola Silenciosa, valor {green}(400), pesando {red}(25kg).

5. Item 5: Equipamento de Camuflagem, valor {green}(350), com peso de {red}(15kg).

Lucas sabia que o valor de cada item representava o quão crucial ele poderia ser para a sua sobrevivência. No entanto, ele também compreendia que não era apenas uma questão de escolher o item mais valioso, pois isso poderia limitar sua capacidade de levar outros itens potencialmente úteis.

Resolvendo manualmente o problema:
---------

Voltando à história do soldado Lucas, ele enfrentava o desafio de escolher entre vários itens valiosos, cada um com seu próprio peso. A mochila de Lucas podia carregar até 40kg, e ele tinha os seguintes itens disponíveis:

| Itens | Valor | Peso |
|-------|-------|------|
| 1     |  500  |  30  |
| 2     |  200  |  10  |
| 3     |  300  |  20  |
| 4     |  400  |  25  |
| 5     |  350  |  15  |

??? Desafio para Lucas:
Se você fosse Lucas, enfrentando uma missão crítica, quais itens você escolheria para garantir sua sobrevivência, maximizando o valor dentro do limite de peso da mochila?

!!! Dica
Considere a importância de cada item para a missão e o peso que cada um adiciona à mochila. O objetivo é alcançar o máximo valor sem exceder o limite de peso de 40kg.
!!!

::: Solução:
* Lucas percebeu que escolher o item de maior valor individual (item 1) consumiria a maior parte da capacidade de sua mochila, deixando pouco espaço para outros itens essenciais. Portanto, ele precisava encontrar uma combinação de itens que oferecesse o maior valor agregado sem ultrapassar o limite de peso.

* Após analisar as opções, Lucas concluiu que a melhor combinação seria levar os itens 4 e 5. Juntos, esses itens somam um valor total de 750 e pesam exatamente 40kg (25kg + 15kg), maximizando assim o valor que ele poderia levar sem exceder a capacidade da mochila. Esta escolha lhe proporcionou um equilíbrio ideal entre poder de defesa, equipamento de sobrevivência e capacidade de camuflagem.
:::
???

Resolução
---------

Para chegar a conclusão de quais os itens a serem adicionados precisamos fazer um processo recursivo para que todas as opções sejam exploradas.

Mas antes de realizar a recursão precisamos compreender quais as situações que podem ocorrem durante o preenchimento da mochila. Uma vez com todas as situações definidas podemos identificar quais são as condições de parada e quais são valores que devem ser retornados pela recursão.

Primeiramente vamos analisar qual é a interação entre um item e a mochila?

??? Quais são as possiveis interações entre um item e a mochila?
:::
Há apenas duas opções, uma de **adicionar** o item na mochila e outra de **não** adicionar ele.
:::
???

Após definir as opções que temos para cada item temos que analisar se existe algum fator que obriga a escolha de uma opção.

??? Existe algum cenário que força a inclusão ou exclusão do item?
:::
Sim existe um cenário que força **não** adicionar o item, este cenário acontece quando o peso do item é maior que o espaço disponivel na mochila.
:::
???

Além de cada situação para cada item temos tambem o cenário global, ou seja, situações que afetam a interação de todos os itens com a mochila.

??? Quais cenários globais temos?
:::
Apenas dois novamente.
um sendo o cenário em que **não há mais espaço na mochila** para adicionar os itens, e o outro sendo quando **não há mais itens válidos para adicionar.**
:::
???

Feito isso podemos esperessar essas conclusões na seguinte formula matemática:

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > j \\ \max(V(i-1, j), V(i-1, j-W_i) + Vi), & \text{Se } W_i \leq j \end{cases}$$

Onde $V(i, j)$ calcula o valor máximo para a combinação da quantidade `md i` de itens e `md j` de espaço disponível na mochila.

A Recursão
---------

Antes de poder criar a função de recursão precisamos de uma estrutura que identifica cada item dentro da mochila e como vimos cada item possui um peso e um valor. Portanto temos a estrutura de um Item como:

``` c
typedef struct {
    int weight;
    int value;
} Item;
```

onde `md weight` é o peso do item e `md value` é o valor do item.

Como vimos temos duas opções para cada item, a de **não** adicionar

??? Como calcular o valor da mochila sem adicionar um item?
:::
Para poder saber o valor da mochila **sem** um item precisamos apenas "pular o seu indice" na recursão e calcular o valor da mochila com os outros itens. `md V(i-1, j)`, ou seja, diminuir o total de itens.
:::
???

e a de adicionar.

??? E para adicionar o item?
:::
Para poder saber o valor da mochila **com** o item precisamos calcular o valor de uma mochila intermediaria que possui um espaço igual ao espaço original (`md j`) menos o volume ocupado pelo item (`md j-Wi`) e somar este resultado com o valor do item. `md V(i-1, j-Wi) + Vi`. Ou seja, diminuir o total de itens (`md i-1`), diminuir o espaço disponível pelo espaço ocupado pelo item (`md j-Wi`) e somar seu valor (`md +Vi`).
:::
???

Uma vez definido como a recursão vai acontecer temos implementar as condições que levam aos cenários.

Como foi visto, o primeiro cénario identificado foi o de não haver espaço para adicionar um item, portanto, devemos verificar se há espaço disponivel para adicionar o item, ou seja, verificar se o peso do item é menor ou igual ao espaço disponivel na mochila.

??? O que significa isso matematicamente?
:::
$$\text{Se } W_i \leq j$$
:::
???

Caso contrario, ou seja, estamos no cénario identificado, a resposta é trivial, não tem espaço para item portanto apenas existe a opção de não adicionar ele a mochila.

``` c
if (peso_do_item > espaco_disponivel) {
    return sem;
}
```

??? O que significa isso matematicamente?
:::
$$V(i, j) = V(i-1, j), \text{Se } W_i > j$$
:::
???

Se for o caso de haver espaço precisamos descobrir se vale a pena adicionar o item. Para isso precisamos primeiro calcular qual seria o valor da mochila com o item.

Para calcular o valor da mochila com o item precisamos criar uma sub mochila que possui como seu espaço disponivel o espaço da mochila atual menos o peso do item $j-W_i$, o que repesenta o volume que seria ocupado pelo item, calcular seu valor e somar este resultado com o valor do item $V_i$.

``` c
int com = V(lista_de_itens, quantidade_de_itens_restantes - 1, espaco_disponivel - peso_do_item) + valor_do_item;
```

Feito isso precisamos calcular o valor da mochila sem o item $V(i-1, j)$.

``` c
int sem = V(lista_de_itens, quantidade_de_itens_restantes - 1, espaco_disponivel);
```

E depois de calcular o valor da mochila sem o item comparamos os valores e escolhemos o maior entre eles, já que o objetivo é maximizar o valor da mochila. $\text{max}(V(i-1, j), V(i-1, j-W_i) + Vi)$.

``` c
if (sem > com) {
    return sem;
}
return com;
```

Além disso temos os cénarios relacionados a todos os itens e a mochila em si. Que são as condições de parada. 
O primeiro é quando não há mais itens para adicionar na mochila, ou seja, $i = 0$.

``` c
if (quantidade_de_itens_restantes == 0) {
    return 0;
}
```

E o segundo é quando não há mais espaço na mochila, ou seja, $j = 0$.

``` c
if (espaco_disponivel == 0) {
    return 0;
}
```

??? Porque o valor retornado é 0 nas condições de parada?
:::
O valor zero representa que não há nenhum valor sendo adicionado a mochila.
:::
???

??? Como seria a função juntando os codigos acima?
:::
``` c
int V(Item *lista_de_itens, int quantidade_de_itens_restantes, int espaco_disponivel) {
    if (quantidade_de_itens_restantes == 0) {
        return 0;
    }
    if (espaco_disponivel == 0) {
        return 0;
    }
    int sem = V(lista_de_itens, quantidade_de_itens_restantes - 1, espaco_disponivel);

    int peso_do_item = lista_de_itens[quantidade_de_itens_restantes - 1].weight;

    if (peso_do_item > espaco_disponivel) {
        return sem;
    }
    int valor_do_item = lista_de_itens[quantidade_de_itens_restantes - 1].value;

    int com = V(lista_de_itens, quantidade_de_itens_restantes - 1, espaco_disponivel - peso_do_item) + valor_do_item;

    if (sem > com) {
        return sem;
    }
    return com;
}
```
:::
???

A matriz da recursão
---------

Para entender melhor o funcionamento do algoritmo, podemos utilizar uma matriz para representar a recursão. 

Na linha `md i = 1`, estamos considerando um cenário em que apenas o `md item 1` pode ser alocado na mochila. Já na linha `md i = 2`, estamos considerando um cenário em que apenas os `md itens 1 e 2` podem ser alocados na mochila. E assim por diante até a última linha, em que o conjunto completo de itens pode ser alocado na mochila.

O mesmo raciocínio é feito nas colunas. Na coluna `md j = 1`, estamos considerando um cenário em que temos uma mochila menor de capacidade 1. Já na coluna `md j = 2`, estamos considerando um cenário em que a mochila menor tem capacidade 2. E assim por diante até a última coluna, em que a capacidade da mochila menor se iguala a capacidade total da mochila real.

??? Checkpoint

Para um conjunto de `md 2 itens`e uma mochila de capacidade `md C = 3`, qual a posição na matriz que contém  a solução?

:checkpoint2

::: Gabarito

A posição que calcula o valor máximo para 2 itens e uma mochila de capacidade 3 é a posição `md (2,3)`.


:gabcheckpoint2

A solução sempre estará na última linha e na última coluna da matriz.

:::

???

Agora, vamos aplicar as condições de parada na matriz.

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

Agora vamos fixar o entendimento da matriz em dois exercícios.


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

Preencha a última posição referente à solução do problema.

:ex3

::: Gabarito

:gabex3

:::

???

