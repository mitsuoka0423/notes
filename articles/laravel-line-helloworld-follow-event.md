---
title: "LaravelでLINE Botのフォローイベントをフックする #laravel #messagingapi #php"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["PHP", "Laravel", "MessagingAPI", "LINE"]
published: true
---

## はじめに

https://zenn.dev/tmitsuoka0423/articles/laravel-line-helloworld

の続きです。
前回作成したLINE Botを改修して、`フォローイベント`（友達登録時/ブロック解除時に発生するイベント）に応答できるように変更します。

少しハマったので、後半で説明します。

### 完成イメージ

https://twitter.com/mitsuoka0423/status/1532916722059341824?conversation=none

## 環境

- MacBook Air (M1, 2020)

```bash
$ sw_vers

ProductName:    macOS
ProductVersion: 12.2.1
BuildVersion:   21D62
```

## 参考URL

- [フォローイベント | Messaging APIリファレンス | LINE Developers](https://developers.line.biz/ja/reference/messaging-api/#follow-event)
- [line-bot-sdk-php/examples/EchoBot at master · line/line-bot-sdk-php](https://github.com/line/line-bot-sdk-php/tree/master/examples/EchoBot)

## 事前準備

下記記事の手順を実施し、オウム返しが動く状態にしておきます。

https://zenn.dev/tmitsuoka0423/articles/laravel-line-helloworld

## コーディング

`routes/app.php` を下記の通り変更します。

```diff php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use LINE\LINEBot;
use LINE\LINEBot\Constant\HTTPHeader;
+ use LINE\LINEBot\Event\FollowEvent;
+ use LINE\LINEBot\Event\MessageEvent\TextMessage;
+ use LINE\LINEBot\HTTPClient\CurlHTTPClient;

(略)

$httpClient = new CurlHTTPClient($_ENV['LINE_CHANNEL_ACCESS_TOKEN']);
$bot = new LINEBot($httpClient, ['channelSecret' => $_ENV['LINE_CHANNEL_SECRET']]);

Route::post('/webhook', function (Request $request) use ($bot) {

-    $request->collect('events')->each(function ($event) use ($bot) {
-        $bot->replyText($event['replyToken'], $event['message']['text']);
-    });

+    $signature = $request->header(HTTPHeader::LINE_SIGNATURE);
+    if (empty($signature)) {
+        return abort(400, 'Bad Request');
+    }
+
+    $events = $bot->parseEventRequest($request->getContent(), $signature);
+
+    collect($events)->each(function ($event) use ($bot) {
+        if ($event instanceof TextMessage) {
+            return $bot->replyText($event->getReplyToken(), $event->getText());
+        }
+        if ($event instanceof FollowEvent) {
+            return $bot->replyText($event->getReplyToken(), '[bot]友達登録されたよ！');
+        }
+    });

    return 'ok!';
});
```

## 変更概要

```diff php
+    $events = $bot->parseEventRequest($request->getContent(), $signature);
```

- `LINE\LINEBot`クラスの`parseEventRequest`メソッドを使うよう変更。署名検証もこのメソッドでできる。便利。
- 公式サンプルの使用例: [line-bot-sdk-php/Route.php at master · line/line-bot-sdk-php](https://github.com/line/line-bot-sdk-php/blob/master/examples/EchoBot/src/LINEBot/EchoBot/Route.php#L44)

```diff php
+        if ($event instanceof FollowEvent) {
+            return $bot->replyText($event->getReplyToken(), '[bot]友達登録されたよ！');
+        }
```

- フォローイベントのときの条件分岐を追加。
- 友達登録されたら〇〇したい、みたいな処理はこの中に書けばOK。
