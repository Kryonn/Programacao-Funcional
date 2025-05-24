```haskell
data Dados = Dados {
    country :: String,
    confirmed :: Int,
    deaths :: Int,
    recovery :: Int,
    active :: Int
}deriving (Show, Read)

main = do
    input <- getLine
    let [n1, n2, n3, n4] = map read (words input) :: [Int]
    csv <- readFile "dados.csv"
    let linhas = lines csv
    putStrLn $ show $ sum $ map active $ filter ((>= n1).confirmed) $ map converte_string linhas
    putStrLn $ show $ sum $ map active $ filter ((>= n1).confirmed) $ map converte_string linhas
    
converte_string :: String -> Dados
converte_string str =
    let [country, confirmed, deaths, recovery, active] = splitComma str
    in Dados country (read confirmed) (read deaths) (read recovery) (read active) 
    
splitComma :: String -> [String]
splitComma [] = [""]
splitComma (c:cs)
    | c == ','  = "" : rest
    | otherwise = (c : head rest) : tail rest
  where
    rest = splitComma cs
```

    
