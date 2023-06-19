formタグにbuttonタグを配置すると、typeの指定の仕方次第で、予期せぬところでformタグが送信されてしまう。

```
<form id='$form'>
  <div>
    <input id='$input' type='text' value='Hello, world!' />
  </div>
  <div>
    <button>ボタン</button>
    <button type='submit'>submit ボタン</button>
  </div>
</form>
```

**type = 'submit'**
formタグ内で、```type指定しない```or```type='submit'```と指定、該当タグをクリックすると、submitイベントが発火してしまう。
(上記コードのbuttonタグはともにクリックすると、送信処理を実行してしまう。)

formタグ内でbuttonタグをクリックしても、formタグの内容を送信したくない場合は、以下のようにbuttonタグのtype指定を変更する。
```
<form id='$form'>
  <div>
    <input id='$input' type='text' value='Hello, world!' />
  </div>
  <div>
    <button type='button'>button ボタン</button>
  </div>
</form>
```
