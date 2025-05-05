# Trabalho 1

## Placar e pontuação de um jogo de boliche

O boliche é um esporte praticado com uma bola pesada e tem como objetivo lançar a bola por uma pista, derrubar 10 pinos do lado oposto da pista dispostos em formação triangular (https://www.infoescola.com/esportes/boliche/).

A fórmula da contagem de pontos no boliche tem as seguintes variáveis (https://boliche.com.br/esporte-boliche/contagem-dos-pontos-no-boliche/):
* Os pontos são a soma dos pinos derrubados.
* Exceto quando fizer Strike (derrubar 10 pinos na 1.ª bola) ou Spare (derrubar 10 pinos nas duas bolas jogadas).
* Se fizer Strike ganha bônus nas 2 bolas jogadas a seguir. Se fizer Spare ganha bônus na próxima bola jogada. O bônus é igual ao número de pinos derrubados.
* O total de 1 partida pode variar de zero a 300 pontos.

A pontuação pode ir de zero (quando nenhum pino é derrubado nas dez jogadas ou “frames”) até o máximo possível de 300 pontos, ou seja, 12 “strikes” consecutivos. Supostamente, como cada partida tem 10 “frames” (jogadas), só seriam possíveis 10 “strikes”. Porém, se o jogador derrubar todos os pinos no primeiro arremesso do 10.º “frame”, tem o direito de jogar mais duas bolas, podendo completar 12 “strikes” numa mesma linha.

Faça um programa que leia a quantidade de pinos derrubados por um praticante de boliche em cada jogada e imprima:
* A sequência de pinos derrubados (de acordo com os exemplos de entrada e saída e as anotações de contagem de pontos - https://boliche.com.br/esporte-boliche/contagem-dos-pontos-no-boliche/).
* A pontuação final do jogador.

Dica: Para testar seu programa, sugere-se utilizar o seguinte simulador de pontos:
* https://www.bowlinggenius.com/

Exemplos de entradas e saídas:

**Exemplo 1:**

Entrada: 1 4 4 5 6 4 5 5 10 0 1 7 3 6 4 10 2 8 6

Saída: 1 4 | 4 5 | 6 / | 5 / | X _ | 0 1 | 7 / | 6 / | X _ | 2 / 6 | 133

**Exemplo 2:**

Entrada: 10 10 10 10 10 10 10 10 10 10 10 10

Saída: X _ | X _ | X _ | X _ | X _ | X _ | X _ | X _ | X _ | X X X | 300

## Código
```haskell
main = do
    entrada <- leitura 10
    putStrLn(concat(formata_placar entrada))
    putStrLn $ show $ soma_pontuacao entrada 0
    
-- FUNÇÕES PARA A LEITURA
leitura :: Int -> IO [(Int, Int)]
leitura 0 = return []
leitura n = do
    jogada1_str <- getLine
    let jogada1 = read jogada1_str :: Int 
    jogada2 <- verifica_jogada1 jogada1
    resto <- leitura(n-1)
    return ((jogada1, jogada2) : resto)
    
verifica_jogada1 :: Int -> IO Int
verifica_jogada1 10 = return (-1)
verifica_jogada1 n = do
    jogada2_str <- getLine
    let jogada2 = read jogada2_str :: Int 
    return (verifica_jogada2 n jogada2)
    
verifica_jogada2 :: Int -> Int -> Int
verifica_jogada2 n m
    | n + m == 10 = (-2)
    | otherwise = m
    
-- FUNÇÕES PARA PRINTAR O PLACAR
formata_tupla :: (Int, Int) -> String
formata_tupla (a, b)
    | b == -1 = "X _ | "
    | b == -2 = show a ++ " / | "
    | otherwise = show a ++ " " ++ show b ++ " | "
    
formata_placar :: [(Int, Int)] -> [String]
formata_placar tuplas = map formata_tupla tuplas
```

```haskell
main = do
    entrada <- leitura 9
    print (concat (map formata_frame entrada))
    print (pontuacao entrada)
    
data Frame = Strike | Spare Int | Open Int Int | Last Int Int (Maybe) deriving Show

leitura :: Int -> IO [Frame]
leitura 0 = return []
leitura n = do
    jogada1_str <- getLine
    let jogada1 = read jogada1_str :: Int 
    frame1 <- verifica_jogada1 jogada1
    resto <- leitura(n-1)
    return (frame1 : resto)

verifica_jogada1 :: Int -> IO Frame
verifica_jogada1 10 = return Strike
verifica_jogada1 n = do
    jogada2_str <- getLine
    let jogada2 = read jogada2_str
    return (verifica_jogada2 n jogada2)
    
verifica_jogada2 :: Int -> Int -> Frame
verifica_jogada2 n jogada2
    | n + jogada2 == 10 = Spare n
    | otherwise = Open n jogada2
    
pontuacao :: [Frame] -> Int
pontuacao [] = 0
pontuacao (Strike:xs) = 10 + bonus_strike xs + pontuacao xs
pontuacao (Spare a:xs) = 10 + bonus_spare xs + pontuacao xs
pontuacao (Open a b:xs) = a + b + pontuacao xs

bonus_strike :: [Frame] -> Int
bonus_strike [] = 0
bonus_strike (Strike:Strike:_) = 20
bonus_strike (Strike:Spare a:_) = 10 + a
bonus_strike (Strike:Open a b:_) = 10 + a
bonus_strike (Spare a:_) = 10
bonus_strike (Open a b:_) = a + b
bonus_strike _ = 0

bonus_spare :: [Frame] -> Int
bonus_spare [] = 0
bonus_spare [Strike] = 0
bonus_spare [Spare a] = 0
bonus_spare [Open a b] = 0
bonus_spare (Strike:_) = 10
bonus_spare (Spare a:_) = a
bonus_spare (Open a b:_) = a

formata_frame :: Frame -> String
formata_frame Strike = "X _ | "
formata_frame (Spare a) = show a ++ " / | "
formata_frame (Open a b) = show a ++ " " ++ show b ++ " | "
```
codigo funcionando
```haskell
main = do
    entrada <- leitura 10
    putStrLn (concat (map formata_frame entrada))
    putStrLn $ show $ pontuacao entrada
    
data Frame = Strike | Spare Int | Open Int Int | Last Int Int (Maybe Int) deriving Show

leitura :: Int -> IO [Frame]
leitura 0 = return []
leitura 1 = do
    jogada1_str <- getLine
    let jogada1 = read jogada1_str :: Int
    jogada2_str <- getLine
    let jogada2 = read jogada2_str :: Int
    if (jogada1 + jogada2 == 10 || jogada1 == 10) then do
        jogada3_str <- getLine
        let jogada3 = read jogada3_str :: Int
        return [Last jogada1 jogada2 (Just jogada3)]
    else
        return [Last jogada1 jogada2 Nothing]
leitura n = do
    jogada1_str <- getLine
    let jogada1 = read jogada1_str :: Int
    if jogada1 == 10 then do
        resto <- leitura(n-1)
        return (Strike: resto)
    else do
        jogada2_str <- getLine
        let jogada2 = read jogada2_str :: Int
        if jogada1 + jogada2 == 10 then do
            resto <- leitura(n-1)
            return (Spare jogada1: resto)
        else do
            resto <- leitura(n-1)
            return (Open jogada1 jogada2: resto)
    
pontuacao :: [Frame] -> Int
pontuacao [] = 0
pontuacao (Strike:xs) = 10 + bonus_strike xs + pontuacao xs
pontuacao (Spare a:xs) = 10 + bonus_spare xs + pontuacao xs
pontuacao (Open a b:xs) = a + b + pontuacao xs
pontuacao (Last a b (Just c):_) = a + b + c
pontuacao (Last a b Nothing:_) = a + b

bonus_strike :: [Frame] -> Int
bonus_strike [] = 0
bonus_strike (Strike:Strike:_) = 20
bonus_strike (Strike:Spare a:_) = 10 + a
bonus_strike (Strike:Open a b:_) = 10 + a
bonus_strike (Strike:Last a b _:_) = 10 + a
bonus_strike (Spare a:_) = 10
bonus_strike (Open a b:_) = a + b
bonus_strike (Last a b _:_) = a + b
bonus_strike _ = 0

bonus_spare :: [Frame] -> Int
bonus_spare [] = 0
bonus_spare [Strike] = 0
bonus_spare [Spare a] = 0
bonus_spare [Open a b] = 0
bonus_spare (Strike:_) = 10
bonus_spare (Spare a:_) = a
bonus_spare (Open a b:_) = a

formata_frame :: Frame -> String
formata_frame Strike = "X _ | "
formata_frame (Spare a) = show a ++ " / | "
formata_frame (Open a b) = show a ++ " " ++ show b ++ " | "
formata_frame (Last 10 10 (Just 10)) = "X X X | "
formata_frame (Last 10 10 (Just c)) = "X X " ++ show c ++ " | "
formata_frame (Last 10 b (Just c)) = "X " ++ show b ++ " " ++ show c ++ " | "
formata_frame (Last a b (Just c)) = show a ++ " / " ++ show c ++ " |"
formata_frame (Last a b Nothing) = show a ++ " " ++ show b ++ " | "
```
\/\/\/\/\/ 100% runcodes \/\/\/\/\/
```haskell
main = do
    -- A entrada do usuário é dada a partir de uma string,
    -- por isso usamos getLine para ler a entrada
    linha <- getLine
    
    -- lista_pinos é uma lista de inteiros, que recebe a entrada
    -- do usuário convertida para uma lista de inteiros.
    -- A conversão é feita da seguinte maneira:
    --  1 - Usamos a função word para converter uma string, dividida
    --      por espaços, em uma lista de strings
    --  2 - Usamos a função map para aplicar a função read em
    --      cada elemento da lista de strings, convertendo
    --      para uma lista de inteiros
    let lista_pinos = map read (words linha) :: [Int]
    
    -- entrada é uma lista de Frame, que recebe a lista de inteiros
    -- convertida em lista de Frame, a partir da função converte_frames
    let entrada = converte_frames lista_pinos
    
    -- Para printar o placar, seguimos os seguinte algoritmo:
    --  1 - Usamos a função map para aplicarmos a formatação de frame
    --      para todos os frames da lista entrada.
    --  2 - Concatenamos todos os elementos da lista.
    --  3 - Printamos.
    putStr $ concat $ map formata_frame entrada
    
    -- Para printar a pontuação, apenas aplicamos a função pontuacao
    -- na lista de frames e printamos o resultado
    putStrLn $ show $ pontuacao entrada
    
-- Tipo Frame, podendo ser um Strike, Spare, Open ou Last
data Frame = Strike | Spare Int | Open Int Int | Last Int Int (Maybe Int) deriving Show


converte_frames :: [Int] -> [Frame]
converte_frames [] = []
converte_frames [a, b, c] = [Last a b (Just c)]
converte_frames [a, b] = [Last a b Nothing]
converte_frames (x:y:xs)
    | x == 10 = Strike: converte_frames (y:xs)
    | x + y == 10 = Spare x: converte_frames xs
    | otherwise = Open x y: converte_frames xs
    
pontuacao :: [Frame] -> Int
pontuacao [] = 0
pontuacao (Strike:xs) = 10 + bonus_strike xs + pontuacao xs
pontuacao (Spare a:xs) = 10 + bonus_spare xs + pontuacao xs
pontuacao (Open a b:xs) = a + b + pontuacao xs
pontuacao (Last a b (Just c):_) = a + b + c
pontuacao (Last a b Nothing:_) = a + b

bonus_strike :: [Frame] -> Int
bonus_strike [] = 0
bonus_strike (Strike:Strike:_) = 20
bonus_strike (Strike:Spare a:_) = 10 + a
bonus_strike (Strike:Open a b:_) = 10 + a
bonus_strike (Strike:Last a b _:_) = 10 + a
bonus_strike (Spare a:_) = 10
bonus_strike (Open a b:_) = a + b
bonus_strike (Last a b _:_) = a + b
bonus_strike _ = 0

bonus_spare :: [Frame] -> Int
bonus_spare [] = 0
bonus_spare (Strike:_) = 10
bonus_spare (Spare a:_) = a
bonus_spare (Open a b:_) = a
bonus_spare [Last a b (Just c)] = a
bonus_spare [Last a b Nothing] = a
bonus_spare _ = 0

formata_frame :: Frame -> String
formata_frame Strike = "X _ | "
formata_frame (Spare a) = show a ++ " / | "
formata_frame (Open a b) = show a ++ " " ++ show b ++ " | "
formata_frame (Last 10 10 (Just 10)) = "X X X | "
formata_frame (Last 10 10 (Just c)) = "X X " ++ show c ++ " | "
formata_frame (Last 10 b (Just 10)) 
    | b == 0 = "X " ++ show b ++ " / | "
    | otherwise = "X " ++ show b ++ " X | "
formata_frame (Last 10 b (Just c))
    | b + c == 10 = "X " ++ show b ++ " / | "
    | otherwise = "X " ++ show b ++ " " ++ show c ++ " | "
formata_frame (Last a b (Just c)) 
    | c == 10 = show a ++ " / X | "
    | otherwise = show a ++ " / " ++ show c ++ " | "
formata_frame (Last a b Nothing) = show a ++ " " ++ show b ++ " | "
```
