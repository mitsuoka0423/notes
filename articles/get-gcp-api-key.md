---
title: "API Keyを発行する手順 for Google Cloud API"
emoji: "🔑"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["gcp", "api"]
published: true
---

## はじめに

本記事は、[Google Cloud API](https://cloud.google.com/apis/docs/overview?hl=ja)を利用する際に使用するAPI Keyの発行方法について記載します。
例として、Google Cloud Vision APIを利用するケースについて紹介します。

公式ドキュメントはこちらを参考にしました。

https://cloud.google.com/vision/docs/before-you-begin?hl=ja

## 手順

### Google Cloud Platformにログインする

こちらからログインします。お持ちのGoogleアカウントでログインしましょう。

https://cloud.google.com/?hl=ja

<a href="https://gyazo.com/e210b3a45385cb7a6ee6ef3e01816cf8"><img src="https://i.gyazo.com/e210b3a45385cb7a6ee6ef3e01816cf8.png" alt="Image from Gyazo" width="1620"/></a>

右上の`無料で開始`ボタンをクリックします。

<a href="https://gyazo.com/054e50e9594ab6bf3abea5605dff85b0"><img src="https://i.gyazo.com/054e50e9594ab6bf3abea5605dff85b0.png" alt="Image from Gyazo" width="1620"/></a>

電話番号やクレジットカード番号が必要になるので用意しておきましょう。

> クレジットカードの登録を求められますが、登録後90日は300ドル相当のクレジットが提供される＆トライアル期間が終了した場合も自動で課金されることはないとのことです。(2022/04/03時点)
> <a href="https://gyazo.com/48ea9a36a06b585b234912697c2c50ae"><img src="https://i.gyazo.com/48ea9a36a06b585b234912697c2c50ae.png" alt="Image from Gyazo" width="1620"/></a>

下記のような画面になればOKです。

<a href="https://gyazo.com/90cac42bd9b4d6b81f3f8a3cba1ee13a"><img src="https://i.gyazo.com/90cac42bd9b4d6b81f3f8a3cba1ee13a.png" alt="Image from Gyazo" width="1620"/></a>

### （オプション）プロジェクトを作成する

こちらの手順はスキップしても大丈夫です。
プロジェクトごとに管理したい人のみ実施してください。

:::details 手順を見る場合はこちらをクリック

<a href="https://gyazo.com/ce2e8b9d01e87f3f504c1d12f74f3217"><img src="https://i.gyazo.com/ce2e8b9d01e87f3f504c1d12f74f3217.png" alt="Image from Gyazo" width="1405"/></a>

<a href="https://gyazo.com/67fbdcbef2dc44cfcc08999aebbc0241"><img src="https://i.gyazo.com/67fbdcbef2dc44cfcc08999aebbc0241.png" alt="Image from Gyazo" width="1405"/></a>

<a href="https://gyazo.com/ea6b26cf521b60738a41ad32989812e3"><img src="https://i.gyazo.com/ea6b26cf521b60738a41ad32989812e3.png" alt="Image from Gyazo" width="1405"/></a>

<a href="https://gyazo.com/e2ff34c596df6f92cf80e9467e24a5a8"><img src="https://i.gyazo.com/e2ff34c596df6f92cf80e9467e24a5a8.png" alt="Image from Gyazo" width="1405"/></a>

<a href="https://gyazo.com/e62df92cfa50cfa196fbd1b6d42e37b2"><img src="https://i.gyazo.com/e62df92cfa50cfa196fbd1b6d42e37b2.png" alt="Image from Gyazo" width="1405"/></a>

<a href="https://gyazo.com/e9c33b0d88bfdf801ae06c9123970b8e"><img src="https://i.gyazo.com/e9c33b0d88bfdf801ae06c9123970b8e.png" alt="Image from Gyazo" width="1620"/></a>

<a href="https://gyazo.com/abca27cadbd3ea7c4c4586156e7e6750"><img src="https://i.gyazo.com/abca27cadbd3ea7c4c4586156e7e6750.png" alt="Image from Gyazo" width="1620"/></a>

:::

### APIを有効にする

利用するAPIを有効にします。今回はCloud Vision APIを有効にする手順を紹介しますが、他のAPIでも同様です。

``

<a href="https://gyazo.com/07d68e17170de4a63f231336897734d2"><img src="https://i.gyazo.com/07d68e17170de4a63f231336897734d2.png" alt="Image from Gyazo" width="1620"/></a>

<a href="https://gyazo.com/6745877bf80322b1fbb2adea3b0b56e2"><img src="https://i.gyazo.com/6745877bf80322b1fbb2adea3b0b56e2.png" alt="Image from Gyazo" width="1620"/></a>

<a href="https://gyazo.com/c613836594f1b5603a9f52612f9b016d"><img src="https://i.gyazo.com/c613836594f1b5603a9f52612f9b016d.png" alt="Image from Gyazo" width="1620"/></a>

<a href="https://gyazo.com/12dd241dacce15fe079b4ad9b9e150db"><img src="https://i.gyazo.com/12dd241dacce15fe079b4ad9b9e150db.png" alt="Image from Gyazo" width="1620"/></a>

<a href="https://gyazo.com/607f469a4a1182180d5bdb13cce1b9be"><img src="https://i.gyazo.com/607f469a4a1182180d5bdb13cce1b9be.png" alt="Image from Gyazo" width="1620"/></a>

<a href="https://gyazo.com/81bb40d022b12090b8788e690a64fba4"><img src="https://i.gyazo.com/81bb40d022b12090b8788e690a64fba4.png" alt="Image from Gyazo" width="1620"/></a>

### APIキーを発行する

<a href="https://gyazo.com/5c35d0dd06f566effb53ab1355547500"><img src="https://i.gyazo.com/5c35d0dd06f566effb53ab1355547500.png" alt="Image from Gyazo" width="1620"/></a>

<a href="https://gyazo.com/76b88ad0dad0baf232fc19997e342d72"><img src="https://i.gyazo.com/76b88ad0dad0baf232fc19997e342d72.png" alt="Image from Gyazo" width="1620"/></a>

<a href="https://gyazo.com/cfdfc48bb5b26c5bd249b25b200917aa"><img src="https://i.gyazo.com/cfdfc48bb5b26c5bd249b25b200917aa.png" alt="Image from Gyazo" width="1620"/></a>

API Keyが発行されるので、こちらをコピーして利用しましょう。

> API Keyは外部に公開しないでください。
> 悪用されて高額な請求につながる可能性があるので、扱いには注意しましょう。

<a href="https://gyazo.com/882c1d0965f354f042c044312d3cd771"><img src="https://i.gyazo.com/882c1d0965f354f042c044312d3cd771.png" alt="Image from Gyazo" width="1620"/></a>

## 動作確認

MakeのGoogle Cloud Visionモジュールに設定してみます。

`connection`の`Add`をクリックし、`API Key`に先程コピーしたAPI Keyを貼り付けます。

<a href="https://gyazo.com/8acf487a9de0ea71a0ada10931a87c2a"><img src="https://i.gyazo.com/8acf487a9de0ea71a0ada10931a87c2a.png" alt="Image from Gyazo" width="788"/></a>

<a href="https://gyazo.com/514b93f82e76f94f2dd367c1486c365c"><img src="https://i.gyazo.com/514b93f82e76f94f2dd367c1486c365c.png" alt="Image from Gyazo" width="819"/></a>



## まとめ

Google Cloud APIのAPI Keyの発行方法について紹介しました。

