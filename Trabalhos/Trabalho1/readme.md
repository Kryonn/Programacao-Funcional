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

## Resolução do Trabalho
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
    --      para todos os elementos da variável entrada.
    --  2 - Concatenamos todos os elementos da lista.
    --  3 - Printamos.
    putStr $ concat $ map formata_frame entrada
    
    -- Para printar a pontuação, apenas aplicamos a função pontuacao
    -- na lista de frames e printamos o resultado
    putStrLn $ show $ pontuacao entrada
 
 
    
-- Tipo Frame, podendo ser um Strike, Spare, Open ou Last
data Frame = Strike | Spare Int | Open Int Int | Last Int Int (Maybe Int) deriving Show



-- converte_frames é uma função que recebe um lista de inteiros,
-- correpondente a quantidade de pinos derrubados de cada jogada,
-- e retorna uma lista de frames.
converte_frames :: [Int] -> [Frame]
-- Lista vazia retorna lista vazia
converte_frames [] = []
-- Lista com três elementos retorna Last (Último frame)
converte_frames [a, b, c] = [Last a b (Just c)]
-- Lista com dois elementos retorna Last (Último frame)
converte_frames [a, b] = [Last a b Nothing]
-- Para listas com mais elementos:
converte_frames (x:y:xs)
    -- Se o primeiro elemento da lista for dez, então é um Strike
    | x == 10 = Strike: converte_frames (y:xs)
    -- Se os dois primeiros elementos soma dez, então é um Spare
    | x + y == 10 = Spare x: converte_frames xs
    -- Se não for nenhum dos casos anteriores, então é um Open(Jogada normal)
    | otherwise = Open x y: converte_frames xs



-- pontuação é uma função que soma a pontuação de todos os frames
-- e seus respectivos bônus(Strike ou Spare)
pontuacao :: [Frame] -> Int
-- Se for um Strike, precisamos somar dez, da jogada feita, mais o
-- bônus do Strike(pontuação duas jogadas posteriores), mais a chamada recursiva
-- do resto da lista de frames
pontuacao (Strike:xs) = 10 + bonus_strike xs + pontuacao xs

-- Se for um Spare, precisamos somar dez, da jogada feita, mais o
-- bônus do Spare(pontuação da jogada posterior), mais a chamada recursiva
-- do resto da lista de frames
pontuacao (Spare a:xs) = 10 + bonus_spare xs + pontuacao xs

-- Se for um Open, precisamos apenas somar a pontuação das duas jogadas
-- com a chamada recursiva do resto da lista de frames
pontuacao (Open a b:xs) = a + b + pontuacao xs

-- Se for um Last, precisamos apenas somar a pontuação de cada jogada
pontuacao [Last a b (Just c)] = a + b + c
pontuacao [Last a b Nothing] = a + b

-- Caso não caia em nenhum caso anterior, somamos 0
pontuacao _ = 0



-- bonus_strike é uma função que, ao receber uma lista de frames,
-- calcula o valor do bônus de um Strike
bonus_strike :: [Frame] -> Int

-- Se forem dois Strikes, então retornamos 20
bonus_strike (Strike:Strike:_) = 20

-- Se for um Strike e um Spare, então retornamos 10 mais a primeira jogada do Spare
bonus_strike (Strike:Spare a:_) = 10 + a

-- Se for um Strike e um Open, então retornamos 10 mais a primeira jogada do Open
bonus_strike (Strike:Open a b:_) = 10 + a

-- Se for um Strike e um Last, então retornamos 10 mais a primeira jogada do Last
bonus_strike [Strike, Last a _ _] = 10 + a

-- Se for um Spare, então retornamos 10
bonus_strike (Spare a:_) = 10

-- Se for um Open, retornamos a soma das duas jogadas
bonus_strike (Open a b:_) = a + b

-- Se for um Last, retornamos a soma das duas jogadas
bonus_strike [Last a b _] = a + b

-- Caso não caia em nenhum caso anterior, somamos 0
bonus_strike _ = 0



-- bonus_strike é uma função que, ao receber uma lista de frames,
-- calcula o valor do bônus de um Spare
bonus_spare :: [Frame] -> Int

-- Se for um Strike, então retornamos 10
bonus_spare (Strike:_) = 10

-- Se for um Spare, Open ou Last, retornamos o valorda primeira jogada
bonus_spare (Spare a:_) = a
bonus_spare (Open a b:_) = a
bonus_spare [Last a b (Just c)] = a
bonus_spare [Last a b Nothing] = a

-- Caso não caia em nenhum caso anterior, somamos 0
bonus_spare _ = 0



-- formata_frame é uma função que recebe um frame e formata no padrão do placar
formata_frame :: Frame -> String

-- Se forem Strike, Spare e Open, retornamos as seguinte strings:
formata_frame Strike = "X _ | "
formata_frame (Spare a) = show a ++ " / | "
formata_frame (Open a b) = show a ++ " " ++ show b ++ " | "

-- Se for Last, retornamos as seguintes strings:
formata_frame (Last 10 10 (Just 10)) = "X X X | "
formata_frame (Last 10 10 (Just c)) = "X X " ++ show c ++ " | "
formata_frame (Last 10 b (Just c))
    | b + c == 10 = "X " ++ show b ++ " / | "
    | otherwise = "X " ++ show b ++ " " ++ show c ++ " | "
formata_frame (Last a b (Just c)) 
    | c == 10 = show a ++ " / X | "
    | otherwise = show a ++ " / " ++ show c ++ " | "
formata_frame (Last a b Nothing) = show a ++ " " ++ show b ++ " | "
```
