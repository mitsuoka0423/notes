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

### 心拍数を取得してみる

> `3LGP...`の部分は`アクセストークンを取得する章`の手順で取得したアクセストークンに置き換えてください。

```bash
const axios = require('axios');

const main = async () => {
  const response = await axios.get('https://api.ouraring.com/v2/usercollection/heartrate', {
    headers: {
      Authorization: `Bearer 3LGP...`,
    }
  });
  console.log(response.data);
};

main();
```

こちらもデータ取得できました。

> timestampはUTCが表示されます。日本時間にするには+9時間してください。

```js
{
  data: [
    {
      bpm: 76,
      source: 'live',
      timestamp: '2022-04-09T11:20:05.160000+00:00'
    },
    {
      bpm: 72,
      source: 'awake',
      timestamp: '2022-04-09T11:28:12+00:00'
    },
    〜略〜
    {
      bpm: 58,
      source: 'awake',
      timestamp: '2022-04-10T00:50:29+00:00'
    }
  ],
  next_token: null
}
```

取得期間を指定する場合は、URLの後ろにパラメータをつけます。
パラメータはタイムスタンプで指定します。

```js
const axios = require('axios');

const main = async () => {
  const now = new Date().getTime();
  const twoHourAgo = now - (2 * 60 * 60 * 1000); 
  const response2 = await axios.get(`https://api.ouraring.com/v2/usercollection/heartrate?start_datetime=${twoHourAgo}&end_datetime=${now}`, {
    headers: {
      Authorization: `Bearer 3LGP...`,
    }
  });
  console.log(response2.data);
};

main();
```

指定した期間のデータのみが返ってくるようになりました。

```js
{
  data: [
    {
      bpm: 62,
      source: 'awake',
      timestamp: '2022-04-10T00:09:16+00:00'
    },
    {
      bpm: 64,
      source: 'awake',
      timestamp: '2022-04-10T00:09:22+00:00'
    },
    〜略〜
    {
      bpm: 58,
      source: 'awake',
      timestamp: '2022-04-10T00:50:29+00:00'
    }
  ],
  next_token: null
}
```

## まとめ

Node.jsからOura API V2を叩いてみました。
アクセストークンも簡単に発行できる＆ドキュメントも整理されており、開発者に優しいですね。
