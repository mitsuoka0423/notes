---
title: "Laravelで超シンプルにLINE Botを作る #laravel #messagingapi #php"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["PHP", "Laravel", "MessagingAPI", "LINE"]
published: true
---

## はじめに

PHP フレームワークの 1 つである`Laravel`を使って、LINE Bot を超シンプルに作ります。

### 完成イメージ

https://twitter.com/mitsuoka0423/status/1522222293958934531?s=20&t=Acf2XNzs3MQMTpb0TKxnmw?conversation=none

## 環境

- MacBook Air (M1, 2020)

```bash
$ sw_vers

ProductName:    macOS
ProductVersion: 12.2.1
BuildVersion:   21D62
```

## 開発準備

Laravel の実行に必要な開発環境を準備します。

> 以下の手順はmacOSを前提にしています。
> Windowsを利用している方は、適宜コマンドや手順を読み替えて実施してください。

### PHPをインストールする

こちらの手順通り PHP をインストールします。

https://www.php.net/manual/ja/install.macosx.packages.php

> Homebrewを利用するので、インストールされていない方は先にインストールしてください。

ターミナルで`php -v` `composer -v`を実行して、バージョンが表示されれば OK です。

```bash
$ php -v

PHP 8.1.6 (cli) (built: May 12 2022 23:30:39) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.6, Copyright (c) Zend Technologies
    with Zend OPcache v8.1.6, Copyright (c), by Zend Technologies
```

```bash
$ composer -v

   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 2.3.5 2022-04-13 16:43:00
```

### Laravelプロジェクトをスタートする

Laravel プロジェクトを始める方法はいくつかありますが、今回は公式ドキュメントで紹介されている`composerでのインストール`でやってみます。

https://readouble.com/laravel/9.x/ja/installation.html

下記をターミナルで実行します。

> `laravel-line-bot-trial`は好きなプロジェクト名に変更してOKです。

```bash
$ composer create-project laravel/laravel laravel-line-bot-trial
$ cd laravel-line-bot-trial
```

完了したら下記を実行し、開発サーバーを起動します。

```bash
$ php artisan serve

Starting Laravel development server: http://127.0.0.1:8000
[Sun May 29 19:02:34 2022] PHP 8.1.6 Development Server (http://127.0.0.1:8000) started
```

開発サーバーを立ち上げたら http://localhost:8000 にアクセスします。
以下のようなページが表示されれば OK です。

> サーバーを停止するときはターミナル上で`Cntl + C`を入力します。

[![Image from Gyazo](https://i.gyazo.com/857419d0ce27189794003ab037694e95.png)](https://gyazo.com/857419d0ce27189794003ab037694e95)

### LINE Messaging API SDKをインストールする

PHP で Messaging API を利用した LINE Bot を開発するため、公式で SDK が提供されています。

https://developers.line.biz/ja/docs/messaging-api/line-bot-sdk/

https://github.com/line/line-bot-sdk-php

こちらの SDK を利用するために、下記をターミナルで実行します。

```bash
$ composer require linecorp/line-bot-sdk
```

`composer.json`に`"linecorp/line-bot-sdk": "バージョン"`が追加されれば OK です。

```diff json
    "require": {
        "php": "^8.0.2",
        "guzzlehttp/guzzle": "^7.2",
        "laravel/framework": "^9.11",
        "laravel/sanctum": "^2.14.1",
        "laravel/tinker": "^2.7",
+        "linecorp/line-bot-sdk": "^7.5"
    },
```

### LINE公式アカウントを作成する

こちらの記事の手順を実施し、`チャネルシークレット`と`チャネルアクセストークン`をコピーしておきます。

https://zenn.dev/protoout/articles/16-line-bot-setup

## コーディング

本記事では、超シンプルに LINE Bot を実装するため、`Routes`に直接ロジックを記述します。

### オウム返しするコードを書く

以下の通り、コードを変更します。

`routes/app.php`

```diff php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
+ use LINE\LINEBot\HTTPClient\CurlHTTPClient;
+ use LINE\LINEBot;

(略)

Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});

+ $httpClient = new CurlHTTPClient($_ENV['LINE_CHANNEL_ACCESS_TOKEN']);
+ $bot = new LINEBot($httpClient, ['channelSecret' => $_ENV['LINE_CHANNEL_SECRET']]);

+ Route::post('/webhook', function (Request $request) use ($bot) {
+     $request->collect('events')->each(function ($event) use ($bot) {
+         $bot->replyText($event['replyToken'], $event['message']['text']);
+     });
+     return 'ok!';
+ });
```

`.env`

```diff
(略)
MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"

+ LINE_CHANNEL_ACCESS_TOKEN=ここにアクセストークンを記入する
+ LINE_CHANNEL_SECRET=ここにチャネルシークレットを記入する
```

### 動作確認する

ターミナルで下記を実行します（実行済みの方はそのままで OK です）

```bash
$ php artisan serve
```

curl でリクエストして、`ok!`と返ってくれば OK です。

```bash
$ curl -X POST "http://localhost:8000/api/webhook"

ok!
```

## Botを動かす

### localhost.runを利用してトンネリングする

`localhost.run`というサービスを利用して、ローカルの開発用サーバーに外部からアクセスできるようにします。

https://localhost.run/

> 簡易的に外部からアクセスするための手段として利用しています。
> 本番稼働の際は、別の手段を検討してください。

以下をターミナルで実行します。

```bash
$ ssh -R 80:localhost:8000 ssh.localhost.run
```

下の方に URL が表示されるのでコピーしておきます。
今回は`https://89e61725c1df04.lhrtunnel.link`でした。実行するごとに新しい URL が発行されます。

```
===============================================================================
Welcome to localhost.run!
(略)
https://localhost.run/docs/
===============================================================================

** your connection id is ada5e2c1-75e6-4655-b3b7-af569f56a345, please mention it if you send me a message about an issue. **

89e61725c1df04.lhrtunnel.link tunneled with tls termination, https://89e61725c1df04.lhrtunnel.link
```

### LINE DevelopersコンソールでWebhookの設定を行う

2 ステップです。

1. LINE Developers コンソールで`Webhook URL`を設定する
2. `Webhookの利用`を ON にする

#### LINE Developersコンソールで`Webhook URL`を設定する

[LINE公式アカウントを作成する](#LINE公式アカウントを作成する)の章で作成した LINE 公式アカウントの
`Messaging API設定 > Webhook URL設定`に[localhost.runを利用してトンネリングする](#localhost.runを利用してトンネリングする)でコピーした URL を貼り付けます。

末尾に`/api/webhook`をつけます。
末尾に`/api/webhook`をつけます。

> 忘れやすいので2回言いました。

[![Image from Gyazo](https://i.gyazo.com/628b7d177e29a9a8fd6049d132f94fd9.png)](https://gyazo.com/628b7d177e29a9a8fd6049d132f94fd9)

[![Image from Gyazo](https://i.gyazo.com/bb8f07d76e7d8f51b692d2d85fa3b054.png)](https://gyazo.com/bb8f07d76e7d8f51b692d2d85fa3b054)

#### `Webhookの利用`をONにする

[![Image from Gyazo](https://i.gyazo.com/60f9c952b0d289e543cc5c534c798754.png)](https://gyazo.com/60f9c952b0d289e543cc5c534c798754)

### 動作確認する

`検証`ボタンをクリックします。

`成功`と表示されれば OK です。

[![Image from Gyazo](https://i.gyazo.com/9de1b079ba6dc66708cee918b1aa2d63.png)](https://gyazo.com/9de1b079ba6dc66708cee918b1aa2d63)

では、実際に LINE で話しかけてみましょう！

こんな感じで、話しかけた内容がそのまま返ってきます。

https://twitter.com/mitsuoka0423/status/1522222293958934531?s=20&t=Acf2XNzs3MQMTpb0TKxnmw?conversation=none

## まとめ

Laravel で LINE Bot を超シンプルに作りました。
途中、ややこしい手順がありますが、LINE Bot 最初の一歩の記事になれば幸いです。

うまく行かない、不明点があるなどの場合は、Twitter で DM ください。
よかったらフォローもお願いします。

https://twitter.com/mitsuoka0423