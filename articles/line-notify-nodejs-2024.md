---
title: "LINE Notifyがサ終になるので、5年前に作ったline-notify-nodejsを振り返る"
emoji: "💬"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["line", "linenotify", "nodejs", "npm", "javascript"]
published: true
---

## はじめに

> この記事は、[LINEDC Advent Calendar 2024](https://qiita.com/advent-calendar/2024/linedc)の10日目の記事です。
>
> 9日目の記事： [TBU]()
> 11日目の記事： [TBU]()

こんにちは。
DMM.comでDMMブックスを開発している[@mitsuoka0423](https://x.com/mitsuoka0423)です。
プライベートでは、LINE Develoer Communityで[LINE API Expert](https://developers.line.biz/ja/community/api-experts/jp-takahiro-mitsuoka/)としても活動しています。

2025年3月末でLINE Notifyがサービス終了することがアナウンスされました。

https://notify-bot.line.me/closing-announce

この記事では、5年前に私が作ったline-notify-nodejsというnpmモジュールについて振り返ろうと思います。


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
