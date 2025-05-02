# Aula 1 - Conceitos básicos

## Haskell

Para o aprendizado de programação funcional, será utilizada a linguagem Haskell. Essa linguagem foi escolhida porque é fortemente baseada em funções puras, o que favorece o desenvolvimento de uma compreensão sólida dos princípios funcionais.
Mas afinal, o que é uma função pura?
Uma função pura é aquela que não causa efeitos colaterais e que sempre retorna o mesmo resultado para os mesmos valores de entrada. Ou seja, seu comportamento é totalmente previsível e independente do contexto externo.

## Funções da Prelude

Prelude é uma biblioteca que é importada implicitamente, possuindo diversas funções úteis que podem ser usadas sem ser implementadas.

Aqui estão algumas funções básicas dessa biblioteca:

### main

Função principal, onde se executa todas as outras funções.

```haskell
main = do
  putStrLn "Hello World"
```

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

Exemplo de uso:
```haskell
-- Lê a entrada do usuário e coloca em a
a <- getLine

-- Converte a string a em tipo que será utilizado
let la = read a
```

## Funções do usuário

### Estrutura das funções

Em haskell, também é possível criar suas próprias funções. Em geral, elas seguirão o seguinte padrão:
```
nome_da_função (parametro1) (parametro2) ...
  operação e retorno do resultado
```

Exemplo:
```haskell
-- sinal é uma função que recebe um número x e retorna o sinal dele
sinal x
-- Uso de guardas, que será mostrado mais a frente
    | x < 0 = -1
    | x == 0 = 0
    | otherwise = 1 
```

### Guardas

Guardas são uma estrutura condicional utilizada em Haskell que se assemelha, conceitualmente, ao if de linguagens como C. No entanto, elas oferecem uma forma mais clara, legível e elegante de tratar múltiplas condições. Sempre que precisamos selecionar um valor com base em condições específicas, podemos utilizar guardas.

Mas surge a pergunta: por que não usar simplesmente if?
A resposta está na legibilidade e organização. Embora o if seja útil em expressões simples, seu uso se torna confuso quando há várias ramificações. Guardas permitem expressar essas situações de forma mais natural, próxima da lógica matemática, evitando o aninhamento excessivo de if-else.
Além disso, guardas se integram bem com a sintaxe declarativa de Haskell, favorecendo um estilo de programação mais limpo, funcional e de fácil manutenção.

No exemplo anterior, por exemplo, o uso de guardas permite com que a função retorne:

* -1, caso `x < 0`
* 0, caso `x == 0`
* 1, caso não seja nenhum caso anterior.

### Exemplos de implementação de funções:

#### resultado

Essa função recebe um número que representa a nota de um aluno e retorna uma string que representa o status dele na matéria.
```haskell
resultado x
    | x >= 5 = "Aprovado"
    | x > 3 = "Recuperação"
    | otherwise = "Reprovado"
```

#### absoluto

Essa função recebe um número e retorna o valor absoluto dele.
```haskell
absoluto x
    | x < 0 = -x
    | otherwise = x
```

#### bhaskara

Essa função recebe três valores que representam uma função de segunda grau e retorna as raízes da função.

```haskell
bhakara a b c
    | delta < 0 = [] -- [] é uma lista vazia
    | delta == 0 = [x] -- [x] é uma lista com um elemento
    | otherwise = [x1,x2] -- [x1, x2] é uma lista com dois elementos
-- where usado para atribuir um "rótulo" a uma determinada expressão. Sendo usado para deixar o código mais legível
    where
        delta = b * b - 4 * a * c
        x = - b / (2 * a)
        x1 = (- b - sqrt delta)/ (2 * a)
        x2 = (- b + sqrt delta)/ (2 * a)
```

### Código completo

Abaixo temos um código completo em haskell, onde são executadas todas as funções aprendidas. Altere as entradas das funções para testar!

###### obs: '$' substitui o parênteses.

```haskell
main = do
    putStrLn $ show $ sinal (-1)
    putStrLn $ resultado 2
    putStrLn $ show $ absoluto 2
    putStrLn $ show $ bhakara 1 4 4
    
sinal x
    | x < 0 = -1
    | x == 0 = 0
    | otherwise = 1
    
resultado x
    | x >= 5 = "Aprovado"
    | x > 3 = "Recuperação"
    | otherwise = "Reprovado"
    
absoluto x
    | x < 0 = -x
    | otherwise = x
    
bhakara a b c
    | delta < 0 = []
    | delta == 0 = [x]
    | otherwise = [x1,x2]
    where
        delta = b * b - 4 * a * c
        x = - b / (2 * a)
        x1 = (- b - sqrt delta)/ (2 * a)
        x2 = (- b + sqrt delta)/ (2 * a)
```









