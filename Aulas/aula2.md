# Aula 2

## Recursão

Em Haskell, não existem variáveis no sentido tradicional das linguagens imperativas. Todos os valores são imutáveis, ou seja, uma vez definidos, não podem ser alterados. Por isso, estruturas de repetição como for ou while, comuns em linguagens como C ou Python, não são utilizadas.

Mas então, como repetir ações ou percorrer elementos, como em uma função que soma todos os valores de uma lista de inteiros? Em Haskell, usamos recursão. Uma função recursiva é aquela que chama a si mesma para resolver partes menores de um problema, até alcançar um caso base que encerra a repetição.

Exemplo:
```haskell
soma [] = 0
soma (x:xs) = x + soma xs
```

No exemplo acima, temos uma função que recebe uma lista de elementos e retorna a soma dos elementos dela.

Mas como ela funciona? Basicamente, ela recebe uma lista e, caso seja vazia, retorna uma lista vazia, caso contenha elementos, 
