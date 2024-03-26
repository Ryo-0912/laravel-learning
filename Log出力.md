1. use Illuminate\Support\Facades\Log;

2. あとは、確認したいところに、以下のように記述するだけ。

```
Log::info();
Log::error(); etc
```

上の1と2をまとめて次のように1行でもかける

```
\Log::info();
\Log::error(); etc
```
