substr関数は、PHPで文字列の一部を抽出するために使用されます。基本的な使い方から応用までをまとめます。

基本的な使い方
substr関数は、文字列から特定の部分を取り出すために使用されます。基本的なシンタックスは以下の通りです：

php
コードをコピーする
string substr ( string $string , int $start [, int $length ] )
$string：対象となる文字列。
$start：抽出を開始する位置。0から始まるインデックス。
$length：抽出する文字数。省略可能。
例
基本的な例
php
コードをコピーする
$text = "Hello, World!";
echo substr($text, 7); // "World!"
$textの7番目の位置（0から数える）から最後までの文字列を返します。

長さを指定する例
php
コードをコピーする
$text = "Hello, World!";
echo substr($text, 7, 5); // "World"
$textの7番目の位置から5文字分を抽出します。

負の値を使う例
php
コードをコピーする
$text = "Hello, World!";
echo substr($text, -6); // "World!"
負の値を使用すると、文字列の末尾から数えます。ここでは末尾から6文字目以降を抽出します。

負の長さを指定する例
php
コードをコピーする
$text = "Hello, World!";
echo substr($text, 7, -1); // "World"
開始位置を7とし、末尾から1文字目を除いた部分を抽出します。

特殊なケース
$startが文字列の長さを超える場合
php
コードをコピーする
$text = "Hello, World!";
echo substr($text, 20); // ""
開始位置が文字列の長さを超える場合、空の文字列を返します。

$lengthが0の場合
php
コードをコピーする
$text = "Hello, World!";
echo substr($text, 0, 0); // ""
長さが0の場合、空の文字列を返します。

$lengthが負の値で$startが負の値の場合
php
コードをコピーする
$text = "Hello, World!";
echo substr($text, -5, -1); // "orld"
$startが負の値で、$lengthも負の値の場合、末尾から指定した範囲の文字列を返します。

マルチバイト文字列の場合
日本語などのマルチバイト文字を扱う場合、mb_substr関数を使用します。mb_substrはマルチバイト文字列を正しく処理できます。

php
コードをコピーする
$text = "こんにちは、世界！";
echo mb_substr($text, 3, 2); // "にち"
mb_substrのシンタックスはsubstrと同様ですが、文字エンコーディングを指定することも可能です：

php
コードをコピーする
$text = "こんにちは、世界！";
echo mb_substr($text, 3, 2, 'UTF-8'); // "にち"
