---
title: "デプロイ"
---

## バックエンドをHerokuにデプロイする

:::message alert
現状、本コードをHerokuでビルドするとエラーが発生するため、
バックエンドコードのHerokuデプロイはスキップします。
:::

## フロントエンドをNetlifyにデプロイする

### ビルドする

:::message
ここからはVS Codeでの作業です。
:::

下記を実行してビルドします。

```bash
yarn build
```

下記のように表示されて、`dist`フォルダーにファイルが生成されていればOKです。

```log
yarn run v1.22.17
warning package.json: No license field
$ tsc && vite build
vite v2.9.9 building for production...
✓ 742 modules transformed.
dist/index.html                 2.20 KiB
dist/assets/index.b7ab0c04.js   483.36 KiB / gzip: 144.86 KiB
✨  Done in 6.08s.
```

[![Image from Gyazo](https://i.gyazo.com/361077db0c17057a36b29feb95e5241f.png)](https://gyazo.com/361077db0c17057a36b29feb95e5241f)

### Netlify にデプロイする

:::message
ここからは[Netlify](https://www.netlify.com/)での作業です。
:::

- [Netlify](https://www.netlify.com/)を開き、ログインします
- サイトを追加します

[![Image from Gyazo](https://i.gyazo.com/929f76e20b42ebf16c1687874f76f8af.png)](https://gyazo.com/929f76e20b42ebf16c1687874f76f8af)

[![Image from Gyazo](https://i.gyazo.com/2cf9382527729992a242e078681cf155.png)](https://gyazo.com/2cf9382527729992a242e078681cf155)

[![Image from Gyazo](https://i.gyazo.com/7aa090e4c5f53d7580237b5d0ce52dad.png)](https://gyazo.com/7aa090e4c5f53d7580237b5d0ce52dad)

`URL`をコピーします。

[![Image from Gyazo](https://i.gyazo.com/05b4f00e3a3d4e92ed11a2ab48adba33.png)](https://gyazo.com/05b4f00e3a3d4e92ed11a2ab48adba33)

### 発行されたURLをフロントエンドの環境変数に追加する

`.env`にさきほどコピーしたNetlifyのURLを`VITE_LIFF_REDIRECT_URI`に設定します。

```diff
VITE_LIFF_ID=1657262868-P5XDwM10
VITE_LIFF_MOCK_MODE=false # true | false
+VITE_LIFF_REDIRECT_URI=https://stellular-biscuit-5b1376.netlify.app/
VITE_LIFF_API_ENDPOINT=https://b7b3704c411571.lhrtunnel.link
VITE_LIFF_CODE_TYPE=barcode # barcode | qrcode
```

再び、

- [ビルドする](#ビルドする)
- [Netlify にデプロイする](#netlify-にデプロイする)

を実施します。

### LIFFアプリのエンドポイントURLを更新する

:::message
ここからは、[LINE Developersコンソール](https://developers.line.biz/ja/)での作業です。
:::

[LIFFアプリを登録する](https://zenn.dev/tmitsuoka0423/books/handson-members-card-laravel-react-line/viewer/liff-react#liff%E3%82%A2%E3%83%97%E3%83%AA%E3%82%92%E7%99%BB%E9%8C%B2%E3%81%99%E3%82%8B)で作成したLIFFアプリの`エンドポイントURL`をNetlifyのURLに変更します。

[![Image from Gyazo](https://i.gyazo.com/1d0416794c351bc1c704feff77053fff.png)](https://gyazo.com/1d0416794c351bc1c704feff77053fff)

更新できたら、LIFF URLにアクセスしましょう。

バーコードが表示されればOKです！

[![Image from Gyazo](https://i.gyazo.com/903ecef196a9200c2af07fec34d187c0.gif)](https://gyazo.com/903ecef196a9200c2af07fec34d187c0)


