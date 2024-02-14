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
