# str_replace

```
str_replace(A, B, C);

A : 文字列Aで指定した文字列が文字列Cにないか探す

B : 文字列CにAが見つかったとき、その文字列をBに変換する

C : 置き換え対象となる文字列
```

例1

```
$s = str_replace('-', '', "94748-58490-939");

echo $s; // 結果: 9474858490939
```

例2
```
$result = str_replace('a', 'b', 'banana');

echo $result; // 結果: 'bbnbnb'
```
