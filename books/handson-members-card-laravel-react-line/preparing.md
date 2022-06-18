---
title: "事前準備"
---

## 事前準備

### PHPのインストール

下記手順を参考にPHPをインストールします。

https://www.php.net/manual/ja/install.php

> Macの方はこちら
>
> https://www.php.net/manual/ja/install.macosx.php
> 
> Windowsの方はこちら
>
> https://www.php.net/manual/ja/install.windows.php

コマンドプロンプトやターミナルで下記のコマンドを実行し、バージョンが表示されればOKです。

```bash
php -v
```

```log
PHP 8.1.6 (cli) (built: May 12 2022 23:30:39) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.6, Copyright (c) Zend Technologies
    with Xdebug v3.1.5, Copyright (c) 2002-2022, by Derick Rethans
    with Zend OPcache v8.1.6, Copyright (c), by Zend Technologies
```

```
composer -v
```

```log
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 2.3.5 2022-04-13 16:43:00
```

### Node.jsのインストール

下記手順を参考にNode.jsをインストールします。

https://zenn.dev/protoout/articles/17-how-to-nodejs-install

コマンドプロンプトやターミナルで下記のコマンドを実行し、バージョンが表示されればOKです。

```bash
node -v
```

```log
v18.0.0
```

```bash
npm -v
```

```log
8.6.0
```

### LINE Developersへログイン & 2つのキーを取得する

下記手順を参考に`チャネルシークレット`・`チャネルアクセストークン`を取得します。

https://zenn.dev/protoout/articles/16-line-bot-setup

チャネルシークレットとチャネルアクセストークンは後で使いますので、コピーしておきます。

### Airtableへログイン

下記ページの右上にある`Sign in`からログインします。
（アカウントをお持ちでない方はサインアップしてください）

https://www.airtable.com/

ログイン後にこのような画面が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/fec2236bbacd86c9eb74eb16018da6b7.png)](https://gyazo.com/fec2236bbacd86c9eb74eb16018da6b7)

### (デプロイされる方のみ)Netlifyへログイン

下記ページの右上にある`Log in`からログインします。
（アカウントをお持ちでない方はサインアップしてください）

https://www.netlify.com/

### (デプロイされる方のみ)Herokuへログイン

下記ページの右上にある`ログイン`からログインします。
（アカウントをお持ちでない方はサインアップしてください）

https://jp.heroku.com/
