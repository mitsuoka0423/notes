---
title: "M5 ATOM Matrixにつないだ水位センサーの値をAzure IoT Centralに送るメモ"
emoji: "☕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["M5Stack", "microPython", "UIFlow"]
published: true
---

## はじめに

最近水耕栽培を始めたのですが、たまにオートサイフォンが動作しなかったり、ポンプが止まったりして、根腐れしてしまうかもしれないので、水位を監視するシステムを作ろうと思います。

今回は、M5 ATOM Matrixに水位センサーをつないで、データをIoT Centralに送信してみます。

## 今回使うもの

| 名前 | URL | 金額(2022/04/30時点) |
| -- | -- | -- |
| M5 ATOM Matrix | https://www.switch-science.com/catalog/6260/ | 2321円 |
| 水位センサー | https://www.switch-science.com/catalog/6282/ | 1100円 |
| モバイルバッテリー | 適当なものを用意 | -- |
| USBケーブル | 適当なものを用意 | -- |

## デバイスのセットアップ

こちらの記事を参考にM5 ATOM Matrixに

https://qiita.com/yukimatsu/items/52c6b8435cc81d1c9dfa

