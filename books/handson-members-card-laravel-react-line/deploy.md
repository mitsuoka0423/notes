---
title: "デプロイ"
---

## [WIP]バックエンドをHerokuにデプロイする

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
VITE_LIFF_MOCK_MODE=true # true | false
+VITE_LIFF_REDIRECT_URI=https://stellular-biscuit-5b1376.netlify.app/
VITE_LIFF_API_ENDPOINT=http://localhost:8000
VITE_LIFF_CODE_TYPE=barcode # barcode | qrcode
```

