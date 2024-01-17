---
title: "face-api.jsで顔検出するメモ for Node.js"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ai", "face-api", "nodejs"]
published: false
---

# はじめに

https://github.com/justadudewhohacks/face-api.js/

Node.js でも利用できるとのことなので試してみます。

https://github.com/justadudewhohacks/face-api.js/#face-api.js-for-nodejs

まずはシンプルに画像ファイルから顔検出してみます。

# 手順

## フォルダの作成＆セットアップする

> `$`はターミナルで実行するコマンドですよ、という目印です。
> 実際に実行する際は、コマンドから`$`を抜いてください。

```bash
$ mkdir nodejs-face-api-js-simple-sample
$ cd nodejs-face-api-js-simple-sample
$ npm init -y
```

## ライブラリをインストールする

```bash
$ npm i face-api.js canvas @tensorflow/tfjs-node
```

## コードを書く

公式の examples がそのまま動くのですが、わかりやすいように

- TypeScript -> JavaScript
- ファイルパスの整理

を行いました。

https://github.com/justadudewhohacks/face-api.js/blob/master/examples/examples-nodejs/faceDetection.ts

```javascript
import * as faceapi from 'face-api.js';

import { canvas, faceDetectionNet, faceDetectionOptions, saveFile } from './commons';

async function run() {

  await faceDetectionNet.loadFromDisk('../../weights')

  const img = await canvas.loadImage('../images/bbt1.jpg')
  const detections = await faceapi.detectAllFaces(img, faceDetectionOptions)

  const out = faceapi.createCanvasFromMedia(img)
  faceapi.draw.drawDetections(out, detections)

  saveFile('faceDetection.jpg', out.toBuffer('image/jpeg'))
  console.log('done, saved results to out/faceDetection.jpg')
}

run()
```