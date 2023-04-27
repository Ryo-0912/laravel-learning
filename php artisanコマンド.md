## php artisan tinker
[参考URL](https://e-seventh.com/laravel-tinker-ease-develop/)

**tinker** で値を調べたいときは、名前空間から記述する。

例1
```
<?php

namespace App\Utils;

class Hello
{
    public static function sayHello()
    {
        return 'hello';
    }
}
```
```
>>> App\Utils\Hello::sayHello();
=> "hello"

```

例2
```
>>> Carbon\Carbon::now();
=> Carbon\Carbon @1622869129 {#3404
     date: 2021-06-05 04:58:49.710927 UTC (+00:00),
   }
```
