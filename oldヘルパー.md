## oldヘルパーの仕組み
[参照URL](https://www.larajapan.com/2020/09/25/old%E3%83%98%E3%83%AB%E3%83%91%E3%83%BC/)

内容を簡単に言うと、セッションに```_old_input```というキーに値が入って、そこの値を読み取ることがoldヘルパーが機能する。

oldヘルパーが機能しない例として、javascriptなどでページ遷移することなどがある。これをoldヘルパーを使って、値を保持するには、例えば以下のようにする。

## view
```
<tr>
　　<td class="row-2 pc"><input class="form-control" id="pc-last-kanji" type="text" name="last_name_kanji" value="{{old('last_name_kanji', $session['last_name_kanji'])}}" placeholder="太宰" maxlength="20" required></td>
　　<td class="row-2"><input class="form-control" type="text" name="first_name_kanji" value="{{old('first_name_kanji', $session['first_name_kanji'])}}" placeholder="太郎" maxlength="20" required></td>
</tr>
```

## controller
```
public function showProfileForm()
{
   $session = session()->get('main');
　　return view('web.teacher.register.register-private', compact('session'));
}

public function storeProfile(ProfileFormRequest $request)
{
  $request->session()->put('main', $request->only([
    'last_name_kanji', 'first_name_kanji', 'last_name_kana', 'first_name_kana',
    'zip_code', 'prefecture_cd', 'city', 'street_number', 'building_name',
    'phone', 'bank_name', 'branch_name', 'account_type', 'account_name', 'account_number',
  ]));

  return redirect(route('web.app-profile.form'));
}
```

