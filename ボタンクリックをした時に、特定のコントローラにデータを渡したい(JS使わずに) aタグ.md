aタグを普通に使うと、そのままページ遷移するだけで、コントローラを通過しない。

そこで、aタグを使って、特定のコントローラにデータを渡して、ページ遷移させたい場合、次のように指定すると、実現できる

```
<a href="{{ action('HogeController@download', $data->id) }}"></a>
```

上のコードのように、第二引数にデータ渡すと、記述したコントローラ、メソッドにデータを渡すことができる。

例

view側

```
<form method="POST" action={!! route('zukans.zukan.destroy', ['videoId' => $video->id]) !!}
                                        class="d-flex justify-content-center align-items-center gap-2"
                                        accept-charset="UTF-8">
  <input name="_method" value="DELETE" type="hidden">
  {{ csrf_field() }}
  **<a href="{{ action('ZukanController@edit', $video->id) }}" class="btn btn-primary me-2"><i class="bi bi-pencil-square"></i> 編集</a>**
  <button type="submit" class="btn btn-danger" title="Delete Video"
     onclick="return confirm(&quot;本当に削除しますか？&quot;)" value="delete">
  <span class="bi bi-trash" aria-hidden="true"></span>
     削除
  </button>
</form>
```

ZukanController側

```
public function edit($video)
{
   $user = Auth::user();
   $milTicket = MilTicket::first();
   $video = Video::find((int)$video);
   $embedkey = $this->millvi->getVideoEmbedKey($video->mil_id_contents, $milTicket->mil_ticket);
   $categories = $this->videoCategoryRepository->getAllCategory();

   return view('zukan.edit', compact('video', 'user', 'embedkey', 'categories'));
}
```
