---
title: "Laravelで超シンプルにLINE Botを作る（Webhook作成編） #laravel #messagingapi #php"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["PHP", "Laravel", "MessagingAPI", "LINE", "Docker"]
published: true
---



## はじめに

このシリーズでは、PHPフレームワークの一つである`Laravel`を使ったLINE Botの作り方を説明します。

http://laravel.jp/

https://developers.line.biz/ja/services/messaging-api/

- [Laravelで超シンプルにLINE Botを作る（開発準備編）](./laravel-line-helloworld-01.md)
- Laravelで超シンプルにLINE Botを作る（Webhookエンドポイント作成編）

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

## Webhookエンドポイントを作成する

`routes/api.php`のファイルを下記の通り編集します。

```diff php:routes/api.php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

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

- Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
-     return $request->user();
- });

+ Route::post('/', function (Request $request) {
+     return [
+         "data" => "hello!world",
+     ];
+ });
```

## 動作確認する

ターミナルで以下のコマンドを実行します。

```bash
$ curl -X POST "http://localhost/api/webhook"
```

以下のレスポンスが返ってきたらOKです。

```json
{"data":"hello!world"}
```

無事エンドポイントを生やすことができました。

## おわりに

次回の記事で、Messaging APIを利用してオウム返しするLINE Botを作成します。
