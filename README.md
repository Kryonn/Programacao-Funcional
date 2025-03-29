# Programação Funcional - SSC0960-2025102

Programação funcional é um paradigma de programação baseado em funções puras, tendo como principais características a imutabilidade e a ausência de efeitos colaterais.

## Classes de tipos

Essa categoria se refere a um conjunto de tipos que possuem uma característica em comum.
* Eq - Tipos que suportam igualdade ou desigualdade
* Ord - Tipos ordenáveis
* Num - Tipos numéricos
* Show - Tipos que podem ser convertidos para string

## Funções

### Função soma

Recebe uma lista de números e retorna a soma dos elementos da lista.
```haskell
soma :: Num a => [a] -> a
soma [] = 0
soma (x:xs) = x + soma xs
```

### Função filtra

Recebe uma condição, uma lista e retorna os valores da lista que satifazem a condição.
```haskell
filtra :: (a -> Bool) -> [a] -> [a]
filtra _ [] = []
filtra teste (x:xs)
    | teste x = x:filtra teste xs
    | otherwise = filtra teste xs
```

### Função mapa

Recebe uma função, uma lista e retorna a função aplicada em todos os elementos da lista.
```haskell
mapa :: (a -> b) -> [a] -> [b]
mapa _ [] = []
mapa f (x:xs) = f x:mapa f xs
```

### Função pertence

Recebe um número, uma lista e retorna um valor booleano que indica se o número está na lista.
```haskell
pertence :: (Eq a) => a -> [a] -> Bool
pertence _ [] = False
pertence e (x:xs)
    | x == e = True
    | otherwise = pertence e xs
```

## Atividades

### Atividade 1

Faça um programa em Haskell que leia três inteiros não negativos, um em cada linha, e se forem correspondentes aos lados de um triângulo, imprima a área utilizando a fórmula de Heron. Caso não seja, imprima “-”. 

Exemplo 1: entrada: 9 5 11, saída: 22.18529918662356

Exemplo 2: entrada: 6 7 13, saída: 0.0

Exemplo 3: entrada: 2 1 10, saída: -

<details>
  <summary>🔍 Clique para ver o código</summary>
  
    main = do
        la <- getLine
        let a = read la
        lb <- getLine
        let b = read lb
        lc <- getLine
        let c = read lc
        putStrLn $ area a b c
        
    valida a b c
        | (a <= b + c) && (b <= a + c) && (c <= a + b) = 1
        | otherwise = 0
        
    area a b c
        | valida a b c == 1 = show $ sqrt (p * (p-a) * (p-b) * (p-c))
        | otherwise = "-"
            where
                p = (a+b+c) / 2


</details>




