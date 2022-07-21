---
title: "(Gitpod)Laravelでオウム返しするLINE Botを作る"
---

コードはこちらのリポジトリで公開しています。

https://github.com/mitsuoka0423/laravel-line-menbers-card/tree/feature/echo-bot

 (ブランチ：`feature/echo-bot`)

> 途中で詰まってしまった方はこちらのコードを利用してください。

## この章の完成イメージ

送ったメッセージをそのまま返すオウム返しを作っていきます。

[![Image from Gyazo](https://i.gyazo.com/6e68f98011666e860b9524b82f8921b3.gif)](https://gyazo.com/6e68f98011666e860b9524b82f8921b3)

## Gitpodでワークスペースを作成する

本ハンズオンでは、エディターと実行環境がついている`Gitpod`を利用してハンズオンを進めていきます。
最初にGitpodの準備を行います。

下記URLを開きます。

https://github.com/mitsuoka0423/laravel-template

下にスクロールして、`Open in Gitpod`を選択します。

[![Image from Gyazo](https://i.gyazo.com/0c8ebc73beb7973b68ccfe70b091cd9b.png)](https://gyazo.com/0c8ebc73beb7973b68ccfe70b091cd9b)

:::details ログインを求められた方は

下記を参考にログインしてください。

[Gitpodへログイン](https://zenn.dev/tmitsuoka0423/books/handson-members-card-laravel-react-line/viewer/preparing#gitpod%E3%81%AE%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3)

[![Image from Gyazo](https://i.gyazo.com/493551a06334c2975b4f6be96e3b5c84.png)](https://gyazo.com/493551a06334c2975b4f6be96e3b5c84)
:::

下記のようなエディターが表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/64f51e87814e5e28ab0eedc2145cac48.png)](https://gyazo.com/64f51e87814e5e28ab0eedc2145cac48)

## Laravelの開発用サーバーを起動する

ターミナルで以下を実行します。

```bash
php artisan serve
```

[![Image from Gyazo](https://i.gyazo.com/d73e411102c43dd058596dc5ea236426.png)](https://gyazo.com/d73e411102c43dd058596dc5ea236426)

右下にポップアップが表示されるので、`Make Public`を選択します。

[![Image from Gyazo](https://i.gyazo.com/44ad55cc1db066964eec27842bde8f52.png)](https://gyazo.com/44ad55cc1db066964eec27842bde8f52)

`サイドバー > Remote Explorer`を選択し、地球マークをクリックします。

[![Image from Gyazo](https://i.gyazo.com/46e9efe74c1bb7b37d97f0ef30def60c.png)](https://gyazo.com/46e9efe74c1bb7b37d97f0ef30def60c)

このような画面が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/03691d8e49b043b4417ddfd567220841.png)](https://gyazo.com/03691d8e49b043b4417ddfd567220841)

## ライブラリをインストールする

LINE Bot開発に下記のライブラリを利用します。

https://github.com/line/line-bot-sdk-php

ターミナルで以下を実行してインストールします。

```bash
composer require linecorp/line-bot-sdk
```

## LINE Webhook用処理を作成する

LINEのイベントをフックしたときに実行される処理を書いていきます。

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
+            return $bot->replyText($event->getReplyToken(), $event->getText());
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

+LINE_CHANNEL_ACCESS_TOKEN=ここにアクセストークンを記入する
+LINE_CHANNEL_SECRET=ここにチャネルシークレットを記入する
```

| 名前                       | 設定する値 |
| ------------------------- | -- |
| LINE_CHANNEL_ACCESS_TOKEN | [LINE公式アカウントの作成 / LINE Botの初め方](https://zenn.dev/protoout/articles/16-line-bot-setup)で取得したアクセストークン |
| LINE_CHANNEL_SECRET       | [LINE公式アカウントの作成 / LINE Botの初め方](https://zenn.dev/protoout/articles/16-line-bot-setup)で取得したチャネルシークレット |

:::message alert
ブラウザで自動翻訳をオンにしていると、アクセストークンやチャネルシークレットをきちんとコピーできないケースがあります。
ブラウザの自動翻訳はオフにしてください。
:::

## 動作確認する

### 開発用サーバーを起動する&外部公開する

ローカルで開発用サーバーを起動し、LINEのイベントをフックできるようにlocalhost.runを利用して外部公開します。

ターミナルを2つ起動して、以下をそれぞれ実行します。

```bash
php artisan serve
```

```bash
ssh -R 80:localhost:8000 ssh.localhost.run
```

実行イメージ

[![Image from Gyazo](https://i.gyazo.com/e71263218699bc6895c682d1e168ffce.png)](https://gyazo.com/e71263218699bc6895c682d1e168ffce)

最後に表示されるURLをコピーします。

今回は`https://b7b3704c411571.lhrtunnel.link`ですね

```log
(略)
** your connection id is 2f47b9ae-7276-41a2-a0d7-5e267a003042, please mention it if you send me a message about an issue. **

b7b3704c411571.lhrtunnel.link tunneled with tls termination, https://b7b3704c411571.lhrtunnel.link
```

:::details localhost.runでエラーが出る方は
`ngork`を利用してみてください。

ターミナルで以下を実行します。
```bash
npx ngrok http 8000
```

```
ngrok by @inconshreveable                                                                                  (Ctrl+C to quit)
                                                                                                                           
Session Status                online                                                                                       
Session Expires               1 hour, 59 minutes                                                                           
Version                       2.3.40                                                                                       
Region                        United States (us)                                                                           
Web Interface                 http://127.0.0.1:4040                                                                        
Forwarding                    http://3f61-2409-10-d320-1c00-c904-a56e-fc93-66fb.ngrok.io -> http://localhost:8000          
Forwarding                    https://3f61-2409-10-d320-1c00-c904-a56e-fc93-66fb.ngrok.io -> http://localhost:8000         
                                                                                                                           
Connections                   ttl     opn     rt1     rt5     p50     p90                                                  
                              0       0       0.00    0.00    0.00    0.00     
```

`Forwarding`の`https`で始まる方のURLを利用してください。
:::

### LINE DevelopersでWebhook URLを設定する

:::message
ここからは、[LINE Developersコンソール](https://developers.line.biz/ja/)での作業です。
:::

Messaging APIのチャネル設定画面から`Webhook URL`を設定します。

場所はこちらです。
LINE Developersコンソール > 作成したMessaging APIのチャネル > Messaging API設定タブ > Webhook設定

[![Image from Gyazo](https://i.gyazo.com/77c6998967d5628366d3f570abfbb351.gif)](https://gyazo.com/77c6998967d5628366d3f570abfbb351)

上記手順でコピーしたURL(`https://xxxxxxxxxxx.lhrtunnel.link`)の末尾に`/api/webhook`をつけます。

> 今回は`https://b7b3704c411571.lhrtunnel.link/api/webhook`となります

:::message alert
`/api/webhook`を忘れないように気をつけてください。
:::

[![Image from Gyazo](https://i.gyazo.com/72fba354ec26794294469eec27b01826.png)](https://gyazo.com/72fba354ec26794294469eec27b01826)

また、`Webhookの利用`をオンにします。

[![Image from Gyazo](https://i.gyazo.com/f5cd7acb933681700ea5ae54f52c75b3.png)](https://gyazo.com/f5cd7acb933681700ea5ae54f52c75b3)

### 動作確認する

`検証`をクリックします。

[![Image from Gyazo](https://i.gyazo.com/12c8af4110591477ed26166bfcea455f.png)](https://gyazo.com/12c8af4110591477ed26166bfcea455f)

`成功`と表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/96d7dbe23fe2044405dd5d1df173339b.png)](https://gyazo.com/96d7dbe23fe2044405dd5d1df173339b)

### LINEで話しかけてみる

作ったLINE Botと友達になり、メッセージを送ってみましょう。
そのまま返却されるようになりました！

[![Image from Gyazo](https://i.gyazo.com/6e68f98011666e860b9524b82f8921b3.gif)](https://gyazo.com/6e68f98011666e860b9524b82f8921b3)

次のステップで、会員バーコードを表示する処理を追加します。
