# Programação Funcional - SSC0960-2025102

Programação funcional é um paradigma de programação baseado em funções puras, tendo como principais características a imutabilidade e a ausência de efeitos colaterais.

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

