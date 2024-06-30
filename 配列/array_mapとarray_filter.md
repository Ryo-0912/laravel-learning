# array_map

```
array_map(コールバック関数, 対象配列);
```

# array_filter

```
$result = array_filter(対象配列, コールバック関数);
```

# array_map と array_filterの違い

- array_map

  対象配列と同じ要素数だけ返す
  
- array_filter

  コールバック関数でtrueとなるものを返す


例1 array_map

```
foreach ($list as $l) {
    $rang = array_map(fn($x) => ($x % $per_max == 0 ? $x : null), $l);
}
```

例2 array_filter

```
foreach ($list as $l) {
    $rang = array_filter($l, fn($x) => $x % $per_max == 0);
}
```

例2のように、array_mapを使って、```fn($x) => $x % $per_max == 0```を満たす値だけ返すことはできない。

なぜなら、array_mapは元の配列の要素数と同じ数だけ値を返すため。

array_filterは、```fn($x) => $x % $per_max == 0```を満たす値だけ返すことができる。
