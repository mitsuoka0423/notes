---
title: "イントロダクション"
---

### 作成するLINE Botのイメージ

[![Image from Gyazo](https://i.gyazo.com/41c390c4bfe4e87263b2cbebdfc6c1bc.png)](https://gyazo.com/41c390c4bfe4e87263b2cbebdfc6c1bc)

`会員カード`とメッセージを送信したら、会員カードバーコードを表示する。

[![Image from Gyazo](https://i.gyazo.com/dde82898893fea6e9ae46212dc19bb47.gif)](https://gyazo.com/dde82898893fea6e9ae46212dc19bb47)

リッチメニューをタップしたら、会員カードUIを表示する。

[![Image from Gyazo](https://i.gyazo.com/7585cc1593670d12a2e66769f0800567.gif)](https://gyazo.com/7585cc1593670d12a2e66769f0800567)

### ライブコーディング動画URL

TODO: 後ほど更新します。

### 著者

#### 光岡高宏/MITSUOKA Takahiro ([@mitsuoka0423](https://twitter.com/mitsuoka0423))

[![Image from Gyazo](https://i.gyazo.com/25ab3c97aff9ac1835bf82e0dc35e997.jpg)](https://gyazo.com/25ab3c97aff9ac1835bf82e0dc35e997)

電子書籍サービスを開発するエンジニア。
エンジニアではない人がモノづくり・制作を行い、現場を変えていく姿を見たいとの想いから「プロトアウトジム」立ち上げ。
毎週もくもく会やセミナーなどのをイベントを開催している。プロトアウトスタジオ5期＆7期講師。LINE BOOT AWARDオムロン賞受賞。
フリーランスとしてAzure IoT Hubを利用した高齢者見守りIoTデバイス・AIカメラの開発も行う。

https://www.value-press.com/pressrelease/295055

### 本資料の方針

- 「普段PHPを利用している開発者に、LINE Botを作る方法を伝える」ことを目的としています
  - フレームワークとしてバックエンドでは`Laravel`、フロントエンドでは`React`を利用していますが、それぞれの詳細な説明は行いません。
- 資料を見た方が再現しやすく、すべての処理を追いやすくするため、できるだけ単純なコードで実装するように心がけています。
  - コントローラーやサービス、モデルなどのクラスをあえて分けず、1クラスにロジックを書いています。
  - 必要に応じてリファクタリングをお願いします。
- 手順はmacOSを利用する前提で書いています。
  - WindowsやLinuxを利用している方は適宜読み替えてください。
