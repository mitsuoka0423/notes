---
title: "LaravelでAPI作成"
---

コードはこちらのリポジトリで公開しています。

https://github.com/mitsuoka0423/line-members-card-laravel-backend

### Laravelプロジェクトを作成する

ターミナルで以下を実行します。

```bash
composer create-project laravel/laravel example-app
cd example-app
php artisan serve
```

> `example-app`は好きなプロジェクト名に変更してOKです。


以下のように表示されたら、http://127.0.0.1:8000 にアクセスします。

```log
Starting Laravel development server: http://127.0.0.1:8000
```

このような画面が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/03691d8e49b043b4417ddfd567220841.png)](https://gyazo.com/03691d8e49b043b4417ddfd567220841)

### ライブラリをインストールする

以下のライブラリを利用します。

https://github.com/line/line-bot-sdk-php

https://github.com/TappNetwork/laravel-airtable

ターミナルで以下を実行してインストールします。

```bash
composer require linecorp/line-bot-sdk tapp/laravel-airtable
```

### LINE Webhook用APIを作成する

友達登録時に会員カードを発行する処理を作成します。

`routes/api.php`を以下の通り変更します。

```diff php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Log;
use Illuminate\Support\Facades\Route;
+use LINE\LINEBot;
+use LINE\LINEBot\Constant\HTTPHeader;
+use LINE\LINEBot\Event\MessageEvent\TextMessage;
+use LINE\LINEBot\HTTPClient\CurlHTTPClient;
+use Tapp\Airtable\Facades\AirtableFacade;

/*
|--------------------------------------------------------------------------
| API Routes
|--------------------------------------------------------------------------
|
| Here is where you can register API routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| is assigned the "api" middleware group. Enjoy building your API!
|
*/

Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});

+$httpClient = new CurlHTTPClient($_ENV['LINE_CHANNEL_ACCESS_TOKEN']);
+$bot = new LINEBot($httpClient, ['channelSecret' => $_ENV['LINE_CHANNEL_SECRET']]);
+
+Route::post('/webhook', function (Request $request) use ($bot) {
+    Log::debug($request);
+
+    $signature = $request->header(HTTPHeader::LINE_SIGNATURE);
+    if (empty($signature)) {
+        return abort(400);
+    }
+
+    $events = $bot->parseEventRequest($request->getContent(), $signature);
+    Log::debug(['$events' => $events]);
+
+    collect($events)->each(function ($event) use ($bot) {
+        if ($event instanceof TextMessage) {
+            if ($event->getText() === '会員カードを発行する')
+            {
+                $user_id = $event->getUserId();
+                Log::debug(['$user_id' => $user_id]);
+
+                $member = Airtable::where('UserId', $user_id)->get();
+                Log::debug(['$member' => $member->toArray()]);
+
+                if ($member->isEmpty()) {
+                    $barcode_id = strval(rand(1000000000, 9999999999));
+                    Log::debug($barcode_id);
+
+                    $member = Airtable::firstOrCreate([
+                        'UserId' => $user_id,
+                        'Name' => $bot->getProfile($user_id)->getJSONDecodedBody()['displayName'],
+                        'MemberId' => $barcode_id,
+                    ]);
+                    Log::debug('Member is created.');
+                    Log::debug($member);
+
+                    return $bot->replyText($event->getReplyToken(), '会員カードを発行しました！');
+                } else {
+                    return $bot->replyText($event->getReplyToken(), '既に会員カードが発行されています。');
+                }
+
+            } else {
+                return $bot->replyText($event->getReplyToken(), $event->getText());
+            }
+        }
+    });
+
+    return 'ok!';
+});
```

`.env`に以下を追加します。

```diff 
(略)
MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"

+LINE_CHANNEL_ACCESS_TOKEN=
+LINE_CHANNEL_SECRET=
+
+AIRTABLE_KEY=
+AIRTABLE_BASE=
+AIRTABLE_TABLE=Members
+AIRTABLE_TYPECAST=true
```

| 名前                       | 設定する値 |
| ------------------------- | -- |
| LINE_CHANNEL_ACCESS_TOKEN | [事前準備](./preparing) > LINE Developersへログイン & 2つのキーを取得する で取得したアクセストークン |
| LINE_CHANNEL_SECRET       | [事前準備](./preparing) > LINE Developersへログイン & 2つのキーを取得する で取得したチャネルシークレット |
| AIRTABLE_KEY              | [事前準備](./preparing) > AirtableのAPI Keyを取得 で取得したAPI Key |
| AIRTABLE_BASE             | [事前準備](./preparing) > Airtableのbase IDを取得 で取得したbase ID |

### 会員情報取得APIを作成する

LIFFに会員カードを表示するためのAPIを作成します。

`routes/api.php`を以下の通り変更します。

```diff php
(略)
    return 'ok!';
});

+Route::get('/members/{member_id}', function ($member_id) {
+    $member = Member::find($member_id);
+
+    if (empty($member)) {
+        return abort(404);
+    }
+
+    return $member;
+});
```

### 動作確認する

#### 開発用サーバーを起動する&外部公開する

ローカルで開発用サーバーを起動し、LINEのWebhookを受け取れるようにlocalhost.runを利用して外部公開します。

> 開発用の方法として利用してください。
> 

ターミナルで以下を実行します。

```bash
php artisan serve
ssh -R 80:localhost:8000 ssh.localhost.run
```

```log
(略)
** your connection id is 2f47b9ae-7276-41a2-a0d7-5e267a003042, please mention it if you send me a message about an issue. **

b7b3704c411571.lhrtunnel.link tunneled with tls termination, https://b7b3704c411571.lhrtunnel.link
```

最後に表示されるURLをコピーしておきます。

#### LINE DevelopersでWebhook URLを設定する

[事前準備](./preparing) > LINE Developersへログイン & 2つのキーを取得するで作成したMessaging APIのチャネル > Messaging API設定 > Webhook設定
から`Webhook URL`を設定します。

設定する値は、[開発用サーバーを起動する&外部公開する](#開発用サーバーを起動する&外部公開する)でコピーしたURLに`/api/webhook`をつけます。

> `/api/webhook`を忘れないように気をつけてください。
> `/api/webhook`を忘れないように気をつけてください。

[![Image from Gyazo](https://i.gyazo.com/d50ef02aa64a49faaadcaa5af323fab3.png)](https://gyazo.com/d50ef02aa64a49faaadcaa5af323fab3)

