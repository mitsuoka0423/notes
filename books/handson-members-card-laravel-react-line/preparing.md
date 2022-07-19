---
title: "事前準備"
---

## Gitpodのログイン

下記URLにアクセスし、`Dashboard`ボタンをクリックします。

https://www.gitpod.io/

[![Image from Gyazo](https://i.gyazo.com/ea8a859be2c316aa0cc0197cf17ec991.png)](https://gyazo.com/ea8a859be2c316aa0cc0197cf17ec991)

`GitHub`、`GitLab`、`BitBucket`のいずれかでログインします。

[![Image from Gyazo](https://i.gyazo.com/50aa1ba66952106b5df46dcca2440a01.png)](https://gyazo.com/50aa1ba66952106b5df46dcca2440a01)

Workspaceが表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/5f9ccc9c470b955d7e8db5563bb1aa8a.png)](https://gyazo.com/5f9ccc9c470b955d7e8db5563bb1aa8a)

:::detail Gitpodを使わない方はこちらを実施してください

## PHPのインストール

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

## Node.jsのインストール

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

:::

## LINE Developersへログイン & 2つのキーを取得する

下記手順を参考に`チャネルシークレット`・`チャネルアクセストークン`を取得します。

https://zenn.dev/protoout/articles/16-line-bot-setup

チャネルシークレットとチャネルアクセストークンは後で使いますので、コピーしておきます。

## Airtable

### Airtableへログイン

下記ページの右上にある`Sign in`からログインします。
（アカウントをお持ちでない方はサインアップしてください）

https://www.airtable.com/

ログイン後にこのような画面が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/fec2236bbacd86c9eb74eb16018da6b7.png)](https://gyazo.com/fec2236bbacd86c9eb74eb16018da6b7)

### Airtableでテーブルを作成

`Add a base`をクリックしてベースを作成します。

[![Image from Gyazo](https://i.gyazo.com/4aa7d2c12a5fc64160cfebb1e806bf7b.png)](https://gyazo.com/4aa7d2c12a5fc64160cfebb1e806bf7b)

- ベース名を`Members Card`に変更します。
- テーブル名を`Members`に変更します。

[![Image from Gyazo](https://i.gyazo.com/7519989ec2a33d1eb7ca1c86a0e97d33.png)](https://gyazo.com/7519989ec2a33d1eb7ca1c86a0e97d33)

フィールド名とタイプを下記の通り変更します。

| 名前 | タイプ |
| -- | -- |
| `UserId` | `Single line text` |
| `Name` | `Single line text` |
| `MemberId` | `Single line text` |

下記の画像の通りになればOKです。

[![Image from Gyazo](https://i.gyazo.com/f1c442d1699f43ac605083a1e1e7f46d.png)](https://gyazo.com/f1c442d1699f43ac605083a1e1e7f46d)

### Airtableのbase IDを取得

`Members Card`ベースのURLの`app***********`をコピーしておきます。

[![Image from Gyazo](https://i.gyazo.com/f01a5de2812822c57ecb922639ae7eb3.png)](https://gyazo.com/f01a5de2812822c57ecb922639ae7eb3)

### AirtableのAPI Keyを取得

下記URLにアクセスします。

https://airtable.com/account

`Generate API Key`をクリックし、API Keyを生成します。

[![Image from Gyazo](https://i.gyazo.com/25f2b5395ed36143aa2ad2bb5dfb1c41.png)](https://gyazo.com/25f2b5395ed36143aa2ad2bb5dfb1c41)

紫背景の箇所をクリックすると、キーが表示されるのでコピーしておきます。

[![Image from Gyazo](https://i.gyazo.com/9972a23a9493da3bf29135da56894ea0.png)](https://gyazo.com/9972a23a9493da3bf29135da56894ea0)

## (デプロイされる方のみ)Netlifyへログイン

下記ページの右上にある`Log in`からログインします。
（アカウントをお持ちでない方はサインアップしてください）

https://www.netlify.com/

## (デプロイされる方のみ)Herokuへログイン

下記ページの右上にある`ログイン`からログインします。
（アカウントをお持ちでない方はサインアップしてください）

https://jp.heroku.com/
