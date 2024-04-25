# array_diff

二つの配列が存在し、片方の配列から二つの配列で被っている要素を除くメソッド

例1

```
$a = [1,2,3,4,5];
$b = [6,7,1];

$result = array_diff($a, $b);

print_r($result);
#=>
Array
(
    [1] => 2
    [2] => 3
    [3] => 4
    [4] => 5
)
```

例2 配列の要素数の多い少ないは関係ない
```
$a = [1,2,3];
$b = [6,7,1,10,5];

$result = array_diff($a, $b);

print_r($result);
#=>
andouryou@andouryounoMacBook-Pro code_validation % php sort.php
Array
(
    [1] => 2
    [2] => 3
)
```
