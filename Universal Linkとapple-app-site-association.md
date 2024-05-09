Universal Linksでは、アプリインストール時に***apple-app-site-associationファイル***が読み込まれ、その後のページ遷移時には再度サーバーへのリクエストが発生しない。
iOSはUniversal Linkをクリックしたときに、apple-app-site-associationファイルを読み込んで、関連付けられたページ（またはウェブコンテンツ）があるかどうかを確認する。
その後、問題なければ、該当アプリのページ(orウェブコンテンツ)が開かれる。アプリがインストールされていない場合、通常のWebページが表示される場合がある。

apple-app-site-associationファイル

```
{
    "applinks": {
        "apps": [],
        "details": [
            {
                "appIDs": [
                    "ABCD1234.com.sample.testing"
                ],
                "appID": "6BTCQFLDGA.jp.co.bridge.app.staging",　　　　// Xcodeでプロジェクト作成をすると自動で生成されるBundle ID
                "components": [
                    {
                        "/": "/launch"
                    },
                    {
                        "/": "/users/inherit-with-email"
                    },
                    {
                        "/": "/verify-email"
                    },
                    {
                        "/": "/reset-password"
                    },
                    {
                        "/": "identification"
                    },
                    {
                        "/": "/notifications/*",
                        "comment": "パスが/notifications/からスタートしたURLはUniversal Linksにする"
                    },
                    {
                        "/": "/matching-list"
                    },
                    {
                        "/": "/inquiry/entry"
                    },
                    {
                        "/": "/user-induction"
                    }
                ],
                "paths": [
                    "/launch",
                    "/users/inherit-with-email",
                    "/verify-email",
                    "/reset-password",
                    "/identification",
                    "/notifications/*",
                    "/matching-list",
                    "/inquiry/entry",
                    "/user-induction"
                ]
            }
        ]
    },
    "webcredentials": {
        "apps": [
            "ABCD1234.com.sample.testing"
        ]
    }
}
```

SPAでは、一度アプリがロードされた後は、通常のページ遷移時にはサーバーへの追加のリクエストが発生しない。

SPAとUniversal Linksは、クライアントサイドでの操作を最適化するために使用される類似したアプローチ。
