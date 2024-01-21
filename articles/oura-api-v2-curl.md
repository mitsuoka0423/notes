---
title: "curlでOura API V2を叩いてみたメモ #oura #api"
emoji: "👻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["curl", "oura", "ouraring", "api"]
published: true
---

## はじめに

昨日 Oura Ring Gen3 が届きました。

https://twitter.com/mitsuoka0423/status/1512748389498503173?s=20&t=iGk02lptPdq4yctcSlXX6g

ドキュメントを見ていると、API があったので触ってみます。
今回は 2022 年 1 月に公開された V2 を触っていきます。

## ドキュメント

https://cloud.ouraring.com/v2/docs

今回は 2022 年 1 月に公開された V2 を触っていきます。

V2 では、以下のデータも取得できるようになっているようです。

- 心拍数
- セッション
- タグ
- ワークアウト

また、V1 のときに取得できた以下のデータは今後公開予定とのことです。

- コンディション
- 睡眠
- 就寝時間

https://support.ouraring.com/hc/ja/articles/4415266939155-Oura-API-v2

## アクセストークンを取得する

こちらからトークンを発行できます。

https://cloud.ouraring.com/personal-access-tokens

以降の手順で使うのでコピーしておきましょう。

## curlでAPIを叩く

### Personal Infoを取得してみる

下記コマンドをターミナルで実行します。

> `3LGP...`の部分は`アクセストークンを取得する章`の手順で取得したアクセストークンに置き換えてください。

```bash
curl -i -X GET \
  -H "Authorization:Bearer 3LGP..." \
 'https://api.ouraring.com/v2/usercollection/personal_info'
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

### Daily アクティビティを取得してみる

> `3LGP...`の部分は`アクセストークンを取得する章`の手順で取得したアクセストークンに置き換えてください。

```bash
curl -i -X GET \
  -H "Authorization:Bearer 3LGP..." \
 'https://api.ouraring.com/v2/usercollection/daily_activity'
```

こちらもデータ取得できました。

```json
{
  "data": [
    {
      "class_5_min": "11111111111111111111111111111111111111111111111111111111112332323222",
      "score": 94,
      "active_calories": 7,
      "average_met_minutes": 1.03125,
      "contributors": {
        "meet_daily_targets": 78,
        "move_every_hour": 100,
        "recovery_time": 100,
        "stay_active": 100,
        "training_frequency": 100,
        "training_volume": 97
      },
      "equivalent_walking_distance": 64,
      "high_activity_met_minutes": 0,
      "high_activity_time": 0,
      "inactivity_alerts": 0,
      "low_activity_met_minutes": 6,
      "low_activity_time": 840,
      "medium_activity_met_minutes": 0,
      "medium_activity_time": 0,
      "met": {
        "interval": 60.0,
        "items": [
          0.9,
          (略)
        ],
        "timestamp": "2022-04-10T04:00:00.000+09:00"
      },
      "meters_to_target": 11600,
      "non_wear_time": 0,
      "resting_time": 17700,
      "sedentary_met_minutes": 2,
      "sedentary_time": 1860,
      "steps": 205,
      "target_calories": 500,
      "target_meters": 12000,
      "total_calories": 1487,
      "day": "2022-04-10",
      "timestamp": "2022-04-10T04:00:00+09:00"
    }
  ],
  "next_token": null
}
```

## まとめ

curl で Oura API V2 を叩いてみました。
アクセストークンも簡単に発行できる＆ドキュメントも整理されており、開発者に優しいですね。

## （余談）APIエンドポイントが2種類ある...？

ドキュメントを眺めていると、API エンドポイントのドメインが 2 種類ありました。

[![Image from Gyazo](https://i.gyazo.com/d9a3f6f4ffba54c2bcf3d55def0979e2.png)](https://gyazo.com/d9a3f6f4ffba54c2bcf3d55def0979e2)

試しに`https://cloud.ouraring.com/v2/usercollection/daily_activity`を叩いてみましたが、404 が返ってきました。

```bash
curl -i -X GET \
  -H "Authorization:Bearer 3LGP..." \
  'https://cloud.ouraring.com/v2/usercollection/daily_activity'
```

今後移行するのでしょうか。ウォッチしておこうと思います。
