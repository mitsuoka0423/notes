---
title: "LINEパート - [kintone+Stripe+LINE]をノーコードで組み合わせて無人店舗を実現！"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["stripe", "kintone", "line", "messagingapi", "Make"]
publication_name: line_dc
published: true
---


## はじめに

本資料は下記イベントの説明用資料です。

https://linedevelopercommunity.connpass.com/event/305413/

## 他手順へのリンク

https://qiita.com/hideokamoto/private/061c4402096c047fc483?fbclid=IwAR0k6GvsVu_h4snY_hnGZdqK83kckEjSXxntsNLhz9I6TIzmKgNI37YfdqE

- TODO: kintone


## 利用するサービスの説明

### LINE (Messaging API)

TODO

### Make

TODO


## LINE Botハンズオン

### 事前準備

#### LINE 公式アカウントを作成 & チャネルアクセストークンを取得する

- TODO: URL

#### Make にログインする

- TODO: URL

### 完成イメージ

#### シナリオ

![image](https://i.imgur.com/zeWTUDa.png)

#### LINE Bot

[![Image from Gyazo](https://i.gyazo.com/94e5bda2678dcf5bbc7a0154eeac8b07.gif)](https://gyazo.com/94e5bda2678dcf5bbc7a0154eeac8b07)

### 事前準備

本資料の内容を進めるためには下記の作業が必要です。

#### LINE

- チャネルアクセストークンの取得（後の手順で利用）

#### Make

- ログイン


### Makeでシナリオを作成する

#### 前提

- [Make](https://www.Make.com/en/login) で操作

#### イメージ

![image](https://i.imgur.com/A4lnHbh.png)

![image](https://i.imgur.com/PfGuDM3.png)

![image](https://i.imgur.com/71Jv9GF.png)

### `LINE`モジュールの`Watch Events`を追加 & 設定する

#### イメージ

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

### 作成したチャネルにWebhook URLを設定する

#### 前提

- [LINE Developesコンソール](https://developers.line.biz/console/)で操作
  - ログインがまだの方は、ログインしておいてください

#### イメージ

![image](https://i.imgur.com/6gLYAwO.png)

![image](https://i.imgur.com/CkPMYbG.png)

![image](https://i.imgur.com/MXdxHOe.png)

![image](https://i.imgur.com/02lLRRA.png)


### `LINE`モジュールの`Send a Reply Message`を追加 & 設定する

#### 前提

- Make で操作

#### イメージ

![image](https://i.imgur.com/yShsjBh.png)

![image](https://i.imgur.com/J7cKn3A.png)

![image](https://i.imgur.com/bYaSqkZ.png)

![image](https://i.imgur.com/ryLyuNB.png)

![image](https://i.imgur.com/cVslXPH.png)

![image](https://i.imgur.com/r5XHXVd.png)

> くるくるしてればOK

### LINEにメッセージを送る

#### イメージ

![image](https://i.imgur.com/sxa02C3.png)

> 送った文字がそのまま返ってくればOK

### LINEのUser IDを確認する

#### イメージ

![image](https://i.imgur.com/5TPbPX6.png)

### 購入リンクを返信する

#### イメージ

![image](https://i.imgur.com/oPJrTHI.png)

![image](https://i.imgur.com/cVslXPH.png)

![image](https://i.imgur.com/4eQw6CE.png)

> 後の手順で決済用 URL を発行して埋め込みます

### 友達追加時にアンケートリンクを送る

下記の資料をご覧ください。

https://qiita.com/cog1t0/private/1b5f1b1f8f1d2c0add43?fbclid=IwAR1Ss_7TWz_2nK6z7lPqhXbltow0hi9xPD0tkssYiNe8c8jxjw-xtsLkZn8

## 他手順へのリンク

https://qiita.com/hideokamoto/private/061c4402096c047fc483?fbclid=IwAR0k6GvsVu_h4snY_hnGZdqK83kckEjSXxntsNLhz9I6TIzmKgNI37YfdqE

- TODO: kintone
