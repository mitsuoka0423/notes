---
title: "Stripe Checkout の metadata に LINE User ID を渡すメモ"
emoji: "💰"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["stripe", "line", "messagingapi", "make"]
published: true
---


## はじめに

LINE Bot に決済機能を組み込みたかったので、 Stripe を利用して実現する方法をメモします


## 利用した Stripe の API

https://stripe.com/docs/api/checkout/sessions/create


## まずは JS で API を叩いてみる

### サンプルコード

```js
const params = new URLSearchParams();

params.append('success_url', 'https://example.com/success');
params.append('line_items[0][price]', 'price_1O6vxJH0HlXjxbVAzs06Dpyq');
params.append('line_items[0][quantity]', '1');
params.append('line_items[0][adjustable_quantity][enabled]', 'true'); // Checkout で数量を変更可能にする
params.append('mode', 'payment');
params.append('metadata[lineId]', 'L1234567890');

const response = await fetch('https://api.stripe.com/v1/checkout/sessions', {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${process.env.STRIPE_SECRET_KEY}`,
  },
  body: params,
});

const json = await response.json();
console.log(json.url);
```

### 実行結果

実行すると決済画面への URL が表示されます

```
https://checkout.stripe.com/c/pay/cs_test_a1Fo7Oj3MR4p2YPZAgE6L5zuZyn346F20XJSShidrZWL94ymNz2abH7Wm0#fidkdWxOYHwnPyd1blpxYHZxWnMwd01dfEZWc3ZGVD1ufXxdSHE0a1I8QDU1U3I9alI0dEsnKSdjd2poVmB3c2B3Jz9xd3BgKSdpZHxqcHFRfHVgJz8ndmxrYmlgWmxxYGgnKSdga2RnaWBVaWRmYG1qaWFgd3YnP3F3cGB4JSUl
```

開くとこんな感じ

![image](https://i.imgur.com/T9U8Y4L.png)

このまま決済します
すると、 Stripe の管理画面で metadata に含めた情報を確認することができます

![image](https://i.imgur.com/c3GC3nr.png)


## LINE Bot に組み込み、実際の LINE User ID を取得する

実装にはノーコードツールの make を利用します

https://make.com/

### シナリオ

![image](https://i.imgur.com/r4IVSfX.png)

### HTTP モジュールの設定

上記で説明した Stripe の Create Checkout Session を実行するよう設定します

![image](https://i.imgur.com/waEttVM.png)

![image](https://i.imgur.com/JPygfFY.png)

ポイントをかいつまんで説明します

- `success_url`
  - 決済が成功したあとに移動するURL
  - 今回は LINE のトークルームに返したかったので、LINE URL スキームを利用しています
  - ドキュメント： https://developers.line.biz/ja/docs/line-login/using-line-url-scheme/#opening-chat-screen
- `metadata[lineId]`
  - LINE User ID を入れたいので、 `Events > Source > User_ID` を選択

### LINE (Send a Reply Message) モジュールの設定

Create Checkout Session のレスポンスの `url` から決済画面を開くことができるので、
メッセージに `url` を入れます

![image](https://i.imgur.com/UloBmVJ.png)


## LINE Bot を動かしてみる

https://www.youtube.com/watch?v=Gh389pZD_Kw&list=PLO5zx9y5mzbl5zE-lESB-69nUbjPvSun2&index=2

決済が完了すると、 Stripe の管理画面に LINE の User ID が表示され、
無事 User ID を metadata にいれることができました

![image](https://i.imgur.com/b2onm0i.png)

