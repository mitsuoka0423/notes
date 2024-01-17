---
title: "Face APIのRecognition04モデルを使ってマスクをつけた顔認識を試してみる"
emoji: "📷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["azure", "cognitiveservices", "faceapi", "顔認識", "マスク"]
published: true
---

この記事は[Azureのカレンダー | Advent Calendar 2021 - Qiita](https://qiita.com/advent-calendar/2021/azure)の 25 日目の記事です。

## はじめに

こんにちは。[@mitsuoka0423](https://twitter.com/mitsuoka0423)です。
Face API の新機能として、今年の 2 月に`Recognition04モデル`が公開され、マスク付きの顔識別の性能が上がっているとのことだったので試してみました。

https://docs.microsoft.com/ja-jp/azure/cognitive-services/face/releasenotes#new-face-api-recognition-model

## 実験方法

### 学習方法

Azure Face API の Person Group を利用し、スマホで正面から撮影した画像を 5 枚ずつ学習させました。
学習させた人数は 2 人です。

利用した API と学習手順は以下のスクラップに記載しています。
https://zenn.dev/tmitsuoka0423/scraps/f5edb42f285c24

### 学習に用いた画像

| # | 画像 | 画像 | 画像 | 画像 | 画像 |
| -- | -- | -- | -- | -- | -- |
| Aさん | ![2-miki-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/93b28130-e8e2-981e-c342-2161f495a904.png) | ![3-miki-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/1646cea0-45a3-0b07-de3f-17c3b3c823da.png) | ![4-miki-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/f52e0fbd-a3f4-8f2d-7fbd-ec31e9ac1b1a.png) | ![1-miki-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/5bfa1e29-5016-bb03-0ecf-56b9df6379d4.png) | ![5-miki-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/0fbdfb66-d24f-7023-3025-461efdbe51bc.png) |
| Bさん | ![2-mitsu-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/1a3f6a81-7bc2-460c-13c1-913bba66c319.png)| ![1-mitsu-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/38b565dd-3370-1c54-adff-51eb214ae54f.png) | ![3-mitsu-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/5c4f8c88-1436-f0e5-2bd0-12f6cd927e5f.png) | ![4-mitsu-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/ccafffb3-764c-8d3b-7782-0f1acdcfed83.png) | ![5-mitsu-front.png.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/6f693c71-dce3-570f-97e8-c90d6a1d61c3.png) |

## 検証結果

学習させた画像とは別に、マスクを着用した状態の画像を撮影し推論しました。

結果に含まれる候補者と信頼度は、[Face APIのレスポンス](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239#:~:text=%22candidates%22%3A%20%5B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22personId%22%3A%20%2225985303%2Dc537%2D4467%2Db41d%2Dbdb45cd95ca1%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22confidence%22%3A%200.92%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D)に含まれる`candidates.personId`と`candidates.confidence`に対応しています。
信頼度は、1 に近いほど候補者である確率が高いです。

### 静止した状態で撮影した画像の推論結果

| # | パターン1 | パターン2 | パターン3 | パターン4 |
| -- | -- | -- | -- | -- |
| 画像 | ![Snap_2021-10-16_18-39-51.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/2f2176cd-db43-e0a9-59f6-33ed32e7ba85.png) | ![Snap_2021-10-16_18-40-56.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/5dbfbd75-2a42-d5f6-fb8e-33512df6f556.png) | ![Snap_2021-10-16_18-51-23.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/0e47087c-00f5-f049-9656-0dcccbf248bb.png) | ![Snap_2021-10-16_18-52-31.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/ee5fc1cf-a8a2-16eb-7777-ac62f2cfb17a.png) |
| 結果：候補者 | Aさん | Aさん | Aさん | Aさん |
| 結果：信頼度 | 0.82848 | 0.82989 | 0.84204 | 0.73857 |

### 歩いている状態を撮影した画像の推論結果

| # | パターン5 | パターン6 | パターン7 | パターン8 |
| -- | -- | -- | -- | -- |
| 画像 | ![スクリーンショット 2021-10-17 23.50.06.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/0a008fbb-5f52-0c5e-6d92-b1de3ffe8a56.png) | ![スクリーンショット 2021-10-17 23.50.30.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/dc13a746-3ecc-181b-6cb2-32a2581e2a6b.png) | ![スクリーンショット 2021-10-17 23.50.44.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/6d118a31-0648-391e-7912-9875a96e791e.png) | ![スクリーンショット 2021-10-17 23.50.59.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/b49bc2a2-a4e6-0c28-9178-f6f2c4280c63.png) |
| 結果：候補者 | Aさん | Aさん | Aさん | Aさん |
| 結果：信頼度 | 0.77256 | 0.86084 | 0.65705 | 0.70921 |


| # | パターン9 | パターン10 | パターン11 | パターン12 |
| -- | -- | -- | -- | -- |
| 画像 | ![スクリーンショット 2021-10-17 23.41.19.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/034508c5-518a-0cbc-1753-99db8753757c.png) | ![スクリーンショット 2021-10-17 23.41.33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/65bc32db-1cb9-2081-ff5c-27b8bd4451ac.png) | ![スクリーンショット 2021-10-17 23.40.50.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/838629ee-9520-720c-f79e-dec787020cf8.png) | ![スクリーンショット 2021-10-17 23.40.59.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/a9d0ce7c-3f23-7a2d-9648-b7d6f5abe9fe.png) |
| 結果：候補者 | Bさん | 候補者なし | Bさん | Bさん |
| 結果：信頼度 | 0.5043 | -- | 0.63694 | 0.57666 |

## 結果まとめ

- 今回試した画像では、2 人の識別を間違えることはなかった。
  - 歩いている状態を撮影した多少ブレがある画像でも 2 人の識別は可能であった。
  - 静止している状態を撮影した画像と比べると、信頼度は下がる。
  - 候補者なしになるケースもあった
- 信頼度は高くても 0.8 ほど。
  - 照明やブレが入ると、識別できなかったり、信頼度が 0.5 程度まで低くなる。
- マスクの色や形は結果に大きく影響しない。

## 感想

1 人 5 枚の画像だけの学習で、マスクを付けた顔写真の識別ができた。すごい。
顔の半分以上が隠れているため、さすがに信頼度は低くなる。
しかし、2 人のうちのどちらかを誤って識別することはなく、実用的なレベルであると感じた。
