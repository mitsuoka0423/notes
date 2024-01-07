---
title: "LINEパート - [kintone+Stripe+LINE]をノーコードで組み合わせて無人店舗を実現！"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["stripe", "kintone", "line", "messagingapi", "Make"]
# publication_name: line_dc
published: true
---


## はじめに

本資料は下記イベントの説明用資料です。

https://linedevelopercommunity.connpass.com/event/305413/

### 他手順へのリンク

- TODO: アンケート
- TODO: kintone
- TODO: Stripe


## 利用するサービスの説明

### LINE (Messaging API)

TODO

### Make

TODO


## このパートの完成イメージ

### シナリオ

![image](https://i.imgur.com/zeWTUDa.png)

### LINE Bot

[![Image from Gyazo](https://i.gyazo.com/94e5bda2678dcf5bbc7a0154eeac8b07.gif)](https://gyazo.com/94e5bda2678dcf5bbc7a0154eeac8b07)


## 事前準備

本資料の内容を進めるためには下記の作業が必要です。

### LINE
  - チャネルアクセストークンの取得（後の手順で利用）
### Make
  - ログイン


## Makeでシナリオを作成する

### 前提

- [Make](https://www.Make.com/en/login) で操作

### イメージ

![image](https://i.imgur.com/A4lnHbh.png)

![image](https://i.imgur.com/PfGuDM3.png)

![image](https://i.imgur.com/71Jv9GF.png)

## `LINE`モジュールの`Watch Events`を追加 & 設定する

### イメージ

![image](https://i.imgur.com/0KuGxkQ.png)

![image](https://i.imgur.com/VDMcCof.png)

![image](https://i.imgur.com/Cj5laiY.png)

![image](https://i.imgur.com/Ow07yBD.png)

![image](https://i.imgur.com/feQ1eAF.png)

![image](https://i.imgur.com/JZQxQo8.png)

![image](https://i.imgur.com/JsdcYTD.png)

![image](https://i.imgur.com/2xz781o.png)

![image](https://i.imgur.com/P9ReEPw.png)

> くるくるしてればOK

## 作成したチャネルにWebhook URLを設定する

### 前提

- [LINE Developesコンソール](https://developers.line.biz/console/)で操作
  - ログインがまだの方は、ログインしておいてください

### イメージ

![image](https://i.imgur.com/6gLYAwO.png)

![image](https://i.imgur.com/CkPMYbG.png)

![image](https://i.imgur.com/MXdxHOe.png)

![image](https://i.imgur.com/02lLRRA.png)


## `LINE`モジュールの`Send a Reply Message`を追加 & 設定する

### 前提

- Make で操作

### イメージ

![image](https://i.imgur.com/yShsjBh.png)

![image](https://i.imgur.com/J7cKn3A.png)

![image](https://i.imgur.com/bYaSqkZ.png)

![image](https://i.imgur.com/ryLyuNB.png)

![image](https://i.imgur.com/cVslXPH.png)

![image](https://i.imgur.com/r5XHXVd.png)

> くるくるしてればOK

## LINEにメッセージを送る

### イメージ

![image](https://i.imgur.com/sxa02C3.png)

> 送った文字がそのまま返ってくればOK

### 友達追加時にアンケートリンクを送る

TODO（桑原さんの資料へのリンクにするか）

### リッチメニューを設定する

TODO


## 他手順へのリンク

- TODO: アンケート
- TODO: kintone
- TODO: Stripe