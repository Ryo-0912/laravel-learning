EventとListenerはセット 

イベントは、何らかの特定の状況やアクションが発生したことを表す。

リスナーは、特定のイベントが発生した時それを検知し、リスナー内に記述されている処理を実行する。

app/Events

```jsx
<?php

declare(strict_types=1);

namespace App\Events;

use App\Dto\EmailNotificationDto;
use App\Mail\SubscriptionPurchaseCompletedEmail;
use Illuminate\Broadcasting\InteractsWithSockets;
use Illuminate\Broadcasting\PrivateChannel;
use Illuminate\Foundation\Events\Dispatchable;
use Illuminate\Queue\SerializesModels;

class SubscriptionPurchaseCompleted
{
    use Dispatchable;
    use InteractsWithSockets;
    use SerializesModels;

    public EmailNotificationDto $emailNotificationDto;

    /**
     * Create a new event instance.
     *
     * @return void
     */
    
    __constructメソッドは記述しているクラスのインスタンス化するときに自動的に呼び出される。
    今回の場合、SubscriptionPurchaseCompletedがnewされた時に、__constructに定義されている情報をSubscriptionPurchaseCompletedインスタンスが保持することができる。
    public function __construct(string $email, string $userName, string $planName, string $planPrice, $startDate)
    {
        $emailNotificationDto = new EmailNotificationDto();
        $emailNotificationDto->to_email = $email;
        $emailNotificationDto->mailable = new SubscriptionPurchaseCompletedEmail($userName, $planName, $planPrice, $startDate);
        $this->emailNotificationDto = $emailNotificationDto;
    }

    /**
     * Get the channels the event should broadcast on.
     *
     * @return \Illuminate\Broadcasting\Channel|array
     */
    
    broadcastOn メソッドは、イベントをブロードキャストするためのチャンネルを指定する。
    この特定のコードでは、PrivateChannel('channel-name') を返しており、このイベントが非公開のチャンネル 'channel-name' にブロードキャストされることを意味しています。
    ブロードキャストは通常、WebSocketやPusherを使用してリアルタイムでクライアントにデータを送信するために使用されます。
    public function broadcastOn()
    {
        return new PrivateChannel('channel-name');
    }
}
```

app/Listner

```jsx
<?php

declare(strict_types=1);

namespace App\Listeners;

use App\Consts\Flag;
use App\Models\EmailNotificationSetting;
use Illuminate\Support\Facades\Mail;

class SendSimpleEmailNotification
{
    /**
     * Create the event listener.
     */
    public function __construct()
    {
    }

    /**
     * Handle the event.
     *
     * @param $event
     */
    public function handle($event): void
    {
        if (!$event->emailNotificationDto->to_email) {
            return;
        }

        // Send Email depending on Email Notification Settings.
        if (isset($event->emailNotificationDto->notification_setting)) {
            $emailNotificationSetting = EmailNotificationSetting::select($event->emailNotificationDto->notification_setting)
                ->where('user_id', $event->emailNotificationDto->user_id)
                ->first();

            if ($emailNotificationSetting->{$event->emailNotificationDto->notification_setting} === Flag::TRUE) {
                **Mail::to($event->emailNotificationDto->to_email)->send($event->emailNotificationDto->mailable);**
            }
        }

        // Always Send Email regardless of Email Notification Settings.
        if ($event->emailNotificationDto->notification_setting === null) {
            **Mail::to($event->emailNotificationDto->to_email)->send($event->emailNotificationDto->mailable);**
        }
    }
}
```

app/Mailクラスの書き方

```jsx
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;

class WelcomeEmail extends Mailable
{
		// コンストラクタ
    public function __construct(string $userName, string $planName, string $planPrice, $startDate)
    {
				// メールで使う情報があれば主にコンストラクタの引数で受け取る
        // ここに定義したデータはbuildメソッド内のviewで使用することができる。

　　　　　$this->userName = $userName;
        $this->planName = $planName;
        $this->planPrice = $planPrice;
        $this->startDate = $startDate;
    }

		// メールを送るメソッド
    public function build()
    {
				// メールを送る
        return $this->to('aaa@example.com')             // 宛先
                    ->cc('bbb@example.com')             // CC
                    ->bcc('ccc@example.com')            // BCC
                    ->subject('会員登録が完了しました')     // 件名
                    ->view('mail.WelcomeEmail')         // 本文（HTMLメール）
                    ->text('mail.WelcomeEmail_text');   // 本文（プレーンテキストメール）
    }
}
```

例

```jsx
<?php

declare(strict_types=1);

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;

class SubscriptionPurchaseCompletedEmail extends Mailable
{
    use Queueable;
    use SerializesModels;

    public $userName;

    public $planName;

    public $planPrice;

    public $startDate;

    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct(string $userName, string $planName, string $planPrice, $startDate)
    {
        $this->userName = $userName;
        $this->planName = $planName;
        $this->planPrice = $planPrice;
        $this->startDate = $startDate;
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->subject('【' . config('app.name') . '】' . 'プレミアム会員契約完了のお知らせ')->view('emails.api.subscription_purchase_completed');
    }
}
```

**EventServiceProvider** : イベントとリスナーを関連付ける

```jsx
<?php

declare(strict_types=1);

namespace App\Providers;

use Illuminate\Auth\Events\Registered;
use Illuminate\Auth\Listeners\SendEmailVerificationNotification;
use Illuminate\Foundation\Support\Providers\EventServiceProvider as ServiceProvider;

class EventServiceProvider extends ServiceProvider
{
    /**
     * The event listener mappings for the application.
     *
     * @var array
     */
    protected $listen = [
        Registered::class => [
            SendEmailVerificationNotification::class,
        ],

        \SocialiteProviders\Manager\SocialiteWasCalled::class => [
            // ... other providers
            'SocialiteProviders\\Line\\LineExtendSocialite@handle',
            'SocialiteProviders\\Apple\\AppleExtendSocialite@handle',
            'SocialiteProviders\\Zoom\\ZoomExtendSocialite@handle',
        ],

        **'App\Events\SubscriptionPurchaseCompleted' => [   // Eventクラス
            'App\Listeners\SendSimpleEmailNotification',  // Event に対応する Listener クラス
        ],**

        'App\Events\SubscriptionPurchaseCanceled' => [
            'App\Listeners\SendSimpleEmailNotification',
        ],

        'App\Events\SubscriptionPurchaseOther' => [
            'App\Listeners\SendSimpleEmailNotification',
        ],
    ];

    /**
     * Register any events for your application.
     */
    public function boot(): void
    {
        parent::boot();
    }
}
```
