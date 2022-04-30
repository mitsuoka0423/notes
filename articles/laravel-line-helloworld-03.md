---
title: "Laravelで超シンプルにLINE Botを作る（ngork編） #laravel #messagingapi #php"
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
- [Laravelで超シンプルにLINE Botを作る（Webhookエンドポイント作成編）](./laravel-line-helloworld-02.md)
- Laravelで超シンプルにLINE Botを作る（ngrokインストール編）
- TODO: Laravelで超シンプルにLINE Botを作る（Messaging API編）

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

ngrokをダウンロードします。

[![Image from Gyazo](https://i.gyazo.com/f2ec72dfdc4b7f5aba2871fd02c7d0d8.png)](https://gyazo.com/f2ec72dfdc4b7f5aba2871fd02c7d0d8)

解凍して適当なフォルダに移動させます。
(今回は`/tmp`に入れました)

以下のコマンドを実行します。

[![Image from Gyazo](https://i.gyazo.com/f2ec72dfdc4b7f5aba2871fd02c7d0d8.png)](https://gyazo.com/f2ec72dfdc4b7f5aba2871fd02c7d0d8)

```bash
$ /tmp/ngrok config add-authtoken <トークン>
```

続いて、以下のコマンドを実行します。

```bash
$ /tmp/ngrok http 80
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

`Forwarding`に表示されるURLをコピーし、LINE Developersコンソールの`Webhook URL`に設定します。

> `/tmp/ngrok http 80`を実行するごとにURLが変わるので注意してください。
> URLの末尾に`/api/webhook`を追加してください。

[![Image from Gyazo](https://i.gyazo.com/20d3bd89290b676b0ceb9d328061473f.png)](https://gyazo.com/20d3bd89290b676b0ceb9d328061473f)

`検証`をクリックします。
`成功`と表示されればOKです。

> エラーになった場合は、以下を見直しましょう
> - ngrokで発行されたURLが古いものになっていないか
> - URL末尾に`/api/webhook`を追加しているか

[![Image from Gyazo](https://i.gyazo.com/329731a8134a257ab773389f0c8d08e6.png)](https://gyazo.com/329731a8134a257ab773389f0c8d08e6)

`Webhookの利用`をONにします。

[![Image from Gyazo](https://i.gyazo.com/c1ea5a57919a80d157533882e5b4df8f.png)](https://gyazo.com/c1ea5a57919a80d157533882e5b4df8f)

以上でngrokの設定は終わりです、

## おわりに

次回の記事で、Messaging APIを利用してオウム返しするLINE Botを作成します。
