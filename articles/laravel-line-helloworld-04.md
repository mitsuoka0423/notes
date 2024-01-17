---
title: "Laravelで超シンプルにLINE Botを作る（Messaging API編） #laravel #messagingapi #php"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["PHP", "Laravel", "MessagingAPI", "LINE", "Docker"]
published: true
---

## はじめに

このシリーズでは、PHP フレームワークの 1 つである`Laravel`を使った LINE Bot の作り方を説明します。

### 完成イメージ

https://twitter.com/mitsuoka0423/status/1522222293958934531?s=20&t=Acf2XNzs3MQMTpb0TKxnmw?conversation=none

### 目次

- [Laravelで超シンプルにLINE Botを作る（開発準備編）](./laravel-line-helloworld-01)
- [Laravelで超シンプルにLINE Botを作る（Webhookエンドポイント作成編）](./laravel-line-helloworld-02)
- [Laravelで超シンプルにLINE Botを作る（ngrokインストール編）](./laravel-line-helloworld-03)
- Laravel で超シンプルに LINE Bot を作る（Messaging API 編）

### ドキュメント

http://laravel.jp/

https://developers.line.biz/ja/services/messaging-api/

## 環境

- MacBook Air (M1, 2020)

```bash
$ sw_vers

ProductName:    macOS
ProductVersion: 12.2.1
BuildVersion:   21D62
```

```bash
$ docker -v

Docker version 20.10.14, build a224086
```

## LINE Messaging API SDK for PHP をインストールする

公式 SDK が提供されていますので、こちらをインストールします。

https://github.com/line/line-bot-sdk-php

ターミナルで以下のコマンドを実行します。

```bash
docker compose exec laravel.test composer require linecorp/line-bot-sdk
```

以下のように表示されれば OK です。

```log
Info from https://repo.packagist.org: #StandWithUkraine
Using version ^7.3 for linecorp/line-bot-sdk
(略)
No publishable resources for tag [laravel-assets].
Publishing complete.
```

`composer.json`に`line-bot-sdk`が追加されます。

```diff json:composer.json
...
"require": {
    "php": "^8.0.2",
    "guzzlehttp/guzzle": "^7.2",
    "laravel/framework": "^9.2",
    "laravel/sanctum": "^2.14.1",
    "laravel/tinker": "^2.7",
+     "linecorp/line-bot-sdk": "^7.3"
},
...
```

## コードを追加する

オウム返しを行うためのコードを`routes/api.php`に追加します。

```diff php:routes/api.php
...
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
+ use LINE\LINEBot\HTTPClient\CurlHTTPClient;
+ use LINE\LINEBot;

Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});

+ $httpClient = new CurlHTTPClient($_ENV['LINE_CHANNEL_ACCESS_TOKEN']);
+ $bot = new LINEBot($httpClient, ['channelSecret' => $_ENV['LINE_CHANNEL_SECRET']]);
+ 
+ Route::post('/webhook', function (Request $request) use ($bot) {
+     $request->collect('events')->each(function ($event) use ($bot) {
+         $bot->replyText($event['replyToken'], $event['message']['text']);
+     });
+     return 'ok!';
+ });
```

`.env`ファイルに以下を追加します。

```diff toml:.env
LINE_CHANNEL_ACCESS_TOKEN=<LINE Developersで発行したアクセストークンを記入>
LINE_CHANNEL_SECRET=<LINE Developersで取得したチャネルシークレットを記入>
```

トークンの取得方法は下記記事を参照してください。

https://zenn.dev/protoout/articles/16-line-bot-setup

## 動作確認する

LINE Developers の Webhook URL に設定するための URL を ngrok を利用して発行します。

### 事前準備

[Laravelで超シンプルにLINE Botを作る（ngrokインストール編）](./laravel-line-helloworld-03)の手順を一通り終わらせておきます。

### URLを発行する

以下のコマンドを実行します。

```bash
$ ngrok http 80
```

`Forwarding`に表示される URL をコピーしておきます。

```log
Session Status                online                                                                         
Account                       xxxxxxxxx (Plan: Free)                                       
Version                       3.0.3                                                                          
Region                        Japan (jp)                                                                     
Latency                       20.696375ms                                                                    
Web Interface                 http://127.0.0.1:4040                                                          
Forwarding                    https://0f6a-2409-10-d320-1c00-9dbe-82dd-d54b-c67b.jp.ngrok.io -> http://localhost:80

Connections                   ttl     opn     rt1     rt5     p50     p90                                    
                              0       0       0.00    0.00    0.00    0.00   
```

### LINE DevelopersコンソールでWebhook URLを設定する

上記でコピーした`Forwarding`に表示される URL を、LINE Developers コンソールの`Webhook URL`に設定します。

> `ngrok http 80`を実行するごとにURLが変わるので注意してください。
> URLの末尾に`/api/webhook`を追加してください。

[![Image from Gyazo](https://i.gyazo.com/20d3bd89290b676b0ceb9d328061473f.png)](https://gyazo.com/20d3bd89290b676b0ceb9d328061473f)

`検証`をクリックします。
`成功`と表示されれば OK です。

> エラーになった場合は、以下を見直しましょう
> - ngrokで発行されたURLが古いものになっていないか
> - URL末尾に`/api/webhook`を追加しているか

[![Image from Gyazo](https://i.gyazo.com/329731a8134a257ab773389f0c8d08e6.png)](https://gyazo.com/329731a8134a257ab773389f0c8d08e6)

`Webhookの利用`を ON にします。

[![Image from Gyazo](https://i.gyazo.com/c1ea5a57919a80d157533882e5b4df8f.png)](https://gyazo.com/c1ea5a57919a80d157533882e5b4df8f)

`応答メッセージ`を無効にします。

[![Image from Gyazo](https://i.gyazo.com/37df0c1b58da3d1f12eda328d8b22d53.png)](https://gyazo.com/37df0c1b58da3d1f12eda328d8b22d53)

これで準備完了です。

### LINEで友達登録して、話しかけてみる

LINE Developers コンソールの`Messaging API設定`タブに QR コードがあるので、読み取って友達になりましょう。

[![Image from Gyazo](https://i.gyazo.com/ba88c870bc0c3eaa188f7557014fbff3.png)](https://gyazo.com/ba88c870bc0c3eaa188f7557014fbff3)

話しかけると、話しかけた言葉がそのまま返ってくるようになりました。

https://twitter.com/mitsuoka0423/status/1522206629441454081?s=20&t=jOlnpNyU1tLayGkUQ0oKRg?conversation=none

## おわりに

Laravel で超シンプルにオウム返しする LINE Bot を作成しました。
Messaging API を使ってもっと色んなことができるので、そちらの解説記事も追加していこうと思います。

- [Laravelで超シンプルにLINE Botを作る（開発準備編）](./laravel-line-helloworld-01)
- [Laravelで超シンプルにLINE Botを作る（Webhookエンドポイント作成編）](./laravel-line-helloworld-02)
- [Laravelで超シンプルにLINE Botを作る（ngrokインストール編）](./laravel-line-helloworld-03)
- Laravel で超シンプルに LINE Bot を作る（Messaging API 編）
