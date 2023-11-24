O Problema da Mochila
======

Introdução
---------

O problema da mochila é um desafio fundamental de otimização combinatória.  Ele apresenta uma questão simples, porém complexa: Dado um conjunto de itens, cada um com um peso e um valor, como podemos maximizar o valor total desses itens em uma mochila sem exceder seu limite de peso?

Formalização do Problema
---------

Imagine que em uma distante terra de conflitos, um jovem soldado chamado Lucas estava se preparando para uma missão crítica. Ele tinha à sua disposição uma mochila militar, mas que podia carregar no máximo 40kg. Antes de partir, Lucas enfrentou um dilema estratégico: escolher entre uma variedade de itens essenciais, cada um com seu próprio peso e valor. O valor de cada item representava sua importância para a sobrevivência e o sucesso na missão.

**Itens Disponíveis:**

1. Item 1: Um kit de primeiros socorros, valor {green}(500), mas pesando {red}(30kg).

2. Item 2: Alimentos compactos, valor {green}(200), com peso de {red}(10kg).

3. Item 3: Um dispositivo de comunicação, valor {green}(300), pesando {red}(20kg).

4. Item 4: Armas e munições, valor {green}(400), pesando {red}(25kg).

5. Item 5: Equipamento de camuflagem, valor {green}(350), com peso de {red}(15kg).

Lucas sabia que o valor de cada item representava o quão crucial ele poderia ser para a sua sobrevivência. No entanto, ele também compreendia que não era apenas uma questão de escolher o item mais valioso, pois isso poderia limitar sua capacidade de levar outros itens potencialmente úteis.

??? Exercício
Se você fosse Lucas, quais extrategias você utilizaria para maximizar o valor total?
???

Se você foi sagaz, já deve ter percebido que você poderia levar metade dos alimentos compactos e metade das armas e munições ou qualquer outra fração de cada item. O que nos leva a uma questão importante: **podemos levar frações de itens ou devemos escolher entre levar um item ou não?**

Existem multiplas variações do problema da mochila, mas a mais comum é a mochila binária, {red}(e de fato, é a que iremos abordar neste handout). Nesta variação, a escolha é binária - você ou leva um item inteiro ou não leva nada dele. É um cenário de "tudo ou nada", sem a possibilidade de levar apenas uma parte de um item. Para Lucas, isso significaria escolher entre levar ou deixar cada item essencial, sem a opção de dividir o peso ou o valor.

Lucas, portanto, tinha que tomar decisões claras sobre quais itens levar, considerando o valor e o peso de cada um, sem a possibilidade de dividir os itens. Esta abordagem reflete o dilema real enfrentado por ele - escolher o conjunto mais valioso de itens completos que caberiam em sua mochila de 40kg, maximizando assim suas chances de sucesso na missão.

Resolvendo o problema:
---------

Voltando à história do soldado Lucas, ele enfrentava o desafio de escolher entre vários itens valiosos, cada um com seu próprio peso. A mochila de Lucas podia carregar até 40kg, e ele tinha os seguintes itens disponíveis:

| Items | Valor | Peso |
|-------|-------|------|
| 1     |  500  |  30  |
| 2     |  200  |  10  |
| 3     |  300  |  20  |
| 4     |  400  |  25  |
| 5     |  350  |  15  |

**Desafio para Lucas:**
Se você fosse Lucas, enfrentando uma missão crítica, quais itens você escolheria para garantir sua sobrevivência, maximizando o valor dentro do limite de peso da mochila?

!!! Dica
Considere a importância de cada item para a missão e o peso que cada um adiciona à mochila. O objetivo é alcançar o máximo valor sem exceder o limite de peso de 40kg.
!!!

::: Solução:
* Lucas percebeu que escolher o item de maior valor individual (Item 1) consumiria a maior parte da capacidade de sua mochila, deixando pouco espaço para outros itens essenciais. Portanto, ele precisava encontrar uma combinação de itens que oferecesse o maior valor agregado sem ultrapassar o limite de peso.



* Após analisar as opções, Lucas concluiu que a melhor combinação seria levar os itens 4 e 5. Juntos, esses itens somam um valor total de 750 e pesam exatamente 40kg (25kg + 15kg), maximizando assim o valor que ele poderia levar sem exceder a capacidade da mochila. Esta escolha lhe proporcionou um equilíbrio ideal entre poder de defesa, equipamento de sobrevivência e capacidade de camuflagem.
:::