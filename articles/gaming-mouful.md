---
title: "2時間で作るゲーミング電気毛布 #mouful"
emoji: "🛏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["m5stack", "m5stickc", "iot", "uIflow", "電気毛布"]
published: true
---

この記事は[プロトアウトスタジオアドベントカレンダー 15日目](https://qiita.com/advent-calendar/2021/protoout)の記事です。

昨日の記事は、[ひろき (@wFnv1kzN7erqHyg)](https://twitter.com/wFnv1kzN7erqHyg)さんの[Teachable Machineを使って、ゆでたまごの気持ちをツンデレ風に表現してみた](https://qiita.com/tanakahiroki/items/9124b89a1e9c61572be1)でした。

## 電気毛布をめっちゃ光らせたい

こんにちは。プロトアウトスタジオ 5 期講師の[@mitsuoka0423](https://twitter.com/mitsuoka0423)です。

最近、すっかり寒くなりましたね。
我が家では毎年電気毛布を使っており、今年も昨日から使い始めました。

電気毛布を使い始めるのと同時に、IoT 電気毛布`mouful`の開発が始まります。
今までに、[スマートコンセントを使って電気毛布のON/OFF操作にチャレンジ](https://qiita.com/mitsuoka0423/items/582ff0c303abe8570ee5)したり、[布団温度によって電気毛布を制御](https://qiita.com/mitsuoka0423/items/ab3c7efb96e21bef5a3d)したりしてきました。

今年は、ゲーミング電気毛布をさくっと 2 時間で開発するところからスタートしたいと思います。

## できたものはこちら

https://youtu.be/ZNpoQY3Hzgs

## ゲーミング電気毛布の作り方

### 準備するもの

- [M5StickC - スイッチサイエンス](https://www.switch-science.com/catalog/5517/)
- [M5Stack用NeoPixel互換 LEDテープ 100 cm - スイッチサイエンス](https://www.switch-science.com/catalog/5211/)

### 作り方

今回は 2 時間で開発するために UIFlow を利用しました。
作業は大きく分けて 2 つあります。

1. M5StickC で UIFlow を使う準備
2. UIFlow でのブロックプログラミング

まず`M5StickC UIFlow`などで検索し、M5Burner を利用して M5StickC にファームウェアを書き込みます。

> 作業の過程は下記のスクラップにまとめています。
> https://zenn.dev/tmitsuoka0423/scraps/411dda94799d3f

ここまでだいたい 1 時間半かかりました。2 時間のほとんどを IFlow を使うための準備に使いました。
続いてブロックで処理を組み立てていきます。

ブロックはこんな感じです。
ピカピカ光らせるために、乱数で色を設定しています。

[![Image from Gyazo](https://i.gyazo.com/41b53a5c8bfd2f6e0cd21532de366f78.png)](https://gyazo.com/41b53a5c8bfd2f6e0cd21532de366f78)

実際動かしてみるとこんな感じです。

[![Image from Gyazo](https://i.gyazo.com/c388c76aa0f03dba48ad13e5ef984c28.gif)](https://gyazo.com/c388c76aa0f03dba48ad13e5ef984c28)

これを電気毛布の上に置いて完成です！

https://youtu.be/ZNpoQY3Hzgs

## まとめ

UIFlow を使って、さくっとゲーミング電気毛布を作成しました。
記事を書くのを含めて 3 時間以内で収めることができました。

UIFlow は初めて使いましたが、直感的にブロックを組み合わせるだけで処理を組み立てることができてとても使いやすかったです。
温度センサーと組み合わせて、電気毛布の温度によって色を変えたりすると応用できそうです。

明日の担当は、[@okinakamasayos1](https://twitter.com/okinakamasayos1)さんです！
