
# str_split 文字列を一文字ずつ分割して、一つの配列とする
```
$pokemon = "POKEMON";

$pokemonArray = str_split($pokemon) // ["P","O","K","E","M","O","N"]

echo $pokemonArray[1]."\n";
#=> O
echo gettype($pokemonArray)."\n";
#=> Array
```

# 二次元配列

```
$H = 5;

for ($h = 0; $h < $H; $h++) {
    $Fields[] = $pokemonArray;
}

print_r($Fields);

#=>

Array
(
    [0] => Array
        (
            [0] => P
            [1] => O
            [2] => K
            [3] => E
            [4] => M
            [5] => O
            [6] => N
        )

    [1] => Array
        (
            [0] => P
            [1] => O
            [2] => K
            [3] => E
            [4] => M
            [5] => O
            [6] => N
        )

    [2] => Array
        (
            [0] => P
            [1] => O
            [2] => K
            [3] => E
            [4] => M
            [5] => O
            [6] => N
        )

    [3] => Array
        (
            [0] => P
            [1] => O
            [2] => K
            [3] => E
            [4] => M
            [5] => O
            [6] => N
        )

    [4] => Array
        (
            [0] => P
            [1] => O
            [2] => K
            [3] => E
            [4] => M
            [5] => O
            [6] => N
        )

)

print_r($Fields[0][1]);

#=> O
```
