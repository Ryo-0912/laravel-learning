`True/False`がreturnされる場合のデータ型は`**bool型**`

例

```
public function checkIfSameEmail(string $email): **bool**
{
　　return $this->model::where('email', $email)->exists();
   #=> true/false
}

public function checkIfSameEmailEdit(string $email): **bool**
{
　　return in_array($email, $this->model::where('email', '<>', $email)->get()->pluck('email')->toArray());
   #=> true/false
}
```
