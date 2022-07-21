---
title: "(Gitpod)LINE LIFF + Reactで会員カードUIを作成する"
---

コードはこちらのリポジトリで公開しています。

https://github.com/mitsuoka0423/line-members-card-react-frontend

:::message
この章では、フロントエンド用のGitpodワークスペースが追加され、バックエンドと合わせて2つのワークスペースを利用します。
どちらのワークスペースで作業をしているかを確認しながら作業しましょう。

資料では、`フロントエンドのGitpod`、`バックエンドのGitpod`と呼びます。
:::

## この章の完成イメージ

会員カードをブラウザで表示します。

[![Image from Gyazo](https://i.gyazo.com/903ecef196a9200c2af07fec34d187c0.gif)](https://gyazo.com/903ecef196a9200c2af07fec34d187c0)

## サンプルコードを動かしてみる

下記にアクセスし、`Open in Gitpod`を選択します。
フロントエンドのGitpodワークスペースが作成されます。

https://github.com/mitsuoka0423/line-members-card-react-frontend

[![Image from Gyazo](https://i.gyazo.com/99f3644cfcef37740928fea9e1dc90bb.png)](https://gyazo.com/99f3644cfcef37740928fea9e1dc90bb)

自動でライブラリのインストールとサーバー起動が行われます。
ターミナルが下記ような表示になっていればOKです。

[![Image from Gyazo](https://i.gyazo.com/c4a3c28619e35d6cb7a94cebc82f581e.png)](https://gyazo.com/c4a3c28619e35d6cb7a94cebc82f581e)

ポップアップが表示されたら`Make Public`を選択してください。

[![Image from Gyazo](https://i.gyazo.com/9b17f02cee8c53eea4e240e3df5c429a.png)](https://gyazo.com/9b17f02cee8c53eea4e240e3df5c429a)

`サイドバー > Remote Explorer`を選択し、地球マークをクリックします。

[![Image from Gyazo](https://i.gyazo.com/b4c2a23f55ecf78e87e54dfd5cab0e1f.png)](https://gyazo.com/b4c2a23f55ecf78e87e54dfd5cab0e1f)

下記のような画面が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/2a0b8df06c5a372d2f3aa258920803e8.png)](https://gyazo.com/2a0b8df06c5a372d2f3aa258920803e8)

## Airtableから会員IDを取得する

- バックエンドに会員情報取得APIを用意します。
- フロントエンドからAPIを叩いて会員情報を取得し、フロントエンドでバーコードを生成します。

### バックエンドの変更

:::message
ここからはバックエンドのGitpodでの作業です。
:::

`routes/api.php`の一番下に、以下のコードを追加します。

```diff php
<?php

use Illuminate\Http\Request;

（略）

    return 'ok!';
});

+// CORS
+Route::options('/members/{idToken}', function ($idToken) {
+    return response('ok', 200)
+        ->header('Access-Control-Allow-Origin', '*')
+        ->header('Access-Control-Allow-Methods', 'GET, OPTIONS')
+        ->header('Access-Control-Allow-Headers', 'Accept, X-Requested-With, Origin, Content-Type');
+});

+Route::get('/members/{idToken}', function ($idToken) use ($bot) {
+    Log::debug(['idToken' => $idToken]);
+
+    // IDトークンを検証する
+    // https://developers.line.biz/ja/reference/line-login/+#verify-id-token
+    $response = Http::asForm()->post('https://api.line.me/oauth2/v2.+1/verify', [
+        'id_token' => $idToken,
+        'client_id' => $_ENV['LINE_LOGIN_CHANNEL_ID'],
+    ]);
+    Log::debug(['response' => $response->json()]);
+
+    // LINEのユーザーIDと表示名を取得する
+    $userId = $response->json()['sub'];
+    $name = $response->json()['name'];
+
+    // 会員登録済みか確認するため、Airtableからデータを取得する
+    $member = Airtable::where('UserId', $userId)->get();
+
+    if ($member->isEmpty()) {
+        // Airtableに会員データがなければ、生成して登録する
+        $memberId = strval(rand(1000000000, 9999999999));
+        $member = Airtable::firstOrCreate([
+            'UserId' => $userId,
+            'Name' => $name,
+            'MemberId' => $memberId,
+        ]);
+        Log::debug('Member is created.');
+    } else {
+        // Airtableにデータがあれば、取得したデータを利用する
+        $memberId = $member->first()['fields']['MemberId'];
+    }
+
+    return $member->first()['fields'];
+});
```

`サイドバー > Remote Explorer`を選択し、地球マークをクリックします。

[![Image from Gyazo](https://i.gyazo.com/46e9efe74c1bb7b37d97f0ef30def60c.png)](https://gyazo.com/46e9efe74c1bb7b37d97f0ef30def60c)

URLをコピーします。この後の手順で利用します。

[![Image from Gyazo](https://i.gyazo.com/564ae3c4d2292689e7565c553c40d5ee.png)](https://gyazo.com/564ae3c4d2292689e7565c553c40d5ee)

### フロントエンドの変更

:::message
ここからはフロントエンドのGitpodでの作業です。
:::

`.env`の`VITE_LIFF_API_ENDPOINT`に、さきほどコピーしたURLを貼り付けます。

```diff
VITE_LIFF_ID=LIFF_ID_HERE
VITE_LIFF_MOCK_MODE=true # true | false
VITE_LIFF_REDIRECT_URI=
+VITE_LIFF_API_ENDPOINT=https://8000-mitsuoka042-laraveltemp-od6kvyaieml.ws-us54.gitpod.io
VITE_LIFF_CODE_TYPE=barcode # barcode | qrcode
```

フロントエンドのGitpodで、`サイドバー > Remote Explorer`を選択し、地球マークをクリックします。
URLをコピーします。この後の手順で利用します。

[![Image from Gyazo](https://i.gyazo.com/b4c2a23f55ecf78e87e54dfd5cab0e1f.png)](https://gyazo.com/b4c2a23f55ecf78e87e54dfd5cab0e1f)

### 動作確認する

さきほどコピーしたURLにアクセスします。

:::message
バックエンド、フロントエンド両方の開発用サーバーを起動しておく必要があります。
ターミナルで`php artisan serve`および`yarn dev`を実行した状態にしてください。
:::

下記のように、バーコードのIDがAPIから取得したものになっていればOKです。

[![Image from Gyazo](https://i.gyazo.com/214a13aedfde10023a3ba08c360930b4.png)](https://gyazo.com/214a13aedfde10023a3ba08c360930b4)

## LIFFアプリの設定を行う

### LIFFアプリを登録する

:::message
ここからは、[LINE Developersコンソール](https://developers.line.biz/ja/)での作業です。
:::

下記手順を参考にチャネルを作成します。
チャネルタイプは`LINEログイン`を選択します。

https://developers.line.biz/ja/docs/liff/getting-started/

下記手順を参考に、さきほど作成したチャネルにLIFFアプリを追加します。

https://developers.line.biz/ja/docs/liff/registering-liff-apps/#registering-liff-app

設定する値はこちらの様にします。

[![Image from Gyazo](https://i.gyazo.com/e257b90132e5c453160f01380817e0a3.png)](https://gyazo.com/e257b90132e5c453160f01380817e0a3)

表示される`LIFF ID`、`LIFF URL`をコピーしておきます。

[![Image from Gyazo](https://i.gyazo.com/01e157acb40a8fa27cca31f4948ca586.png)](https://gyazo.com/01e157acb40a8fa27cca31f4948ca586)

### localhostの証明書を設定する

:::message
ここからはVS Code（フロントエンドのプロジェクト）での作業です。
:::

こちらの記事を参考にさせていただき、`localhost-key.pem`と`localhost.pem`を生成します。

https://dev.classmethod.jp/articles/vite-https-localhost/

下記のように`localhost-key.pem`と`localhost.pem`が生成されればOKです。

[![Image from Gyazo](https://i.gyazo.com/2d6a8db22e4db276a91b34cbe43bf403.png)](https://gyazo.com/2d6a8db22e4db276a91b34cbe43bf403)

続いて`vite.config.ts`を下記の通り変更します。

```diff typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
+import fs from 'fs';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  build: {
    outDir: 'dist',
  },
+  server: {
+    https: {
+      key: fs.readFileSync('./localhost-key.pem'),
+      cert: fs.readFileSync('./localhost.pem'),
+    },
+  },
});
```

開発用サーバーを再起動すると、URLが`https`始まりになります。

```log
vite v2.9.9 dev server running at:

> Local: https://localhost:3000/
> Network: use `--host` to expose

ready in 171ms.
```

https://localhost:3000/ にアクセスして、バーコードが表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/3f367695f3e825ac5d8fc520b380b241.png)](https://gyazo.com/3f367695f3e825ac5d8fc520b380b241)


### 環境変数を設定する

:::message
バックエンドのプロジェクトで作業を行います。
:::

`.env`に`LINE_LOGIN_CHANNEL_ID`を追加します。

```diff
(略)
LINE_CHANNEL_ACCESS_TOKEN=xxxxxxxxxxxxxxxxxxx
LINE_CHANNEL_SECRET=xxxxxxxxxxxxxxxxxxxx
+LINE_LOGIN_CHANNEL_ID=12345678

AIRTABLE_KEY=keyXXXXXXXXXX
AIRTABLE_BASE=appXXXXXXXXXXXX
AIRTABLE_TABLE=Members
AIRTABLE_TYPECAST=false
```

:::message
フロントエンドのプロジェクトで作業を行います。
:::

`.env`を下記の通り変更します。

```diff
+VITE_LIFF_ID=1657231722-PwJ6Glrz
+VITE_LIFF_MOCK_MODE=false # true | false
+VITE_LIFF_REDIRECT_URI=https://localhost:3000
VITE_LIFF_API_ENDPOINT=https://b7b3704c411571.lhrtunnel.link
VITE_LIFF_CODE_TYPE=barcode # barcode | qrcode
```

### 動作確認する

[LIFFアプリを登録する](#liffアプリを登録する)でコピーしたLIFF URLにアクセスします。

[![Image from Gyazo](https://i.gyazo.com/b250123461d3d200a9519063aa4c5dca.png)](https://gyazo.com/b250123461d3d200a9519063aa4c5dca)

LINEへのログインが求められるので、ログインします。

[![Image from Gyazo](https://i.gyazo.com/51642832b08702b19fd906a83832fe76.png)](https://gyazo.com/51642832b08702b19fd906a83832fe76)

Airtableの`MemberId`と同じ番号が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/903ecef196a9200c2af07fec34d187c0.gif)](https://gyazo.com/903ecef196a9200c2af07fec34d187c0)

[![Image from Gyazo](https://i.gyazo.com/96481d73cc41af804b04b7b3432fb567.png)](https://gyazo.com/96481d73cc41af804b04b7b3432fb567)
