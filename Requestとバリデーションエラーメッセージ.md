バリデーションはRequestクラスで行う。

次のような感じ。

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class ConfirmOrderRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {

        if ($this->send_selecter === "2") {
            return [
                'saved_address' => 'required|integer'
            ];
        }

        if ($this->send_selecter === "3") {
            return [
                'delivery_zip' => 'required|regex:/^\d{3}-?\d{4}$/',
                'delivery_pref' => 'required|integer',
                'delivery_address' => 'required|string',
                'delivery_address2' => 'required|string',
                'save_address_flag' => 'nullable',
                'save_address_title' => 'nullable|required_if:save_address_flag,1|string'
            ];
        }

        return ['send_selecter' => 'required|integer|between:1,3'];

    }
}
```

バリデーションを条件分けしたいときは、required_if を使う。

バリデーションエラーメッセージを表示させるときはvalidation.phpに記述。

```php
<?php

return [

                         　　　　　　　　 **略**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    'regex' => ':attribute を適切な形式で入力してください.',
    'required' => ':attribute を入力してください',
    'required_if' => ':otherを:valueするときは、:attributeを入力してください',
   
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                                       略
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    'custom' => [
        'attribute-name' => [
            'rule-name' => 'custom-message',
        ],
    ],

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                                       略
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    'attributes' => [
		'email' => 'メールアドレス',
		'password' => 'パスワード',
        'company_name' => '会社名',
        'old_password' => '元のパスワード',
        'new_password' => '新規パスワード',
        'delivery_zip' => '郵便番号',
        'delivery_pref' => '都道府県',
        'delivery_address' => '住所1',
        'delivery_address2' => '住所2',
        'save_address_title' => '保存名',
        'save_address_flag' => '新規納品先住所',
        'saved_address' => '登録済み住所'
    ],

    'values' => [
        'save_address_flag' => [
            '1' => '保存'
        ],
    ],
];
```

required_if を用いたときのバリデーションエラーメッセージを表示の仕方。

```php
'save_address_title' => 'nullable|required_if:save_address_flag,1|string'
```

このようなとき、まず、validation.phpのエラー文の型の:attributeには’save_address_title’が割り当てられ、:otherには’save_address_flag’が割り当てられる。

:valueには、required_ifの2つ目の引数1が入る。

この1を日本語に割り当てたいときは、validation.php内でvaluesのように記述する。
