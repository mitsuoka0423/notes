---
title: '最近寒いので電気毛布のリモート制御にチャレンジする #SleepTech'
tags:
  - obniz
  - protooutStudio
  - 布団
  - SleepTech
  - レゴテクニック
private: false
updated_at: '2024-01-28T07:52:28+09:00'
id: ab3c7efb96e21bef5a3d
organization_url_name: null
slide: false
ignorePublish: false
---
プロトアウトスタジオアドベントカレンダー1 発目の記事です！

## はじめに

最近また一段と寒くなりましたね～。最低気温 4℃まで行きましたか。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/b8f21463-8614-d83a-edda-a7ab745ae057.png)

布団だけでは寒いので電気毛布をつけ始めました。
私はお風呂に入ってすぐ寝るので初めは`弱`でいいのですが、明け方の一番寒い時間帯では`中～強`にしたいです。

そんな希望を叶えるために、本記事ではリモートから電気毛布のコントローラ制御にトライしていきます。

## 用意するもの

- 電気毛布 2,000 円くらい
    - https://www.amazon.co.jp/gp/product/B0145YK65U/ref=ppx_yo_dt_b_asin_image_o05_s00?ie=UTF8&psc=1
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/c255e8bc-09d8-a52c-7a6d-8216477a19aa.png)

- obniz Board 4,500 円くらい
    - https://obniz.io/ja/products#obnizBoard

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/96a8b95c-390e-2eed-5cee-183656f4661d.png)

- GeekServo 1 つ 500 円くらい
    - https://www.amazon.co.jp/gp/product/B07WDJVWKQ/ref=ppx_yo_dt_b_asin_image_o01_s00?ie=UTF8&psc=1

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/6c1952ac-8f80-91f8-2f35-ed2ccd9a55f4.png)

- LEGO テクニックパーツ類合計 500 円くらい
    - https://item.rakuten.co.jp/brickers/6112-001/
    - https://item.rakuten.co.jp/brickers/3743-001/
    - https://item.rakuten.co.jp/brickers/3647-194/

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/a656ade5-9516-6150-98f0-c1986ba31fec.png)


合計 7,500 円くらいでした。

## 組み立て

### STEP1. LEGO テクニックパーツを組み立てる

買ったパーツをはめる

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/e5d122d0-59ad-0f1d-8863-f9aa7b343881.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/9d69e01d-36e3-6e15-cd25-7e024e128c98.png)

### STEP2. パーツを加工する

電気毛布のコントローラにくっつけるため、LEGO を少し削ります。
今回買ったパーツは加工しやすく、ニッパーとヤスリでできました。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/fffe4a66-6668-6cd7-25f3-08e7083ce4cc.png)

そしてくっつける

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/3dd40234-2c05-a902-b4a5-20e52edf9c44.png)

こんな感じ。接着は両面テープで行いました。

### STEP3. ケースに固定する

その辺にあったプラスチックの板に固定していきます（何かのフタ…？
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/bacfa54c-4305-15c0-11a3-8ed925407219.png)

GeekServo 用の穴をあける。ちょっと割れてしまいました

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/110d251c-0182-c6ed-69df-07316ae98093.png)

GeekServo をはめる

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/7ed37172-9954-bc56-4964-441ca9872e8c.png)


電気毛布のコントローラもくっつける

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/7ed13e7b-4646-70aa-cb4f-099ccb73a8e0.png)

ちゃんとギアがかみ合うように位置を調整します。
コントローラの接着も両面テープです。

これでハードは完成！
実際に動かしてみましょう。

## obnizからモーターを動かしてみる

今回は obniz の[JSパーツライブラリの画面](https://obniz.io/ja/sdk/parts/DCMotor/README.md)上から簡単に動かします。

### GeekServoとobnizを接続

GeekServo の赤ケーブルを 0 ピン、黒ケーブルを 1 ピンにつなぎます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/3855e7d9-53ad-c748-7e0b-84fa1302866b.png)

### JSパーツライブラリから動かす

[https://obniz.io/ja/sdk/parts/DCMotor/README.md](https://obniz.io/ja/sdk/parts/DCMotor/README.md)を開き、`Test Run`をクリック
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/82534714-3c89-8f18-73ab-48f71188d95f.png)

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">obnizでGeekServoを動かす <a href="https://twitter.com/hashtag/obniz?src=hash&amp;ref_src=twsrc%5Etfw">#obniz</a> <a href="https://t.co/Zq4K2R8hoT">pic.twitter.com/Zq4K2R8hoT</a></p>&mdash; Takahiro Mitsuoka (@t_mitsuoka) <a href="https://twitter.com/t_mitsuoka/status/1199995053164220421?ref_src=twsrc%5Etfw">November 28, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

動いた！
パーツライブラリがそのまま使えるなんてありがたい限りです。

## 電気毛布のコントローラを動かす

実際に動かしてみました。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">むむっ<br>接着かケースの強度が…(再掲) <a href="https://t.co/nXh82K75am">pic.twitter.com/nXh82K75am</a></p>&mdash; Takahiro Mitsuoka (@t_mitsuoka) <a href="https://twitter.com/t_mitsuoka/status/1211900173149859840?ref_src=twsrc%5Etfw">December 31, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

むむっ。残念ながらまだ実用化には至らなそうです。

## まとめ

個々のパーツは完成しましたが、残念ながらきれいには動きませんでした。
パーツとケースの固定方法をもう少しちゃんとやる必要がありそうです。

これから冬本番ということで、早めに実装してフィードバックを集めたいところです。

皆さんも夜は暖かくしておやすみください。

## 12/2の記事は…

@cog1t0 さんの[「初心者のためのobniz案内」](https://qiita.com/cog1t0/items/5e7d934ab1a2272d62d7)です！
