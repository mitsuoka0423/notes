---
title: "Laravelで超シンプルにLINE Botを作る（ngork編） #laravel #messagingapi #php"
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
- Laravel で超シンプルに LINE Bot を作る（ngrok インストール編）
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

## ngrokをインストールする

こちらからサインアップします。

https://ngrok.com/

画面に表示される通りにコマンドを実行します。

[![Image from Gyazo](https://i.gyazo.com/0ce5fabdca02498ba1e3e05b8d5f7024.png)](https://gyazo.com/0ce5fabdca02498ba1e3e05b8d5f7024)

`3. Fire it up`に書いてあるコマンドを実行し、以下のような表示になれば OK です。

```bash
$ ngrok http 80
```

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

以上で ngrok の設定は終わりです。

## おわりに

次回の記事で、Messaging API を利用してオウム返しする LINE Bot を作成します。

- [Laravelで超シンプルにLINE Botを作る（開発準備編）](./laravel-line-helloworld-01)
- [Laravelで超シンプルにLINE Botを作る（Webhookエンドポイント作成編）](./laravel-line-helloworld-02)
- Laravel で超シンプルに LINE Bot を作る（ngrok インストール編）
- [Laravelで超シンプルにLINE Botを作る（Messaging API編）](./laravel-line-helloworld-04)
