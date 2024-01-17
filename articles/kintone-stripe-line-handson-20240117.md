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

https://qiita.com/hideokamoto/items/061c4402096c047fc483

https://qiita.com/RyBB/items/c2ad4170e8acf0501803


## 利用するサービスの説明

### LINE (Messaging API)

https://line.me/ja/

- 国内月間アクティブユーザー：9,600 万人
- 友達登録するだけでアプリを利用してもらうことができる
- Messaging API を活用して、 LINE Bot を制作できる

### Make

https://www.make.com/en∏

- 様々なサービスを繋いでアプリを開発できるノーコードツール
- モジュールをつないで、シナリオを作成する


## LINE Botハンズオン

### 完成イメージ

#### シナリオ

![image](https://i.imgur.com/zeWTUDa.png)

#### LINE Bot

[![Image from Gyazo](https://i.gyazo.com/94e5bda2678dcf5bbc7a0154eeac8b07.gif)](https://gyazo.com/94e5bda2678dcf5bbc7a0154eeac8b07)

### 事前準備

本資料の内容を進めるためには下記の作業が必要です。

#### LINE

https://zenn.dev/protoout/articles/16-line-bot-setup

以下が終わっていれば OK です。

- [ ] LINE 公式アカウントが作成され、友達追加されていること
- [ ] チャネルアクセストークンが発行されていること

#### Make

https://zenn.dev/protoout/articles/12-integromat-signup

以下が終わっていれば OK です。

- [ ] Make にログインできていること

### Makeでシナリオを作成する

#### 前提

- [Make](https://www.Make.com/en/login) で操作

#### 操作イメージ

![image](https://i.imgur.com/A4lnHbh.png)

![image](https://i.imgur.com/PfGuDM3.png)

![image](https://i.imgur.com/71Jv9GF.png)

### `LINE`モジュールの`Watch Events`を追加 & 設定する

#### 操作イメージ

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

:::message
Make はこのあとの手順でも利用するので、タブで開きっぱなしにしておいてください。
:::

#### 操作イメージ

![image](https://i.imgur.com/6gLYAwO.png)

![image](https://i.imgur.com/CkPMYbG.png)

![image](https://i.imgur.com/MXdxHOe.png)

![image](https://i.imgur.com/02lLRRA.png)


### `LINE`モジュールの`Send a Reply Message`を追加 & 設定する

#### 前提

- Make で操作

#### 操作イメージ

![image](https://i.imgur.com/yShsjBh.png)

![image](https://i.imgur.com/J7cKn3A.png)

![image](https://i.imgur.com/bYaSqkZ.png)

![image](https://i.imgur.com/ryLyuNB.png)

![image](https://i.imgur.com/cVslXPH.png)

![image](https://i.imgur.com/r5XHXVd.png)

> くるくるしてればOK

### LINE にメッセージを送る

#### 操作イメージ

![image](https://i.imgur.com/sxa02C3.png)

> 送った文字がそのまま返ってくればOK

### LINE の User ID を確認する

#### 操作イメージ

![image](https://i.imgur.com/5TPbPX6.png)

### 購入リンクを返信する

#### 操作イメージ

![image](https://i.imgur.com/oPJrTHI.png)

![image](https://i.imgur.com/cVslXPH.png)

![image](https://i.imgur.com/4eQw6CE.png)

> 後の手順で決済用 URL を発行して埋め込みます

### 友達追加時にアンケートリンクを送る

下記の資料をご覧ください。

https://qiita.com/cog1t0/private/1b5f1b1f8f1d2c0add43?fbclid=IwAR1Ss_7TWz_2nK6z7lPqhXbltow0hi9xPD0tkssYiNe8c8jxjw-xtsLkZn8


## 他手順へのリンク

https://qiita.com/hideokamoto/items/061c4402096c047fc483

https://qiita.com/RyBB/items/c2ad4170e8acf0501803


## LINE Developer Community では様々なイベントを開催しています

ビジネスに活用できるような内容から、作ってみた系の LT イベントまで幅広く実施しています。

イベント参加は connpass から申し込み可能です。

https://linedevelopercommunity.connpass.com/

YouTube でイベントのアーカイブ動画を公開しています。

https://www.youtube.com/channel/UCZkYYwmvSA6y7WWLxM5x9IA
