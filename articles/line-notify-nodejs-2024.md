---
title: "LINE Notifyがサ終になるので、5年前に作ったline-notify-nodejsを振り返る"
emoji: "💬"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["line", "linenotify", "nodejs", "npm", "javascript"]
published: true
---

## はじめに

> この記事は、[〇〇アドベントカレンダー]()n日目の記事です
>
> n日目の記事： [TBU]()
> n日目の記事： [TBU]()

こんにちは。
DMM.comでDMMブックスの開発をしている[@mitsuoka0423](https://x.com/mitsuoka0423)です。
プライベートでは、LINE Develoer CommunityでLINE API Expertとしても活動しています。

2025年3月末でLINE Notifyがサービス終了することがアナウンスされました。

https://notify-bot.line.me/closing-announce

5年前に私が作ったline-notify-nodejsというnpmモジュールについて振り返ろうと思います。


## line-notify-nodejs を作ったきっかけ

- 当時、JSのクライアントライブラリがなく、みんなaxiosなどでAPIを叩いていた
- のびすけさんに言われて作った
- OSS活動をやってみたかった

## どれくらい使ってもらえたか

- ダウンロード数
  - npm trendsで集計
- リポジトリをいくつかピックアップ
- Issue/PR

## 振り返り

- こんなに沢山の人に使ってもらえるとは思わなかった
- メンテをもっとやればよかった

## おわりに

- LINE Notifyは素晴らしいサービスだった
- 使いやすさ向上に少しは役に立てたと思う
- 代替としてMessaging APIのプッシュメッセージを使ってほしい
