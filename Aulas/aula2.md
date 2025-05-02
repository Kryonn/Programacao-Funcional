# Aula 2 - Recursão e Tipagem de valores

## Recursão

Em Haskell, não existem variáveis no sentido tradicional das linguagens imperativas. Todos os valores são imutáveis, ou seja, uma vez definidos, não podem ser alterados. Por isso, estruturas de repetição como for ou while, comuns em linguagens como C ou Python, não são utilizadas.

Mas então, como repetir ações ou percorrer elementos, como em uma função que soma todos os valores de uma lista de inteiros? Em Haskell, usamos recursão. Uma função recursiva é aquela que chama a si mesma para resolver partes menores de um problema, até alcançar um caso base que encerra a repetição.

Exemplo:
```haskell
soma [] = 0
soma (x:xs) = x + soma xs
```

No exemplo acima, temos uma função que recebe uma lista de elementos e retorna a soma dos elementos dela.

Mas como ela funciona? Basicamente, ela recebe uma lista e separa em x, que é o primeiro elemento da lista (cabeça), e xs, que é o resto da lista (cauda). Após isso, verifica se é uma lista vazia, caso seja, retorna zero, caso não seja, retorna o valor da cabeça somado com o retorno da função soma aplicada na cauda.

### Mais funções recursivas

#### somaPos

Essa função recebe uma lista e retorna a soma dos elementos positivos dela.

```haskell
somaPos [] = 0
somaPos (x:xs)
    | x > 0 = x + somaPos xs
    | otherwise = somaPos xs
```

#### somaNeg

Essa função recebe uma lista e retorna a soma dos elementos negativos dela.

```haskell
somaNeg [] = 0
somaNeg (x:xs)
    | x < 0 = x + somaNeg xs
    | otherwise = somaNeg xs
```

#### somaPar

Essa função recebe uma lista e retorna a soma dos elementos pares dela.

```haskell
somaPar [] = 0
somaPar (x:xs)
    | mod x 2 == 0 = x + somaPar xs
    | otherwise = somaPar xs
```

## Generalização

Nos exemplos anteriores, foram implementadas três variações da função de soma de elementos, cada uma com sua própria condição específica para realizar a soma. No entanto, essas funções compartilham uma estrutura bastante semelhante, o que abre espaço para generalização.

Ao identificar esse padrão comum, podemos abstrair a lógica repetida e criar uma única função mais genérica, capaz de receber diferentes critérios como parâmetro. Dessa forma, evitamos repetição de código e tornamos a implementação mais concisa, reutilizável e elegante.

### Funções de alta ordem

Para generalizar funções de forma eficiente em Haskell, é essencial compreender um conceito fundamental: funções de alta ordem (também chamadas de funções de segunda ordem).

Essas funções são aquelas que recebem outras funções como parâmetros e/ou retornam funções como resultado. Em Haskell, esse conceito é amplamente utilizado, permitindo criar soluções mais flexíveis, reutilizáveis e expressivas.

Aqui temos um exemplo clássico de função de alta ordem:

#### filtra

Essa função recebe uma condição, uma lista e retorna uma lista com todos os elementos que satisfazem a condição.

```haskell
-- Independente da condição, se for uma lista vazia, retorna uma lista vazia.
filtra _ [] = []
-- Caso não seja uma lista vazia:
--    Se x satisfazer a condição (cond), então é concatenado com a chamada recursiva da cauda.
--    Senão, apenas retorna a chamada recursiva da cauda.
filtra cond (x:xs)
    | cond x = x: filtra cond xs
    | otherwise = filtra cond xs
```

Exemplo de uso da função filtra:
```haskell
putStrLn $ show $ filtra (> 0) [1, 2, -1, -5, 6]
-- Saída: [1, 2, 6]

putStrLn $ show $ filtra (< 0) [1, 2, -1, -5, 6]
-- Saída: [-1, -5]
```

Agora que temos a função filtra, podemos generalizar as funções de soma anteriores:

```haskell
main = do
-- Printa a soma dos valores da lista dos números positivos
    putStrLn $ show $ soma $ filtra (> 0) [1, 2, -1, -5, 6]

-- Printa a soma dos valores da lista dos números negativos
    putStrLn $ show $ soma $ filtra (< 0) [1, 2, -1, -5, 6]

-- Printa a soma dos valores da lista dos números pares
-- A condição utilizada se trata de uma função lambda, que recebe x e, caso mod x 2 == 0, retorna true
    putStrLn $ show $ soma $ filtra (\x -> x `mod` 2 == 0) [1, 2, -1, -5, 6] 
    
soma [] = 0
soma (x:xs) = x + soma xs

filtra _ [] = []
filtra cond (x:xs)
    | cond x = x: filtra cond xs
    | otherwise = filtra cond xs
```

Observe que, para fazer diferentes tipos de soma, precisamos da implementação de apenas duas funções.

## Tipos

Se tentarmos executar o seguinte código, o que aconteceria?

```haskell
main = do
    putStrLn $ show $ soma $ filtra (\x -> x * 10) [1, 2, -1, -5, 6]
    
soma [] = 0
soma (x:xs) = x + soma xs

filtra _ [] = []
filtra cond (x:xs)
    | cond x = x: filtra cond xs
    | otherwise = filtra cond xs
```

Exatamente. Isso resultaria em um erro de compilação, pois a função filtra espera receber como argumento uma função que retorne um valor booleano (ou seja, que indique se o elemento deve ser mantido ou não na lista). No entanto, foi passada uma função que apenas multiplica o valor por 10, retornando um inteiro em vez de um booleano — o que não atende ao tipo esperado por filtra.

Por conta disso, é uma boa prática sempre indicar o tipo dos operandos de uma operação.
Aqui alguns dos tipos mais usados:

* Int - inteiros menores (quatidade de bytes limitado).
* Integer - inteiros grandes (quantidade de bytes ilimitado).
* Float - números reais de precisão.
* Double - números reais de precisão dupla.
* Char - caracter único.
* Bool - booleano (true / false).
* String - lista de caracteres.
* [a] - lista de elementos do tipo a.
* (a, b) - tupla com dois elementos.

### Declaração do tipo

Para declarar os tipos dos valores que serão operados em uma função, seguiremos o seguinte padrão:
```
nome_da_função :: tipo_parâmetro1 -> tipo_parâmetro2 -> tipo_parâmetro3 ...
Declaração da função
```

Exemplos de aplicação da tipagem dos valores:

#### soma

```haskell
-- A função soma uma lista de inteiros e retorna uma valor inteiro
soma :: [Int] -> Int  
soma [] = 0
soma (x:xs) = x + soma xs
```

#### absoluto

```haskell
-- A função recebe um inteiro e retorna o valor absoluto dele
absoluto :: Int -> Int
absoluto x
    | x < 0 = -x
    | otherwise = x
```

#### resultado

```haskell
-- A função recebe um inteiro e retorna uma string
resultado :: Int -> String
resultado x
    | x >= 5 = "Aprovado"
    | x > 3 = "Recuperação"
    | otherwise = "Reprovado"
```

#### filtra

```haskell
-- A função recebe uma função booleana, uma lista de inteiros e retorna a lista com todos os valores que satisfizeram a condição
filtra :: (Int -> Bool) -> [Int] -> [Int]
filtra _ [] = []
filtra cond (x:xs)
    | cond x = x: filtra cond xs
    | otherwise = filtra cond xs
```

### Código completo

Abaixo temos um código completo em haskell, onde são executadas todas as funções aprendidas. Altere as entradas das funções para testar!

```haskell
main = do
-- Exemplos de uso da função filtra
    putStrLn $ show $ filtra (> 0) [1, 2, -1, -5, 6]
    putStrLn $ show $ filtra (< 0) [1, 2, -1, -5, 6]
    
-- Printa a soma dos valores da lista dos números positivos
    putStrLn $ show $ soma $ filtra (> 0) [1, 2, -1, -5, 6]

-- Printa a soma dos valores da lista dos números negativos
    putStrLn $ show $ soma $ filtra (< 0) [1, 2, -1, -5, 6]

-- Printa a soma dos valores da lista dos números pares
-- A condição utilizada se trata de uma função lambda, que recebe x e, caso mod x 2 == 0, retorna true
    putStrLn $ show $ soma $ filtra (\x -> x `mod` 2 == 0) [1, 2, -1, -5, 6]
    
-- Descomente a linha abaixo para ver se compila
--    putStrLn $ show $ soma $ filtra (\x -> x * 10) [1, 2, -1, -5, 6]

soma :: [Int] -> Int
soma [] = 0
soma (x:xs) = x + soma xs

filtra :: (Int -> Bool) -> [Int] -> [Int]
filtra _ [] = []
filtra cond (x:xs)
    | cond x = x: filtra cond xs
    | otherwise = filtra cond xs
```



