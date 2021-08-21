---
title: "opencv4nodejsでbase64エンコードされた画像を読み込むメモ"
emoji: "📷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nodejs", "opencv", "opencv4nodejs"]
published: true
---

## コード

`cv.imdecode`を利用します。

https://justadudewhohacks.github.io/opencv4nodejs/docs/cv#imdecode

```javascript
import cv from 'opencv4nodejs';

const data = 'base64エンコードされたデータ';

const img = await cv.imdecodeAsync(data);
cv.imshowWait('result', img); 
```
