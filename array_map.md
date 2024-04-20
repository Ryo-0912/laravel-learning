# array_map

array_map(関数, 配列);

したがって、関数欄には自作関数でも良い

例

```
[$a,$b] = array_map('intval', ["12", "64"]);

print_r($a);

#=> 12
```
