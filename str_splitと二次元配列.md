
# str_split 文字列を一文字ずつ分割して、一つの配列とする
```
$alpha = "ABCDEFGHIJKLMN";

$alphaArray = str_split($alpha) // ["A","B","C","D","E","F","G","H","I","J","K","L","M","N"]

echo $$alphaArray[7]."\n";
#=> H
echo gettype($$alphaArray)."\n";
#=> Array
```

# 二次元配列

```
$H = 5;

for ($h = 0; $h < $H; $h++) {
    $Fields[] = $$alphaArray;
}

print_r($Fields);

#=>

Array
(
    [0] => Array
        (
            [0] => A
            [1] => B
            [2] => C
            [3] => D
            [4] => E
            [5] => F
            [6] => G
            [7] => H
            [8] => I
            [9] => J
            [10] => K
            [11] => L
            [12] => M
            [13] => N
        )

    [1] => Array
        (
            [0] => A
            [1] => B
            [2] => C
            [3] => D
            [4] => E
            [5] => F
            [6] => G
            [7] => H
            [8] => I
            [9] => J
            [10] => K
            [11] => L
            [12] => M
            [13] => N
        )

    [2] => Array
        (
            [0] => A
            [1] => B
            [2] => C
            [3] => D
            [4] => E
            [5] => F
            [6] => G
            [7] => H
            [8] => I
            [9] => J
            [10] => K
            [11] => L
            [12] => M
            [13] => N
        )

    [3] => Array
        (
            [0] => A
            [1] => B
            [2] => C
            [3] => D
            [4] => E
            [5] => F
            [6] => G
            [7] => H
            [8] => I
            [9] => J
            [10] => K
            [11] => L
            [12] => M
            [13] => N
        )

    [4] => Array
        (
            [0] => A
            [1] => B
            [2] => C
            [3] => D
            [4] => E
            [5] => F
            [6] => G
            [7] => H
            [8] => I
            [9] => J
            [10] => K
            [11] => L
            [12] => M
            [13] => N
        )

)
H

print_r($Fields[0][7]);

#=> H
```
