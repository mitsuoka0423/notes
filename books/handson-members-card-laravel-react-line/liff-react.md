---
title: "LINE LIFF + Reactで会員カードUI作成"
---

### チャネル(LINEログイン)を作成する

下記手順を参考にチャネルを作成します。
チャネルタイプは`LINEログイン`を選択します。

https://developers.line.biz/ja/docs/liff/getting-started/

### LIFFアプリを追加する

下記手順を参考に、さきほど作成したチャネルにLIFFアプリを追加します。

https://developers.line.biz/ja/docs/liff/registering-liff-apps/

表示される`LIFF ID`をコピーしておきます。

[![Image from Gyazo](https://i.gyazo.com/1981211fd6b768133002cad45730f50a.png)](https://gyazo.com/1981211fd6b768133002cad45730f50a)

### サンプルコードをダウンロードする

コードはこちらのリポジトリで公開しています。

https://github.com/mitsuoka0423/line-members-card-react-frontend

コードをzipでダウンロードして解凍します。（クローンでもOKです）

[![Image from Gyazo](https://i.gyazo.com/fabb3b1b77c007f07b0afe8585bbc4d7.png)](https://gyazo.com/fabb3b1b77c007f07b0afe8585bbc4d7)

### ライブラリをインストールする

ターミナルでさきほど解凍したフォルダーに移動し、下記を実行します。

```bash
yarn
```

以下のように表示されればOKです。

```log
✨  Done in 3.66s.
```

### 環境変数を設定する

ターミナルで下記を実行します。

```bash
cp .env.sample .env
```

`.env`ファイルが作成されるので、`VITE_LIFF_ID`に[LIFFアプリを追加する](#liffアプリを追加する)でコピーしたLIFF IDをペーストします。
その他の項目は変更しなくてOKです。

変更後のイメージは以下のようになります。

> ※LIFF IDの値は自身のものを使用してください。

```diff
+ VITE_LIFF_ID=1657187467-JglvQA3a
VITE_LIFF_MOCK_MODE=true # true | false
VITE_LIFF_REDIRECT_URI=https://localhost:3000
VITE_LIFF_API_ENDPOINT=http://localhost:8000
VITE_LIFF_CODE_TYPE=barcode # barcode | qrcode
```

### 実行する

ターミナルで書きを実行します。

```bash
yarn dev
```

以下のように表示されます。

```log
vite v2.9.9 dev server running at:

> Local: http://localhost:3000/
> Network: use `--host` to expose

ready in 225ms.
```

http://localhost:3000/ にアクセスして、下記のような画面が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/8c0445bcae717f2c3dbac403a96922bc.png)](https://gyazo.com/8c0445bcae717f2c3dbac403a96922bc)

以上でフロントエンドの準備は終わりです。
バックエンドの実装に進みましょう。

### [WIP] (オプション)フロントエンド実装手順

### LIFFプロジェクトを作成する

Create LIFF Appを利用してプロジェクトを作成します。
今回は`React`と`TypeScript`を使います。

https://developers.line.biz/ja/docs/liff/cli-tool-create-liff-app/

ターミナルで以下を実行します。

```bash
npx @line/create-liff-app
```

いくつか質問されるので、回答します。

> ? Enter your project name:  my-app
`my-app`にします。（好きなプロジェクト名に変更してOKです）

> ? Which template do you want to use? react
`react`を選択します。

> ? JavaScript or TypeScript? TypeScript
`TypeScript`を選択します。

> ? Please enter your LIFF ID: 
Don't you have LIFF ID? Check out https://developers.line.biz/ja/docs/liff/getting-started/ 1657231722-pA2V6Kj0


> ? Which package manager do you want to use? yarn
`yarn`を選択します。（`npm`でもOKです）

下記のように表示されればOKです。

```bash
Done! Now run: 

  cd my-app
  yarn dev
```

### 動作確認する

以下を実行し、ローカルで開発用サーバーを立ち上げます。

```bash
cd my-app
yarn dev
```

```log
vite v2.9.12 dev server running at:

> Local: http://localhost:3000/
> Network: use `--host` to expose

ready in 240ms.
```

http://localhost:3000/ にアクセスして、下記のような画面が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/d028c91d37c16468f2c9b9c22226561e.png)](https://gyazo.com/d028c91d37c16468f2c9b9c22226561e)

### ライブラリをインストールする

ターミナルで下記を実行します。

```bash
yarn add @chakra-ui/react @emotion/react@^11 @emotion/styled@^11 framer-motion@^6 qrcode.react react-barcodes
yarn add -D @line/liff-mock
```

> Chakra UI v2を利用するとなぜかエラーが発生するので、v1を利用しています。

### 会員カードUIを作成する

#### `liff.d.ts`を作成する

`my-app/src/liff.d.ts`を作成し、以下をコピペします。

```typescript
import { ExtendedInit, LiffMockApi } from '@line/liff-mock';

declare module '@line/liff' {
  interface Liff {
    init: ExtendedInit;
    $mock: LiffMockApi;
  }
}
```

#### `App.tsx`を変更する

`my-app/src/App.tsx`を開き、以下をコピペします。

```typescript
import { useEffect, useState } from "react";
import liff from "@line/liff";
import "./App.css";

function App() {
  const [message, setMessage] = useState("");
  const [error, setError] = useState("");

  useEffect(() => {
    liff
      .init({
        liffId: import.meta.env.VITE_LIFF_ID
      })
      .then(() => {
        setMessage("LIFF init succeeded.");
      })
      .catch((e: Error) => {
        setMessage("LIFF init failed.");
        setError(`${e}`);
      });
  });

  return (
    <ChakraProvider>
      <Box>
      </Box>
    </ChakraProvider>
  );
}

export default App;
```

### 動作確認する

ターミナルで以下を実行します。

```bash
yarn dev
```

http://localhost:3000/ にアクセスして、下記のような画面が表示されればOKです。

[![Image from Gyazo](https://i.gyazo.com/8c0445bcae717f2c3dbac403a96922bc.png)](https://gyazo.com/8c0445bcae717f2c3dbac403a96922bc)

以上でフロントエンドの実装は終わりです。
バックエンドの実装に進みましょう。
