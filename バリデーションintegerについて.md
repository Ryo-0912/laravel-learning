Requestファイル内で、バリデーションを行うとき、次のようにバリデーションを設定しているとする。

GetDetailMessageTemplateRequest

```
<?php

declare(strict_types=1);

namespace App\Http\Requests\Api;

use Illuminate\Foundation\Http\FormRequest;

class GetDetailMessageTemplateRequest extends FormRequest
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
        return [
            'message_template_id' => 'required|integer',
        ];
    }
}
```

この時、integerとしてバリデーションを通過しているのだから、Controller内でもintegerとして認識されるはずと思っていたが、そうではなかった。

MessageTemplateController

```
<?php

declare(strict_types=1);

namespace App\Http\Controllers\Api;

use App\Http\Requests\Api\RegisterMessageTemplateRequest;
use App\Http\Requests\Api\UpdateMessageTemplateRequest;
use App\Http\Requests\Api\UpdateMessageTemplateSettingRequest;
use App\Http\UseCases\GetMessageTemplateListUseCase;
use App\Http\UseCases\RegisterMessageTemplateUseCase;
use App\Http\UseCases\UpdateMessageTemplateSettingUseCase;
use App\Http\UseCases\UpdateMessageTemplateUseCase;
use App\Dto\UseCase\UpdateMessageTemplateDto;
use Illuminate\Http\JsonResponse;

class MessageTemplateController extends ApiBaseController
{
    /**
     * Get Message Template List API
     * @author Kabir Gaire
     * @param GetMessageTemplateListUseCase $getMessageTemplateListUseCase
     * @return JsonResponse
     */
    public function getMessageTemplateList(GetMessageTemplateListUseCase $getMessageTemplateListUseCase): JsonResponse
    {
        return response()->json($getMessageTemplateListUseCase(
            $this->user()->id,
            (int) $this->user()->template_registration_flag
        ));
    }

    /**
     * Update Message Template Setting API
     * @author Kabir Gaire
     * @param UpdateMessageTemplateSettingUseCase $updateMessageTemplateSettingUseCase
     * @param UpdateMessageTemplateSettingRequest $request
     * @return JsonResponse
     */
    public function updateSetting(UpdateMessageTemplateSettingUseCase $updateMessageTemplateSettingUseCase, UpdateMessageTemplateSettingRequest $request): JsonResponse
    {
        return response()->json($updateMessageTemplateSettingUseCase(
            $this->user()->id,
            $request
        ));
    }

    /**
     * Register Message Template API
     *
     * @author Sayaka Kamei
     * @param RegisterMessageTemplateUseCase $registerMessageTemplateUseCase
     * @param RegisterMessageTemplateRequest $request
     * @return JsonResponse
     */
    public function registerMessageTemplate(RegisterMessageTemplateUseCase $registerMessageTemplateUseCase, RegisterMessageTemplateRequest $request): JsonResponse
    {
        return response()->json($registerMessageTemplateUseCase($request->message_template));
    }

    /**
     * Update Message Template API
     *
     * @author Sayaka Kamei
     * @param UpdateMessageTemplateUseCase $updateMessageTemplateUseCase
     * @param UpdateMessageTemplateRequest $request
     * @return JsonResponse
     */
    public function update(UpdateMessageTemplateUseCase $updateMessageTemplateUseCase, UpdateMessageTemplateRequest $request): JsonResponse
    {
        $updateMessageTemplateDto = new UpdateMessageTemplateDto($request);

        return response()->json($updateMessageTemplateUseCase($updateMessageTemplateDto));
    }
}

```

GetDetailMessageTemplateUseCase

```
<?php

declare(strict_types=1);

namespace App\Http\UseCases;

use App\Repositories\MessageTemplate\MessageTemplateRepositoryInterface;
use App\Exceptions\NoDataException;

class GetDetailMessageTemplateUseCase extends BaseUseCase
{
    private $messageTemplateRepository;

    public function __construct(
        MessageTemplateRepositoryInterface $messageTemplateRepository,
    ) {
        $this->messageTemplateRepository = $messageTemplateRepository;
    }

    /**
     * Get Detail Message Template API
     * @author Ryo Ando
     * @param int $messageTemplateId
     * @return mixed
     */
    public function __invoke(int $messageTemplateId): array       
    {
        $messageTemplate = $this->messageTemplateRepository->findMessageTemplate($messageTemplateId);

        if(!$messageTemplate)
        {
            throw new NoDataException();
        }

        return [
            'id' => $messageTemplate->id,
            'user_id' => $messageTemplate->user_id,
            'message_template' => $messageTemplate->message_template,
        ];
    }
}

```

このような記述をしており、クエリストリングでリクエストを投げると、次のようなエラーが出た。

```
local.ERROR: App\Http\UseCases\GetDetailMessageTemplateUseCase::__invoke(): Argument #1 ($messageTemplateId) must be of type int, string given, called in /usr/share/nginx/html/app/Http/Controllers/Api/MessageTemplateController.php on line 80 {"userId":2,"exception":"[object] (TypeError(code: 0): App\\Http\\UseCases\\GetDetailMessageTemplateUseCase::__invoke(): Argument #1 ($messageTemplateId) must be of type int, string given, called in /usr/share/nginx/html/app/Http/Controllers/Api/MessageTemplateController.php on line 80 at /usr/share/nginx/html/app/Http/UseCases/GetDetailMessageTemplateUseCase.php:26)
[stacktrace]
```

個人的には、バリデーション通過しているのに、なぜstringとして認識されているの？と思ったが、バリデーションのintegerの仕様を正しく認識していなかった。

***Laravelバリデーションのintegerは、該当パラメータが整数で構成された文字列か数値であるかをチェックしている***

したがって、本来クエリストリングでリクエストを送っているので、Requestではstringとして認識しているが、バリデーションintegerはstring数字でもバリデーションを通過する。
