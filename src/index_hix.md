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

$$V(i, j) = \begin{cases} V(i-1, j), & \text{Se } W_i > C \\ \max(V(i-1, j), V(i-1, j-W_i)), & \text{Se } W_i \leq C \end{cases}$$

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

Onde

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