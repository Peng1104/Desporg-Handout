O Problema da Mochila
======

Introdução
---------

O problema da mochila é um desafio fundamental de otimização combinatória.  Ele apresenta uma questão simples, porém complexa: Dado um conjunto de itens, cada um com um peso e um valor, como podemos maximizar o valor total desses itens em uma mochila sem exceder seu limite de peso?

Formalização do Problema
---------

Imagine que você tem uma mochila que pode carregar um peso máximo de W. Você também tem um conjunto de itens, cada um com seu próprio peso e valor. O objetivo é determinar a combinação mais valiosa de itens que pode caber na mochila sem ultrapassar o limite de peso.

Tipos de Problemas da Mochila:
---------

Existem algumas variações do problema da mochila, cada uma com suas próprias características e restrições. As três mais comuns são:

1. Mochila 0/1 (binária): Você ou leva um item ou o deixa. Não há meio-termo.

2. Mochila Fracionária: Você pode levar frações de um item.

3. Mochila Ilimitada: Você pode levar um número ilimitado de cada item

{red}(Neste Handout iremos nos concentrar na mochila binária.)

Exemplo (Mochila 0/1):
---------

Imagine que você é um soldado indo para a guerra e tem uma mochila que pode carregar até 40kg. Você tem os seguintes itens disponíveis para colocar na mochila:

| Items | Valor | Peso |
|-------|-------|------|
| 1     |  500  |  30  |
| 2     |  200  |  10  |
| 3     |  300  |  20  |
| 4     |  400  |  25  |
| 5     |  350  |  15  |

??? Exercício
Qual combinação de itens você deve levar para maximizar o valor total?

!!! Aviso
Neste exemplo quanto maior for o valor maior é sua chance de sobrevivência.
!!!

::: Gabarito
Apesar de parecer intuitivo levar o item de maior valor, isso não necessariamente resulta na melhor solução. Neste caso, a melhor solução é levar os itens 4 e 5, que resulta em um valor total de 750.
:::