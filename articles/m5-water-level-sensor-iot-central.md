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

## 