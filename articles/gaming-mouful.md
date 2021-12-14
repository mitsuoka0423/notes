---
title: "ゲーミング電気毛布を作る #mouful"
emoji: "🛏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["m5stack", "m5stickc", "iot", "uIflow", "電気毛布"]
published: true
---

この記事は[プロトアウトスタジオアドベントカレンダー 15日目](https://qiita.com/advent-calendar/2021/protoout)の記事です。

昨日の記事は、[ひろき (@wFnv1kzN7erqHyg)](https://twitter.com/wFnv1kzN7erqHyg)さんの[Teachable Machineを使って、ゆでたまごの気持ちをツンデレ風に表現してみた](https://qiita.com/tanakahiroki/items/9124b89a1e9c61572be1)でした。

## 電気毛布をめっちゃ光らせたい

こんにちは。プロトアウトスタジオ5期講師の[@mitsuoka0423](https://twitter.com/mitsuoka0423)です。

最近、すっかり寒くなりましたね。
我が家では毎年電気毛布を使っており、今年も昨日から使い始めました。
電気毛布を使い始めるのと同時に、IoT電気毛布`mouful`の開発が始まります。

今年は、ゲーミング電気毛布を開発するところからスタートしていきます。

## できたものはこちら

TODO: 動画

## ゲーミング電気毛布の作り方

### 準備するもの

- [M5StickC - スイッチサイエンス](https://www.switch-science.com/catalog/5517/)
- [M5Stack用NeoPixel互換 LEDテープ 100 cm - スイッチサイエンス](https://www.switch-science.com/catalog/5211/)

### 作り方

今回はサクッと開発するため、UIFlowを利用しました。
まず下記記事を参考に、M5Burnerを利用してM5StickCにファームウェアを書き込みます。

https://zenn.dev/tmitsuoka0423/scraps/411dda94799d3f

ブロックはこんな感じです。
ピカピカ光らせるために、乱数で色を設定しています。

[![Image from Gyazo](https://i.gyazo.com/41b53a5c8bfd2f6e0cd21532de366f78.png)](https://gyazo.com/41b53a5c8bfd2f6e0cd21532de366f78)

実際動かしてみるとこんな感じです。

[![Image from Gyazo](https://i.gyazo.com/c388c76aa0f03dba48ad13e5ef984c28.gif)](https://gyazo.com/c388c76aa0f03dba48ad13e5ef984c28)

これを電気毛布の上に置いて完成です！

TODO: 動画

## まとめ

UIFlowを使って、さくっとゲーミング電気毛布を作成しました。
UIFlowは初めて使いましたが、直感的にブロックを組み合わせるだけで処理を組み立てることができ、とても使いやすかったです。
温度センサーと組み合わせて、電気毛布の温度によって色を変えたりすると応用できそうです。

明日の担当は、[@okinakamasayos1](https://twitter.com/okinakamasayos1)さんです！
