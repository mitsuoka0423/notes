---
title: "Blob Storageの画像ファイルを取得してFace APIに渡すときの型についてメモ for TypeScript"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["azure", "faceapi", "blobstorage", "azurefunctions", "azure-sdk-for-js"]
published: true
---

## はじめに

Blob Storageに保存してある顔写真を取得してFace APIで顔検出するコードをTypeScriptで書いています。
型周りで少し苦戦したので、解決方法をメモしておきます。

## コード

```typescript
const downloadBlockBlobResponse = await blockBlobClient.download(0);

const faceList = await faceClient.face.detectWithStream(
  () => {
    return downloadBlockBlobResponse.readableStreamBody as NodeJS.ReadableStream;
  }
);
context.log(faceList);
```

## ポイント

- `downloadBlockBlobResponse.readableStreamBody as NodeJS.ReadableStream`
  - 型をキャスト(Node.js上で実行される場合は、`undefined`にならない)
- `() => {
    return downloadBlockBlobResponse.readableStreamBody as NodeJS.ReadableStream;
  }`
  - `detectWithStream`の引数の型に合わせる

## 型

- `blockBlobClient.download.readableStreamBody`の戻り値の型は`NodeJS.ReadableStream | undefined`
  - https://github.com/Azure/azure-sdk-for-js/blob/4da08a15a6fe25063854020cd7f8f685df4cd4ca/sdk/storage/storage-blob/src/BlobDownloadResponse.ts#L492
- `faceClient.face.detectWithStream`の引数の型は`() => NodeJS.ReadableStream`
  - https://github.com/Azure/azure-sdk-for-js/blob/e823b26f4600a698eb1c06f48da9b780fb2331a7/sdk/core/core-http/src/webResource.ts#L24
