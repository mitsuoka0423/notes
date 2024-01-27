---
title: obniz+赤外線LED+TypeScriptでリモコンコンセント(OCR-05W)を動かす
tags:
  - JavaScript
  - Node.js
  - TypeScript
  - IoT
  - obniz
private: false
updated_at: '2024-01-28T07:52:27+09:00'
id: 0652beb8a42d8daad6fb
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに

こんにちは。電気毛布エンジニアの[@tmitsuoka0423](https://twitter.com/tmitsuoka0423)です。

昨日書いた「[obniz+赤外線LEDでリモコンコンセント(OCR-05W)を動かす](https://qiita.com/tmisuoka0423/items/ac84514454857d157c60)」では、obniz のパーツライブラリページ上で動作確認しましたが、今回は TypeScript で実装していきます。
ソースコードは[https://github.com/tmitsuoka0423/obniz-ocr-05w](https://github.com/tmitsuoka0423/obniz-ocr-05w)で公開しています。

## 今回使うもの

| 品名 | 画像 | 価格 |
| --- | --- | --- |
| [obniz Board](https://obniz.myshopify.com/collections/devices/products/obniz) | <img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/19de797a-5105-21bc-8cc6-59c78da8d807.png" width="300"> | 5,500円くらい |
| [赤外線センサー OSRB38C9AA](http://akizukidenshi.com/catalog/g/gI-04659/) | <img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/2224085f-2ad5-f4f5-e9ba-9d550a740a16.png" width="300"> | 50円くらい |
| [赤外線LED OSI5FU5111C-40](http://akizukidenshi.com/catalog/g/gI-03261/) | <img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/c34ab511-18e3-748a-58c4-16f5ea6820fa.png" width="300"> | 20円くらい |
| [リモコンコンセント OCR-05W](https://www.amazon.co.jp/%E9%9B%BB%E6%A9%9F%E5%99%A8%E5%85%B7%E5%B0%82%E7%94%A8-%E3%83%AA%E3%83%A2%E3%82%B3%E3%83%B3%E3%82%B3%E3%83%B3%E3%82%BB%E3%83%B3%E3%83%88-%E5%93%81%E7%95%AA-07-8251-OCR-05W/dp/B01ABMGGQ8/ref=sr_1_fkmr0_2?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&keywords=cor-05W&qid=1585031707&sr=8-2-fkmr0) | <img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/001869d6-8bd2-dbc0-0593-212fe426207f.png" width="300"> | 1,200円くらい |

## obnizと素子を接続する

以下図のように接続します。
[![Screenshot from Gyazo](https://gyazo.com/f2085e2251e28d0b01763674e64e3d8e/raw)](https://gyazo.com/f2085e2251e28d0b01763674e64e3d8e)

## リモコンの赤外線信号を解析する

まずは、赤外線センサーを使ってリモコンの信号を受信するプログラムを作成します。
（確認してないですが、このプログラムを使って OCR-05W 以外のリモコンの信号も受信できると思います）。

```typescript:receive.ts
import Obniz from 'obniz';

const obniz = new Obniz('OBNIZ_ID_HERE');

obniz.onconnect = async () => {
  const sensor = obniz.wired('IRSensor', { vcc: 0, gnd: 1, output: 2 });
  console.log('リモコンのボタンを短く押してください…');

  sensor.start(function (arr) {
    console.log('受信したシグナル：');
    console.log(JSON.stringify(arr));

    obniz.close();
  });
}
```

これをコンパイルして実行する。
[![Screenshot from Gyazo](https://gyazo.com/2380c2b8b897e69486eafd6548df2119/raw)](https://gyazo.com/2380c2b8b897e69486eafd6548df2119)
と表示されるので、リモコンのボタンを押すと、めっちゃ長い信号が受信される。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">昨日から作っているobniz+赤外線LED+リモコンコンセントをTypeScriptで書き直し中。<br>まずはリモコン受信。<a href="https://twitter.com/hashtag/obniz?src=hash&amp;ref_src=twsrc%5Etfw">#obniz</a> <a href="https://twitter.com/hashtag/iot?src=hash&amp;ref_src=twsrc%5Etfw">#iot</a> <a href="https://t.co/Uj8A0TqliA">pic.twitter.com/Uj8A0TqliA</a></p>&mdash; Takahiro Mitsuoka (@tmitsuoka0423) <a href="https://twitter.com/tmitsuoka0423/status/1242659511157972997?ref_src=twsrc%5Etfw">March 25, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

無事受信できた。

## リモコンコンセントにobnizから信号を送信する

プログラムは以下の通り。

```typescript:send.ts
import Obniz from 'obniz';

const on: (0 | 1)[] = [1, 1, 1, 1, 1, (略)]; // 受信したリモコンの信号をコピペする。
const off: (0 | 1)[] = [1, 1, 1, 1, 1, (略)]; // GitHubに全量を載せています。

const obniz = new Obniz('OBNIZ_ID_HERE');

obniz.onconnect = async () => {
  const led = obniz.wired('InfraredLED', { anode: 3, cathode: 4 });
  led.send(on);
  // led.send(off);
  console.log('信号を送信しました。');

  obniz.close();
}
```

動かしてみる。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">昨日作ったobniz+赤外線LEDをTypeScriptで実装しました。<a href="https://t.co/ZjlVZz4pa7">https://t.co/ZjlVZz4pa7</a><br>TypeScriptは良い。<br><br>クラウドファンディング実施中”mouful”<br>1週間使ってみてくれる人、開発メンバー募集です！<a href="https://t.co/jrNHkUPUG7">https://t.co/jrNHkUPUG7</a><a href="https://twitter.com/hashtag/obniz?src=hash&amp;ref_src=twsrc%5Etfw">#obniz</a> <a href="https://twitter.com/hashtag/iot?src=hash&amp;ref_src=twsrc%5Etfw">#iot</a> <a href="https://twitter.com/hashtag/sleeptech?src=hash&amp;ref_src=twsrc%5Etfw">#sleeptech</a> <a href="https://twitter.com/hashtag/protoout?src=hash&amp;ref_src=twsrc%5Etfw">#protoout</a> <a href="https://t.co/lsb1HrJ8vO">pic.twitter.com/lsb1HrJ8vO</a></p>&mdash; Takahiro Mitsuoka (@tmitsuoka0423) <a href="https://twitter.com/tmitsuoka0423/status/1242668356118798336?ref_src=twsrc%5Etfw">March 25, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## まとめ

obniz＋TypeScript でリモコンコンセントの操作ができるようになりました。

JavaScript で書くのと比べて、TypeScript だとプロパティ名の補完が効くようになるのでオススメです。
gnd とか vcc とかピン名を調べる手間がなくなります。

次回は AWS にデプロイしていきます。
