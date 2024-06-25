# array_chunk

```
array_chunk(対象となる配列, 分割する要素のサイズ, bool $preserve_keys = false)
```

bool $preserve_keys　=> true : 元配列のキーを維持

bool $preserve_keys　=> false : キーを新たに割り振る

例

```
$input_array = [1, 2, 3, 4, 5, 6, 7, 8, 9];
$chunked_array = array_chunk($input_array, 3);

print_r($chunked_array);
```

```
Array
(
    [0] => Array
        (
            [0] => 1
            [1] => 2
            [2] => 3
        )

    [1] => Array
        (
            [0] => 4
            [1] => 5
            [2] => 6
        )

    [2] => Array
        (
            [0] => 7
            [1] => 8
            [2] => 9
        )
)
```

例2

```
$input_array = [1 => 'a', 2 => 'b', 3 => 'c', 4 => 'd', 5 => 'e'];
$chunked_array = array_chunk($input_array, 2, true);

print_r($chunked_array);
```

```
Array
(
    [0] => Array
        (
            [1] => a
            [2] => b
        )

    [1] => Array
        (
            [3] => c
            [4] => d
        )

    [2] => Array
        (
            [5] => e
        )
)
```
