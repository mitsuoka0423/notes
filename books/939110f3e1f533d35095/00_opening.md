---
title: "イントロダクション"
---

# 本記事について

本記事は、ハンズオンイベント「AIを使ったLINE Botを1時間で作ってみよう～プロトアウト体験会～」の説明資料です。

https://protoout.connpass.com/event/223612/

ソースコードはこちらで公開しています。

https://github.com/tmitsuoka0423/line-bot-azure-face-api-face-verification-handson

# （事前準備がまだ終わっていない方は...）

↓記事の手順を参考に、オープニングの間にコソッと進めておいてください！

https://qiita.com/tmitsuoka0423/private/b7b448d98121925c2a68

# ハンズオンにご参加いただきありがとうございます！

[![Image from Gyazo](https://i.gyazo.com/a5cfc3719be3c26b3d613dc9d200656d.png)](https://gyazo.com/a5cfc3719be3c26b3d613dc9d200656d)

# ハンズオン主催：プロトアウトスタジオ

[![Image from Gyazo](https://i.gyazo.com/40d9aa37f61706eb4222f27e7268eef4.png)](https://gyazo.com/40d9aa37f61706eb4222f27e7268eef4)

プロトアウトスタジオに興味をお持ちの方はこちらをご覧ください。

https://protoout.studio/

# スケジュール

| スタート | エンド | 内容 |
|--|--|--|
|08:00|08:10|オープニング|
|08:10|08:40|ハンズオン前半戦|
|08:40|08:50|休憩|
|08:50|09:20|ハンズオン後半戦|
|09:20|09:30|クロージング・アンケート回答|
|09:30|10:00|QA・サポートタイム|
# 講師: **光岡高宏**

[![Image from Gyazo](https://i.gyazo.com/8651c98b1d98c0e634dda8bdf4a02af7.jpg)](https://gyazo.com/8651c98b1d98c0e634dda8bdf4a02af7)

> 電子書籍サービスを開発するエンジニア。 LINE BOOT AWARDS 2018 オムロン賞受賞。  
> "より多くの人にモノづくりの楽しさを知ってもらいたい"という想いから、ハンズオンイベントを開催している。
>
> 寝ることが好きで、より快適な睡眠を実現するため、スマホで布団温度を調節できるIoTデバイス"mouful"を開発中。  
> プロトアウトスタジオ2期卒業生。

# 心構え

- まずは動くものを作ることをゴールにしましょう
  - コードを完璧に理解する必要は**全く**ありません
  - コードのこの辺をいじるとLINE Botのメッセージを変えられるんだ、という感覚を掴みましょう
- 講師をうまく活用しましょう
  - 質問はチャット欄に記入してもらえるとうれしいです。キリのいいタイミングで回答します。
  - 最後の30分に質問タイムを設けていますので、そちらも積極的に活用してください。
  - https://twitter.com/motcho_tw/status/870589211832795136
- とはいえ、1時間でハンズオンを進めるため、全員の完了を待たずに先に進むことがあります。ご承知おきください。
  - 今日のハンズオンは録画・公開しますので、ぜひ復習してください

# 今日作るもののイメージ

以下のようなLINE Botを作成していきます。

## 前半戦：オウム返しBot

[![Image from Gyazo](https://i.gyazo.com/94e5bda2678dcf5bbc7a0154eeac8b07.gif)](https://gyazo.com/94e5bda2678dcf5bbc7a0154eeac8b07)

## 後半戦：顔検証Bot

[![Image from Gyazo](https://i.gyazo.com/d59567d7e01a7f1ec2b6e134a474bbfe.gif)](https://gyazo.com/d59567d7e01a7f1ec2b6e134a474bbfe)

# 周知事項

[![Image from Gyazo](https://i.gyazo.com/09ab6bbfef636a90e5741bda0c03313a.png)](https://gyazo.com/09ab6bbfef636a90e5741bda0c03313a)

# それではハンズオンの内容に入っていきましょう！