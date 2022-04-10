---
title: "Node.jsでOura API V2を触ってみたメモ #oura #nodejs #api"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nodejs", "oura", "ouraring", "api"]
published: true
---

## はじめに

昨日Oura Ring Gen3が届きました。

https://twitter.com/mitsuoka0423/status/1512748389498503173?s=20&t=iGk02lptPdq4yctcSlXX6g

前回はcurlを使ってAPIを叩いたので、今回はNode.jsからAPIを叩いていこうと思います。
npmを探してみましたがV2のクライアントライブラリはまだないようだったので、axiosを使っていきます。

curlで叩いたときの記事はこちら。

https://zenn.dev/tmitsuoka0423/articles/oura-api-v2-curl

## ドキュメント

https://cloud.ouraring.com/v2/docs

今回は2022年1月に公開されたV2を触っていきます。

V2では、以下のデータも取得できるようになっているようです。

- 心拍数
- セッション
- タグ
- ワークアウト

また、V1のときに取得できた以下のデータは今後公開予定とのことです。

- コンディション
- 睡眠
- 就寝時間

https://support.ouraring.com/hc/ja/articles/4415266939155-Oura-API-v2

## 準備

### アクセストークンを取得する

こちらからトークンを発行できます。

https://cloud.ouraring.com/personal-access-tokens

以降の手順で使うのでコピーしておきましょう。

## Node.jsからAPIを叩く

下記のコードはこちらで公開しています。

https://github.com/mitsuoka0423/oura-api-v2-nodejs-sample

### Personal Infoを取得してみる

下記コマンドをターミナルで実行します。

> `3LGP...`の部分は`アクセストークンを取得する章`の手順で取得したアクセストークンに置き換えてください。

```js
const axios = require('axios');

const main = async () => {
  const response = await axios.get('https://api.ouraring.com/v2/usercollection/daily_activity', {
    headers: {
      Authorization: `Bearer 3LGP...`,
    }
  });
  console.log(response.data);
};

main();
```

データ取得できました。

```json
{
  "age": 30,
  "weight": 51.0,
  "height": 1.6,
  "biological_sex": "male",
  "email": "********@gmail.com"
}
```

### Daily Activityを取得してみる

> `3LGP...`の部分は`アクセストークンを取得する章`の手順で取得したアクセストークンに置き換えてください。

```bash
const axios = require('axios');

const main = async () => {
  const response = await axios.get('https://api.ouraring.com/v2/usercollection/daily_activity', {
    headers: {
      Authorization: `Bearer 3LGP...`,
    }
  });
  console.log(response.data);
};

main();
```

こちらもデータ取得できました。

```js
{
  data: [
    {
      class_5_min: '111111111111111111111111111111111111111111111111111111111123323232222222222222',
      score: 94,
      active_calories: 10,
      average_met_minutes: 1.09375,
      contributors: [Object],
      equivalent_walking_distance: 64,
      high_activity_met_minutes: 0,
      high_activity_time: 0,
      inactivity_alerts: 0,
      low_activity_met_minutes: 7,
      low_activity_time: 1020,
      medium_activity_met_minutes: 0,
      medium_activity_time: 0,
      met: [Object],
      meters_to_target: 11600,
      non_wear_time: 0,
      resting_time: 17700,
      sedentary_met_minutes: 4,
      sedentary_time: 4800,
      steps: 282,
      target_calories: 500,
      target_meters: 12000,
      total_calories: 1510,
      day: '2022-04-10',
      timestamp: '2022-04-10T04:00:00+09:00'
    }
  ],
  next_token: null
}
```

## まとめ

Node.jsからOura API V2を叩いてみました。
アクセストークンも簡単に発行できる＆ドキュメントも整理されており、開発者に優しいですね。
