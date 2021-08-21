---
title: "opencv4nodejsで画像ファイルから顔検出するメモ"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["顔検出", "detection", "nodejs", "opencv", "opencv4nodejs"]
published: true
---

# はじめに

画像処理で有名な`OpenCV`のnpmライブラリ`opencv4nodejs`を使って、画像ファイルの顔検出を行ってみます。

https://opencv.org/

https://github.com/justadudewhohacks/opencv4nodejs

# インストール

```bash
npm install --save opencv4nodejs
```

# コード

> トップレベルawaitを使うために、`package.json`に以下のプロパティを追加します。
> 
> ```json
> "type": "module"
> ```

```javascript
import cv from 'opencv4nodejs';

const classifier = new cv.CascadeClassifier(cv.HAAR_FRONTALFACE_ALT2);

try {
    const img = await cv.imreadAsync('./lenna.jpg');
    const grayImg = await img.bgrToGrayAsync();
    const result = await classifier.detectMultiScaleAsync(grayImg);

    if (!result.objects.length) {
        throw new Error('failed to detect faces');
    }

    const minDetections = 10;
    result.objects.forEach((faceRect, i) => {
        if (result.numDetections[i] < minDetections) {
            return;
        }
        const rect = cv.drawDetection(
            img,
            faceRect,
            { color: new cv.Vec(255, 0, 0), segmentFraction: 4 }
        );

        cv.imwrite(`member${i}.jpg`, img.getRegion(faceRect));
    });
    cv.imshowWait('result', img);   
} catch (e) {
    console.error(e);
}
```

# 動作確認

```bash
node index.js
```

[![Image from Gyazo](https://i.gyazo.com/887241bf833faa98ebef0dff1822f34e.jpg)](https://gyazo.com/887241bf833faa98ebef0dff1822f34e)

顔検出できました。
