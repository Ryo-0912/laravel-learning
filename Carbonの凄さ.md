** 年齢の取得 **

次のように、ageプロパティを使うことで、Carbonで取得した日付から現在までの年数を計算してくれる。

```
> $carbon = new Carbon\Carbon('2017-01-01 12:30:30');
= Carbon\Carbon @1483241430 {#6540
    date: 2017-01-01 12:30:30.0 Asia/Tokyo (+09:00),
  }

> $carbon->age;
= 7
```
