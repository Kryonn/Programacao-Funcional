# Aula 1 - Conceitos básicos

## Haskell

Para o aprendizado de programação funcional, será utilizada a linguagem Haskell. Essa linguagem foi escolhida porque é fortemente baseada em funções puras, o que favorece o desenvolvimento de uma compreensão sólida dos princípios funcionais.
Mas afinal, o que é uma função pura?
Uma função pura é aquela que não causa efeitos colaterais e que sempre retorna o mesmo resultado para os mesmos valores de entrada. Ou seja, seu comportamento é totalmente previsível e independente do contexto externo.

## Funções da Prelude

Prelude é uma biblioteca que é importada implicitamente, possuindo diversas funções úteis que podem ser usadas sem ser implementadas. Aqui estão algumas funções básicas dessa biblioteca:

### putStrLn

Função que printa uma string.

Exemplo de uso:
```haskell
-- Printa a string "STRING"
putStrLn "STRING"

-- Saída: STRING
```

### show

Função que converte valores do tipo show em string.

Exemplo de uso:
```haskell
-- Printa a conversão do inteiro 1 para string "1"
putStrLn $ show $ 1

-- Saída: 1
```

### getLine

Função que faz a leitura da entrada do usuário.

Exemplo de uso:
```haskell
-- Lê a entrada do usuário e coloca em a
a <- getLine
```

### read

Função que converte string para valores do tipo que vai ser usado.

## 





