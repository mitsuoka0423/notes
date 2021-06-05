---
title: "Azure Face APIを使って2つの顔が同一人物か判定する"
emoji: "😺"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AI", "Azure", "CognitiveServices", "Face"]
published: true
---

## はじめに

2枚の写真に写っている人物が同一人物であるかどうかを判定する`顔認識`を、Azure Face APIを使って行ってみます。

公式ドキュメントはこちら。

https://docs.microsoft.com/ja-jp/azure/cognitive-services/face/concepts/face-recognition

## Azure Face APIを使った顔認識ステップ

顔認識を行うには以下のステップを踏む必要があります。

1. `顔検出API`を利用し、比較したい2枚の写真の`FaceID`を取得する。
1. `確認API`に2つのFaceIDを送り、同一人物かどうかを判定する。

## 今回利用するAPIはこちら

### 顔検出API

写真に写った顔を検出するためのAPIです。
検出された顔にはそれぞれ`FaceID`が割り振られ、確認APIの実行に必要です。

https://japaneast.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236/console

### 確認API

指定した2つの`FaceID`の顔が同一人物かどうかを判定するAPIです。

https://japaneast.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a/console

## 実際に試してみる

今回はこちらのガッキーが同一人物かどうか判定してみます。

[![Image from Gyazo](https://i.gyazo.com/b1fa5fcd4f25b349c43f039cedb9b145.png)](https://gyazo.com/b1fa5fcd4f25b349c43f039cedb9b145)

まずは、[顔検出API](https://japaneast.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236/console
)を使って、FaceIDを取得します。
APIキーの取得方法は[こちら](https://zenn.dev/tmitsuoka0423/books/b21e50db77ff1eab89a3/viewer/chapter3#2.3.1.face-api%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)。

[![Image from Gyazo](https://i.gyazo.com/a9ade88befc15daa8aceb858aa9e60bd.png)](https://gyazo.com/a9ade88befc15daa8aceb858aa9e60bd)

画面下部にある`Send`ボタンをクリックするとFaceIDが返ってきます。
今回は`c2e933ef-8557-4306-a59f-7d86bc5ee30f`でした。

[![Image from Gyazo](https://i.gyazo.com/de264bd76e18651c44f2249ff7abd71c.png)](https://gyazo.com/de264bd76e18651c44f2249ff7abd71c)

2枚目の画像に対しても同様にFaceIDを取得します。
2つ目のFaceIDは`8729b426-700a-4901-96a9-0abb0c3d7320`でした。

続いて、[確認API](https://japaneast.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a/console
)を使います。

APIキーを入力し、Request bodyには先程取得したFaceIDを以下のフォーマットで入力します。

```
{
  "faceId1": "c2e933ef-8557-4306-a59f-7d86bc5ee30f",
  "faceId2": "8729b426-700a-4901-96a9-0abb0c3d7320"
}
```

[![Image from Gyazo](https://i.gyazo.com/6ebcd790654a9b2146e3fa586a9247aa.png)](https://gyazo.com/6ebcd790654a9b2146e3fa586a9247aa)

画面下部にある`Send`ボタンをクリックすると、同一人物度合いが返ってきます。
今回は、`96%`くらい同一人物という結果が得られました。

[![Image from Gyazo](https://i.gyazo.com/a9c905babb9a4736bd06303befdd3fad.png)](https://gyazo.com/a9c905babb9a4736bd06303befdd3fad)

## まとめ

Azure Face APIを使って、2枚のガッキーが同一人物か判定してみました。
`顔検出API`と`確認API`の2つのAPIを使わないといけませんが、手順自体はシンプルでわかりやすかったです。

とりあえず1パターンしか試していませんが、写真の明るさ、顔が写っている角度、距離などの条件を変えた場合、どれくらいの精度が出るかも見てみたいですね。
