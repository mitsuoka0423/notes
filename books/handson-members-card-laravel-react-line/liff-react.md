---
title: "LINE LIFF + Reactで会員カードUIを作成する"
---

コードはこちらのリポジトリで公開しています。

https://github.com/mitsuoka0423/line-members-card-react-frontend

## この章の完成イメージ

会員カードをブラウザで表示します。

[![Image from Gyazo](https://i.gyazo.com/903ecef196a9200c2af07fec34d187c0.gif)](https://gyazo.com/903ecef196a9200c2af07fec34d187c0)

## サンプルコードを動かしてみる

### サンプルコードをダウンロードする

コードはこちらのリポジトリで公開しています。

https://github.com/mitsuoka0423/line-members-card-react-frontend

コードを zip でダウンロードして解凍します（クローンでも OK です）

[![Image from Gyazo](https://i.gyazo.com/fabb3b1b77c007f07b0afe8585bbc4d7.png)](https://gyazo.com/fabb3b1b77c007f07b0afe8585bbc4d7)

### ライブラリをインストールする

:::message
ここからは VS Code での作業です。
:::

ターミナルでさきほど解凍したフォルダーに移動し、下記を実行します。

```bash
yarn
```

以下のように表示されれば OK です。

```log
✨  Done in 3.66s.
```

### 環境変数を設定する

ターミナルで下記を実行します。

```bash
cp .env.sample .env
```

`.env`ファイルの中身は変更なしで OK です。

### 動作確認する

ターミナルで下記を実行します。

```bash
yarn dev
```

以下のように表示されます。

```log
vite v2.9.9 dev server running at:

> Local: http://localhost:3000/
> Network: use `--host` to expose

ready in 225ms.
```

http://localhost:3000/ にアクセスして、下記のような画面が表示されれば OK です。

[![Image from Gyazo](https://i.gyazo.com/2a0b8df06c5a372d2f3aa258920803e8.png)](https://gyazo.com/2a0b8df06c5a372d2f3aa258920803e8)

## Airtableから会員IDを取得する

フロントエンドから API を叩いて、会員 ID を取得し、フロントエンドでバーコードを生成します。

### バックエンドの変更

ここからは Laravel のコードを変更します。

`routes/api.php`の一番下に、以下のコードを追加します。

```php
Route::get('/members/{idToken}', function ($idToken) use ($bot) {
    Log::debug(['idToken' => $idToken]);

    // IDトークンを検証する
    // https://developers.line.biz/ja/reference/line-login/#verify-id-token
    $response = Http::asForm()->post('https://api.line.me/oauth2/v2.1/verify', [
        'id_token' => $idToken,
        'client_id' => $_ENV['LINE_LOGIN_CHANNEL_ID'],
    ]);
    Log::debug(['response' => $response->json()]);

    // LINEのユーザーIDと表示名を取得する
    $userId = $response->json()['sub'];
    $name = $response->json()['name'];

    // 会員登録済みか確認するため、Airtableからデータを取得する
    $member = Airtable::where('UserId', $userId)->get();

    if ($member->isEmpty()) {
        // Airtableに会員データがなければ、生成して登録する
        $memberId = strval(rand(1000000000, 9999999999));
        $member = Airtable::firstOrCreate([
            'UserId' => $userId,
            'Name' => $name,
            'MemberId' => $memberId,
        ]);
        Log::debug('Member is created.');
    } else {
        // Airtableにデータがあれば、取得したデータを利用する
        $memberId = $member->first()['fields']['MemberId'];
    }

    return $member->first()['fields'];
});
```

:::details api.php の全量はこちら
```diff php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Http;
use Illuminate\Support\Facades\Route;
use Illuminate\Support\Facades\Storage;
use LINE\LINEBot;
use LINE\LINEBot\Constant\HTTPHeader;
use LINE\LINEBot\Event\MessageEvent\TextMessage;
use LINE\LINEBot\HTTPClient\CurlHTTPClient;
use LINE\LINEBot\MessageBuilder\ImageMessageBuilder;
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

$barcodeGenerator = new BarcodeGeneratorPNG();

Route::post('/webhook', function (Request $request) use ($bot, $barcodeGenerator) {
    Log::debug($request);

    $signature = $request->header(HTTPHeader::LINE_SIGNATURE);
    if (empty($signature)) {
        return abort(400);
    }

    $events = $bot->parseEventRequest($request->getContent(), $signature);
    Log::debug(['$events' => $events]);

    collect($events)->each(function ($event) use ($bot, $barcodeGenerator) {
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

                $barcodeFileName = "{$memberId}.png";
                $barcodeFilePath = "public/{$barcodeFileName}";
                if (!Storage::exists($barcodeFilePath)) {
                    $barcodeImage = $barcodeGenerator->getBarcode($memberId, $barcodeGenerator::TYPE_CODE_128);
                    Storage::put($barcodeFilePath, $barcodeImage);
                } else {
                    $barcodeImage = Storage::get($barcodeFilePath);
                }

                $imageUrl = Config::get('app.url') . '/storage/' . $barcodeFileName;
                $imageMessageBuilder = new ImageMessageBuilder($imageUrl, $imageUrl);
                return $bot->replyMessage($event->getReplyToken(), $imageMessageBuilder);
            } else {
                return $bot->replyText($event->getReplyToken(), $event->getText());
            }
        }
    });

    return 'ok!';
});

+Route::get('/members/{idToken}', function ($idToken) use ($bot) {
+    Log::debug(['idToken' => $idToken]);
+
+    // IDトークンを検証する
+    // https://developers.line.biz/ja/reference/line-login/+#verify-id-token
+    $response = Http::asForm()->post('https://api.line.me/oauth2/v2.+1/verify', [
+        'id_token' => $idToken,
+        'client_id' => $_ENV['LINE_LOGIN_CHANNEL_ID'],
+    ]);
+    Log::debug(['response' => $response->json()]);
+
+    // LINEのユーザーIDと表示名を取得する
+    $userId = $response->json()['sub'];
+    $name = $response->json()['name'];
+
+    // 会員登録済みか確認するため、Airtableからデータを取得する
+    $member = Airtable::where('UserId', $userId)->get();
+
+    if ($member->isEmpty()) {
+        // Airtableに会員データがなければ、生成して登録する
+        $memberId = strval(rand(1000000000, 9999999999));
+        $member = Airtable::firstOrCreate([
+            'UserId' => $userId,
+            'Name' => $name,
+            'MemberId' => $memberId,
+        ]);
+        Log::debug('Member is created.');
+    } else {
+        // Airtableにデータがあれば、取得したデータを利用する
+        $memberId = $member->first()['fields']['MemberId'];
+    }
+
+    return $member->first()['fields'];
+});
```
:::

### フロントエンドの変更



`.env`の`VITE_LIFF_API_ENDPOINT`を以下の通り変更します。

```diff
VITE_LIFF_ID=LIFF_ID_HERE
VITE_LIFF_MOCK_MODE=true # true | false
VITE_LIFF_REDIRECT_URI=
+VITE_LIFF_API_ENDPOINT=https://b7b3704c411571.lhrtunnel.link
VITE_LIFF_CODE_TYPE=barcode # barcode | qrcode
```

### 動作確認する

http://localhost:3000/ にアクセスします。

:::message
バックエンド、フロントエンド両方の開発用サーバーを起動しておく必要があります。
ターミナルで`php artisan serve`および`yarn dev`を実行した状態にしてください。
:::

下記のように、バーコードの ID が API から取得したものになっていれば OK です。

[![Image from Gyazo](https://i.gyazo.com/214a13aedfde10023a3ba08c360930b4.png)](https://gyazo.com/214a13aedfde10023a3ba08c360930b4)

## LIFFアプリの設定を行う

### LIFFアプリを登録する

:::message
ここからは、[LINE Developersコンソール](https://developers.line.biz/ja/)での作業です。
:::

下記手順を参考にチャネルを作成します。
チャネルタイプは`LINEログイン`を選択します。

https://developers.line.biz/ja/docs/liff/getting-started/

下記手順を参考に、さきほど作成したチャネルに LIFF アプリを追加します。

https://developers.line.biz/ja/docs/liff/registering-liff-apps/#registering-liff-app

設定する値はこちらの様にします。

[![Image from Gyazo](https://i.gyazo.com/e257b90132e5c453160f01380817e0a3.png)](https://gyazo.com/e257b90132e5c453160f01380817e0a3)

表示される`LIFF ID`、`LIFF URL`をコピーしておきます。

[![Image from Gyazo](https://i.gyazo.com/01e157acb40a8fa27cca31f4948ca586.png)](https://gyazo.com/01e157acb40a8fa27cca31f4948ca586)

### localhostの証明書を設定する

:::message
ここからは VS Code（フロントエンドのプロジェクト）での作業です。
:::

こちらの記事を参考にさせていただき、`localhost-key.pem`と`localhost.pem`を生成します。

https://dev.classmethod.jp/articles/vite-https-localhost/

下記のように`localhost-key.pem`と`localhost.pem`が生成されれば OK です。

[![Image from Gyazo](https://i.gyazo.com/2d6a8db22e4db276a91b34cbe43bf403.png)](https://gyazo.com/2d6a8db22e4db276a91b34cbe43bf403)

続いて`vite.config.ts`を下記の通り変更します。

```diff typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
+import fs from 'fs';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  build: {
    outDir: 'dist',
  },
+  server: {
+    https: {
+      key: fs.readFileSync('./localhost-key.pem'),
+      cert: fs.readFileSync('./localhost.pem'),
+    },
+  },
});
```

開発用サーバーを再起動すると、URL が`https`始まりになります。

```log
vite v2.9.9 dev server running at:

> Local: https://localhost:3000/
> Network: use `--host` to expose

ready in 171ms.
```

https://localhost:3000/ にアクセスして、バーコードが表示されれば OK です。

[![Image from Gyazo](https://i.gyazo.com/3f367695f3e825ac5d8fc520b380b241.png)](https://gyazo.com/3f367695f3e825ac5d8fc520b380b241)


### 環境変数を設定する

:::message
バックエンドのプロジェクトで作業を行います。
:::

`.env`に`LINE_LOGIN_CHANNEL_ID`を追加します。

```diff
(略)
LINE_CHANNEL_ACCESS_TOKEN=xxxxxxxxxxxxxxxxxxx
LINE_CHANNEL_SECRET=xxxxxxxxxxxxxxxxxxxx
+LINE_LOGIN_CHANNEL_ID=12345678

AIRTABLE_KEY=keyXXXXXXXXXX
AIRTABLE_BASE=appXXXXXXXXXXXX
AIRTABLE_TABLE=Members
AIRTABLE_TYPECAST=false
```

:::message
フロントエンドのプロジェクトで作業を行います。
:::

`.env`を下記の通り変更します。

```diff
+VITE_LIFF_ID=1657231722-PwJ6Glrz
+VITE_LIFF_MOCK_MODE=false # true | false
+VITE_LIFF_REDIRECT_URI=https://localhost:3000
VITE_LIFF_API_ENDPOINT=https://b7b3704c411571.lhrtunnel.link
VITE_LIFF_CODE_TYPE=barcode # barcode | qrcode
```

### 動作確認する

[LIFFアプリを登録する](#liffアプリを登録する)でコピーした LIFF URL にアクセスします。

[![Image from Gyazo](https://i.gyazo.com/b250123461d3d200a9519063aa4c5dca.png)](https://gyazo.com/b250123461d3d200a9519063aa4c5dca)

LINE へのログインが求められるので、ログインします。

[![Image from Gyazo](https://i.gyazo.com/51642832b08702b19fd906a83832fe76.png)](https://gyazo.com/51642832b08702b19fd906a83832fe76)

Airtable の`MemberId`と同じ番号が表示されれば OK です。

[![Image from Gyazo](https://i.gyazo.com/903ecef196a9200c2af07fec34d187c0.gif)](https://gyazo.com/903ecef196a9200c2af07fec34d187c0)

[![Image from Gyazo](https://i.gyazo.com/96481d73cc41af804b04b7b3432fb567.png)](https://gyazo.com/96481d73cc41af804b04b7b3432fb567)
