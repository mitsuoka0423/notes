---
title: "トークルームにバーコードを表示する"
---

コードはこちらのリポジトリで公開しています。

https://github.com/mitsuoka0423/laravel-line-menbers-card/tree/feature/barcode-bot

 (ブランチ：`feature/barcode-bot`)

> 途中で詰まってしまった方はこちらのコードを利用してください。

## この章の完成イメージ

`会員カード`とメッセージを送ると、会員カードの画像を表示します。

[![Image from Gyazo](https://i.gyazo.com/78d8f4c0d45f66204d7b0052aa26d7ff.gif)](https://gyazo.com/78d8f4c0d45f66204d7b0052aa26d7ff)

## ライブラリをインストールする

以下のライブラリを利用します。

- Airtableクライアントライブラリ（非公式）

https://github.com/TappNetwork/laravel-airtable

- PHPでバーコード画像を生成するライブラリ

https://github.com/picqer/php-barcode-generator

ターミナルで以下を実行します。

```bash
composer require tapp/laravel-airtable picqer/php-barcode-generator
```

## Airtableに会員情報を登録する

### (事前準備)Airtableの`base ID`と`API Key`を取得する

[事前準備](./preparing) の Airtableへログイン 〜 AirtableのAPI Keyを取得 を実施し、`base ID`と`API Key`を取得しておきます。

### コードを変更する

以下の3ファイルを変更します。

- `routes/api.php`
- `config/airtable.php`
- `.env`

順番に変更します。

#### `routes/api.php`

```diff php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use LINE\LINEBot;
use LINE\LINEBot\Constant\HTTPHeader;
use LINE\LINEBot\Event\MessageEvent\TextMessage;
use LINE\LINEBot\HTTPClient\CurlHTTPClient;
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

$httpClient = new CurlHTTPClient($_ENV['LINE_CHANNEL_ACCESS_TOKEN']);
$bot = new LINEBot($httpClient, ['channelSecret' => $_ENV['LINE_CHANNEL_SECRET']]);

Route::post('/webhook', function (Request $request) use ($bot) {
    Log::debug($request);

    $signature = $request->header(HTTPHeader::LINE_SIGNATURE);
    if (empty($signature)) {
        return abort(400);
    }

    $events = $bot->parseEventRequest($request->getContent(), $signature);
    Log::debug(['$events' => $events]);

    collect($events)->each(function ($event) use ($bot) {
        if ($event instanceof TextMessage) {
+          if ($event->getText() === '会員カード') {
+              // 会員登録済みか確認するため、Airtableからデータを取得する
+              $member = Airtable::where('UserId', $event->getUserId())->get();
+
+              if ($member->isEmpty()) {
+                  // Airtableに会員データがなければ、生成して登録する
+                  $memberId = strval(rand(1000000000, 9999999999));
+                  $member = Airtable::firstOrCreate([
+                      'UserId' => $event->getUserId(),
+                      'Name' => $bot->getProfile($event->getUserId())->getJSONDecodedBody()['displayName'],
+                      'MemberId' => $memberId,
+                  ]);
+                  Log::debug('Member is created.');
+              } else {
+                  // Airtableにデータがあれば、取得したデータを利用する
+                  $memberId = $member->first()['fields']['MemberId'];
+              }
+
+              return $bot->replyText($event->getReplyToken(), "会員IDは {$memberId} です！");
+          } else {
+              return $bot->replyText($event->getReplyToken(), $event->getText());
+          }
        }
    });

    return 'ok!';
});
```

#### `config/airtable.php`

続いて下記をターミナルで実行し、`config/airtable.php`を作成します。

```bash
php artisan vendor:publish --provider="Tapp\Airtable\AirtableServiceProvider"
```

`config/airtable.php`を一部変更します。

```diff php
(略)
    /*
    |--------------------------------------------------------------------------
    | Default Airtable Table
    |--------------------------------------------------------------------------
    |
    | This value can be found on the API docs page:
    | https://airtable.com/[BASE_ID]/api/docs#curl/table:tasks
    | The value will be hilighted at the beginning of each table section.
    | Example:
    | Each record in the `Tasks` contains the following fields
    |
     */
    'default' => 'default',

    'tables' => [

        'default' => [
            'name' => env('AIRTABLE_TABLE'),
        ],
+        env('AIRTABLE_TABLE') => [
+            'name' => env('AIRTABLE_TABLE'),
+        ]

    ],
(略)
```

#### `.env`

`.env`を開き、一番下にAirtableの変数を追加します。

```diff
LINE_CHANNEL_ACCESS_TOKEN=xxxxxxxxxxxxxxxxxxxxxxx
LINE_CHANNEL_SECRET=xxxxxxxxxxxxxxxxx

+AIRTABLE_KEY=事前準備で取得した API Key を入力する
+AIRTABLE_BASE=事前準備で取得した base ID を入力する
+AIRTABLE_TABLE=Members
+AIRTABLE_TYPECAST=false
```

### 動作確認

ここまで変更して、LINEで`会員カード`とメッセージを送ると以下のようになります。

- LINEに会員IDが表示される
- Airtableにレコードが追加される

[![Image from Gyazo](https://i.gyazo.com/dde82898893fea6e9ae46212dc19bb47.gif)](https://gyazo.com/dde82898893fea6e9ae46212dc19bb47)

続いて、会員IDをもとにバーコードを表示するよう変更します。

## 会員IDからバーコードを生成する

### コードを変更する

以下の2ファイルを変更します。

- `routes/api.php`
- `.env`

また、ターミナルでコマンドを実行します。

#### `routes/api.php`

```diff php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use LINE\LINEBot;
use LINE\LINEBot\Constant\HTTPHeader;
use LINE\LINEBot\Event\MessageEvent\TextMessage;
use LINE\LINEBot\HTTPClient\CurlHTTPClient;
+use LINE\LINEBot\MessageBuilder\ImageMessageBuilder;
use Tapp\Airtable\Facades\AirtableFacade;
use Picqer\Barcode\BarcodeGeneratorPNG;

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

$httpClient = new CurlHTTPClient($_ENV['LINE_CHANNEL_ACCESS_TOKEN']);
$bot = new LINEBot($httpClient, ['channelSecret' => $_ENV['LINE_CHANNEL_SECRET']]);

+$barcodeGenerator = new BarcodeGeneratorPNG();

+Route::post('/webhook', function (Request $request) use ($bot, $barcodeGenerator) {
    Log::debug($request);

    $signature = $request->header(HTTPHeader::LINE_SIGNATURE);
    if (empty($signature)) {
        return abort(400);
    }

    $events = $bot->parseEventRequest($request->getContent(), $signature);
    Log::debug(['$events' => $events]);

+    collect($events)->each(function ($event) use ($bot, $barcodeGenerator) {
        if ($event instanceof TextMessage) {
            if ($event instanceof TextMessage) {
                if ($event->getText() === '会員カード') {
                    // 会員登録済みか確認するため、Airtableからデータを取得する
                    $member = Airtable::where('UserId', $event->getUserId())->get();

                    if ($member->isEmpty()) {
                        // Airtableに会員データがなければ、生成して登録する
                        $memberId = strval(rand(1000000000, 9999999999));
                        $member = Airtable::firstOrCreate([
                            'UserId' => $event->getUserId(),
                            'Name' => $bot->getProfile($event->getUserId())->getJSONDecodedBody()['displayName'],
                            'MemberId' => $memberId,
                        ]);
                        Log::debug('Member is created.');
                    } else {
                        // Airtableにデータがあれば、取得したデータを利用する
                        $memberId = $member->first()['fields']['MemberId'];
                    }

+                    $barcodeFileName = "{$memberId}.png";
+                    $barcodeFilePath = "public/{$barcodeFileName}";
+                    if (!Storage::exists($barcodeFilePath)) {
+                        $barcodeImage = $barcodeGenerator->getBarcode($memberId, $barcodeGenerator::TYPE_CODE_128);
+                        Storage::put($barcodeFilePath, $barcodeImage);
+                    } else {
+                        $barcodeImage = Storage::get($barcodeFilePath);
+                    }
+
+                    $imageUrl = Config::get('app.url') . '/' . $barcodeFilePath;
+                    $imageMessageBuilder = new ImageMessageBuilder($imageUrl, $imageUrl);
+                    return $bot->replyMessage($event->getReplyToken(), $imageMessageBuilder);
                } else {
                    return $bot->replyText($event->getReplyToken(), $event->getText());
                }
            }
        }
    });

    return 'ok!';
});
```

#### `.env`

`.env`の`APP_URL`を[Laravelでオウム返しするLINE Botを作る](./laravel-echo-bot)で発行したURLに書き換えます。

```diff
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:qhQdHDelWITh6E5JjlIbP4CXseXC9fwg96hbZvZe1XU=
APP_DEBUG=true
-APP_URL=http://localhost
+APP_URL=https://769a571cb14e66.lhrtunnel.link
(略)
```

#### コマンド実行

ターミナルで以下を実行します。

```bash
php artisan storage:link
```

### 動作確認する

ここまで変更して、LINEで`会員カード`とメッセージを送るとバーコードが表示されるようになります！

[![Image from Gyazo](https://i.gyazo.com/78d8f4c0d45f66204d7b0052aa26d7ff.gif)](https://gyazo.com/78d8f4c0d45f66204d7b0052aa26d7ff)
