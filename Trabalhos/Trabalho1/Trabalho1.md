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
