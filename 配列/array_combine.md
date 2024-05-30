# array_combine

2つの配列を結合して1つの連想配列（キーと値のペアの配列）を作成する。

最初の配列がキーとして使われ、2番目の配列がそのキーに対応する値として使われる。

キーワード : 連想配列

```
<?php
  // 配列
  $array1 = [ "a", "b", "c" ];
  $array2 = [ "1", "2", "3" ];

  // 実行
  $result = array_combine($array1, $array2) ;

  // 返り値
  print_r( $result ) ;
?>
```

```
Array
(
    [a] => 1
    [b] => 2
    [c] => 3
)
```

array_combineを使わないで、同じ連想配列を作ろうとすると、次のようになる。

```
<?php
  $array1 = ["a", "b", "c"];
  $array2 = ["1", "2", "3"];
  
  $combinedArray = [];
  for ($i = 0; $i < count($array1); $i++) {
      $combinedArray[$array1[$i]] = $array2[$i];
  }
  
  print_r($combinedArray);
?>
```
