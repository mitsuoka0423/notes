---
title: "Laravelで超シンプルにLINE Botを作る（開発準備編） #laravel #messagingapi #php"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["PHP", "Laravel", "MessagingAPI", "LINE", "Docker"]
published: true
---

## はじめに

このシリーズでは、PHPフレームワークの一つである`Laravel`を使ったLINE Botの作り方を説明します。

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

## Laravelプロジェクトをスタートする

今回はローカルにはインストールせず、Dockerを利用してLaravelプロジェクトを実行する方法でやっていきます。

ドキュメントはこちらを参照しています。

https://readouble.com/laravel/9.x/ja/installation.html

> 以下の手順はMacを前提にしています。
> Windowsのパソコンを利用している方は、 https://readouble.com/laravel/9.x/ja/installation.html の`Windowsで始める`を参考に進めてください。

### Docker Desktopをインストール

こちらからDocker Desktopをインストールしましょう。

https://www.docker.com/products/docker-desktop/

`docker -v`と`docker compose version`をターミナルで実行し、バージョンが表示されればOKです。

```bash
$ docker -v

Docker version 20.10.14, build a224086
```

```bash
$ docker compose version

Docker Compose version v2.4.1
```

### Laravelプロジェクトを作成する

今回はデスクトップにLaravelプロジェクトを作成します。
以下のコマンドをターミナルで実行します。

```bash
$ cd ~/Desktop
$ curl -s "https://laravel.build/example-app" | bash
```

> `$`は「ターミナルで実行するよ」という目印なので、実際には入力しないでください。

実行すると以下のように表示され、途中でパスワードの入力を求められます。
入力すると、デスクトップに`example-app`フォルダが作成されます。

```log
 _                               _
| |                             | |
| |     __ _ _ __ __ ___   _____| |
| |    / _` | '__/ _` \ \ / / _ \ |
| |___| (_| | | | (_| |\ V /  __/ |
|______\__,_|_|  \__,_| \_/ \___|_|

Warning: TTY mode requires /dev/tty to be read/writable.
    Creating a "laravel/laravel" project at "./example-app"
    Info from https://repo.packagist.org: #StandWithUkraine
    Installing laravel/laravel (v9.1.6)
      - Downloading laravel/laravel (v9.1.6)
      - Installing laravel/laravel (v9.1.6): Extracting archive
    Created project in /opt/example-app
    > @php -r "file_exists('.env') || copy('.env.example', '.env');"
(略)
Thank you! We hope you build something incredible. Dive in with: cd example-app && ./vendor/bin/sail up
```

### Laravelプロジェクトを起動する

ターミナルで以下を実行します。

```bash
$ cd example-app && ./vendor/bin/sail up
```

起動したようです。

```bash
[+] Running 1/0
 ⠿ Container example-app-selenium-1  Running                                                                                                                                                                           0.0s
Attaching to example-app-laravel.test-1, example-app-mailhog-1, example-app-meilisearch-1, example-app-mysql-1, example-app-redis-1, example-app-selenium-1
example-app-redis-1         | 1:C 29 Apr 2022 00:19:11.679 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
(略)
example-app-laravel.test-1  | Starting Laravel development server: http://0.0.0.0:80
example-app-laravel.test-1  | [Fri Apr 29 00:19:15 2022] PHP 8.1.5 Development Server (http://0.0.0.0:80) started
```

### 起動を確認する

http://0.0.0.0:80/ にアクセスして、画面が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/4d882a96dbd2ab1d25e4c77d3e7e1c85.png)](https://gyazo.com/4d882a96dbd2ab1d25e4c77d3e7e1c85)

## おわりに

次回の記事で、LINE Bot用のWebhookエンドポイントを実装していきます。
