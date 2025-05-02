# Programação Funcional - SSC0960

Programação funcional é um paradigma de programação baseado no uso de funções puras, ou seja, funções que sempre retornam o mesmo resultado para os mesmos argumentos e não causam efeitos colaterais. Entre suas principais características estão a imutabilidade, onde os dados não podem ser alterados após sua criação, e a ausência de efeitos colaterais, o que facilita a previsibilidade e a testabilidade do código.

Além disso, esse paradigma valoriza o uso de funções de ordem superior, recursão no lugar de laços tradicionais, e o tratamento de funções como valores de primeira classe, ou seja, podendo ser passadas como argumentos, retornadas por outras funções e armazenadas em variáveis.

A programação funcional promove um estilo de desenvolvimento mais declarativo, onde se descreve o que deve ser feito, em vez de como fazer, contribuindo para um código mais conciso, modular e fácil de manter.

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

### Atividade 2

Faça um programa em Haskell que leia dois inteiros não negativos, um em cada linha, e imprima o número de inteiros defeituosos, perfeitos e abundantes entre esses números, um em cada linha.

 * Número Perfeito: A soma dos divisores próprios é igual ao número.
 * Número Abundante: A soma dos divisores próprios é maior que o número.
 * Número Defeituoso: A soma dos divisores próprios é menor que o número.

<details>
  <summary>🔍 Clique para ver o código</summary>
  
    main = do
        n1_ <- getLine
        n2_ <- getLine
        let n1 = read n1_
        let n2 = read n2_
        let l1 = lista n1 n2 
        putStrLn $ show $ conta (<0) $ compara (mapa soma (mapa divisores l1)) l1
        putStrLn $ show $ conta (==0) $ compara (mapa soma (mapa divisores l1)) l1
        putStrLn $ show $ conta (>0) $ compara (mapa soma (mapa divisores l1)) l1
    
    
    lista :: Int -> Int -> [Int]
    lista a b = [a..b]
    
    soma :: (Num a) => [a] -> a
    soma [] = 0
    soma (x:xs) = x + soma xs
    
    divisores :: Int -> [Int]
    divisores n = [x | x <- [1..n-1], n `mod` x == 0]
    
    mapa :: (a -> b) -> [a] -> [b]
    mapa _ [] = []
    mapa f (x:xs) = f x: mapa f xs
    
    compara :: (Eq a, Ord a, Num a) => [a] -> [a] -> [a]
    compara [] [] = []
    compara (x:xs) (y:ys)
        | x == y = 0: comp
        | x > y = 1: comp
        | otherwise = -1: comp
            where
                comp = compara xs ys
        
    conta :: (Num a) => (a -> Bool) -> [a] -> a
    conta _ [] = 0
    conta cond (x:xs)
        | cond x = 1 + conta cond xs
        | otherwise = conta cond xs


</details>

### Atividade 3

Faça um programa em Haskell, utilizando os conceitos de programação do paradigma funcional, que leia dois inteiros x e y, sendo que x é menor que y, e imprima o comprimento do maior intervalo entre dois números primos consecutivos maiores ou iguais a x e menores ou iguais a y.

<details>
  <summary>🔍 Clique para ver o código</summary>
  
    import Data.Maybe (fromMaybe)

    main = do
        n1_ <- getLine
        n2_ <- getLine
        let n1 = read n1_
        let n2 = read n2_
        
        -- A função fromMaybe permite retornar 0 ao inves de Nothing
        putStrLn $ show $ fromMaybe 0 $ pegaMaior $ diferenca $ pegaEqto (< n2) $ ignoraEqto (< n1) primos 
        
    -- Lista de todos os primos existentes
    primos = crivo [2..]
        where
            crivo (x:xs) = x:(crivo $ filtra ((/=0).(`mod` x)) xs)
    
    -- A função diferenca recebe uma lista e
    -- devolve uma lista com a diferença entre
    -- números consecutivos
    diferenca :: (Num a) => [a] -> [a]
    diferenca [] = []
    diferenca [_] = []
    diferenca (x:y:zs) = (y-x):diferenca (y:zs)
    
    -- A função pegaMaior recebe uma lista e
    -- devolve o maior elemento entre eles.
    -- A função Maybe funciona como um if.
    -- A função maximum pega o maior elemento da lista
    pegaMaior :: (Ord a) => [a] -> Maybe a
    pegaMaior [] = Nothing
    pegaMaior xs = Just (maximum xs)
    
    -- A função filtra recebe uma lista e
    -- devolve uma lista com os elementos
    -- que satisfazem a condição
    filtra :: (a->Bool) -> [a] -> [a]
    filtra _ [] = []
    filtra cond (x:xs)
        | cond x = x:filtra cond xs
        | otherwise = filtra cond xs
    
    -- A função pegaEqto recebe uma lista e
    -- devolve uma lista com os elementos
    -- que satisfazem a condição
    pegaEqto :: (a->Bool) -> [a] -> [a]
    pegaEqto _ [] = []
    pegaEqto t (x:xs)
        | t x = x:pegaEqto t xs
        | otherwise = []
    
    -- A função ignoraEqto recebe uma lista e
    -- devolve uma lista com os elementos
    -- que satisfazem a condição
    ignoraEqto :: (a->Bool) -> [a] -> [a]
    ignoraEqto _ [] = []
    ignoraEqto t (x:xs)
        | t x = ignoraEqto t xs
        | otherwise = x:xs



</details>






