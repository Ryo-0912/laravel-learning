# array_values

連想配列からvalueだけを取得/配列=>連想配列にする

例1　連想配列からvalueだけを取得

```
<?php
$fruits = ["apple" => "りんご", "orange" => "みかん", "lemon" => "れもん"];

$result = array_values($fruits);
print_r($result);
?>

#=>
Array
(
  [0] => りんご
  [1] => みかん
  [2] => れもん
)
```

例2 配列=>連想配列にする

```
<?php
$fruits = ["りんご",  "みかん", "",  "メロン"];

// 配列内の不要要素を削除する
$fruits =array_filter($fruits);
print_r($fruits);

// 添字を振り直す
$result = array_values($fruits);
print_r($result);
?>

#=>
Array
(
  [0] => りんご
  [1] => みかん
  [3] => メロン
)

Array
(
  [0] => りんご
  [1] => みかん
  [2] => メロン
)
```

# array_keys


