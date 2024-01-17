---
title: "Laravelで超シンプルにLINE Botを作る（Webhook作成編） #laravel #messagingapi #php"
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
- Laravel で超シンプルに LINE Bot を作る（Webhook エンドポイント作成編）
- [Laravelで超シンプルにLINE Botを作る（ngrokインストール編）](./laravel-line-helloworld-03)
- [Laravelで超シンプルにLINE Botを作る（Messaging API編）](./laravel-line-helloworld-04)

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

以下のレスポンスが返ってきたら OK です。

```json
{"data":"hello!world"}
```

無事エンドポイントを生やすことができました。

## おわりに

次回の記事で、Messaging API を利用してオウム返しする LINE Bot を作成します。

- [Laravelで超シンプルにLINE Botを作る（開発準備編）](./laravel-line-helloworld-01)
- Laravel で超シンプルに LINE Bot を作る（Webhook エンドポイント作成編）
- [Laravelで超シンプルにLINE Botを作る（ngrokインストール編）](./laravel-line-helloworld-03)
- [Laravelで超シンプルにLINE Botを作る（Messaging API編）](./laravel-line-helloworld-04)
