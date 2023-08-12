---
title: "若年性アルツハイマーの方のためにメモをタイムラインで遡れるアプリ「Sovinnon」を作った"
emoji: "👨‍💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["JavaScript", "WebSpeechAPI", "LINE", "React", "認知症フレンドリーテック"]
published: true
---

## 認知症フレンドリーテックハッカソンに参戦してきました

こんにちは！
LINE API Expert の [@mitsuoka0423](https://twitter.com/mitsuoka0423) です

2023/08/05-06でオンライン/オフラインハイブリッド開催された認知症フレンドリーテックハッカソンに参戦してきました

https://dementia-friendly-tech.connpass.com/event/282971/

> 私は、普段東京に住んでいるのですが、旅行も兼ねて福岡で現地参加しました

## 若年性アルツハイマーの方のためのメモをタイムラインで遡れるアプリ「Sovinnon」

ハッカソンのテーマが「認知症フレンドリーxテクノロジー」でしたので、
私達のチームでは、若年性アルツハイマーの方のためのヘルプマークデジタル化アプリ
「Sovinnon（ソビノン）」を作りました

こちらから無料で利用できますので、スマートフォンでご利用ください

https://mitsuoka0423.github.io/dementia-hackathon/

またコードはこちらで公開しています

https://github.com/mitsuoka0423/dementia-hackathon

> Sovinnonは、スウェーデン語で「交わる」という意味らしい

### 若年性アルツハイマーの方の抱える課題

下記2点に着目してプロダクトを検討しました

1. ぱっと見ても、健常者と見分けがつかない
  - 男性が女性に道を聞くと、ナンパと勘違いされることがあるとのこと
2. 直近のエピソード記憶がなくなりやすい

> ナンパと間違えられるのは、困りますよね、、

### 当事者はどうしているのか

1の課題については、ヒアリングしたところ[ヘルプマーク](https://www.fukushi.metro.tokyo.lg.jp/shougai/shougai_shisaku/helpmark.html)をよく利用しているとのことでした

2については、メモや写真に残して、あとで思い出せるようにしているとのことでした
ただメモを残すのが大変だったり、大量のメモや写真の中から必要な情報を見つけるのが大変という課題もあるようです

### 「Sovinnon」の解決策

Sovinnonでは、1と2の課題を解決するアプリの解決・軽減を目指し、以下の機能を実装しました

| ヘルプマーク表示 | プロフィール表示 | 音声メモ・写真のタイムライン表示 |
| -- | -- | -- | 
| [![Image from Gyazo](https://i.gyazo.com/96fd30ae7ec78729c77b423b10d9e343.png)](https://gyazo.com/96fd30ae7ec78729c77b423b10d9e343) | [![Image from Gyazo](https://i.gyazo.com/bc5077d9d52e8f08ef3e7d0d6a5e8642.png)](https://gyazo.com/bc5077d9d52e8f08ef3e7d0d6a5e8642) | [![Image from Gyazo](https://i.gyazo.com/1120ea63d4212b62ba6bba993b226f20.png)](https://gyazo.com/1120ea63d4212b62ba6bba993b226f20) |

誰かに助けを求めるときに、ヘルプマークを見せた上で、自身の過去を軽く伝え、質問する人に信用してもらえるような設計になっています

また、音声メモや写真を記録し、タイムライン形式で表示することで、あとで記憶を追えるようにしています
（写真撮影はハッカソン期間では未実装）

## 使った技術

[![Image from Gyazo](https://i.gyazo.com/20ce4af48fb1b64c6840e571a45b7c95.png)](https://gyazo.com/20ce4af48fb1b64c6840e571a45b7c95)

Sovinnonの実態はWebアプリで、音声メモの部分は[ブラウザ標準のWeb Speech APIのSpeech Recognition](https://developer.mozilla.org/ja/docs/Web/API/Web_Speech_API)を利用しており、フロントエンドのフレームワークはReactを利用しました

またユーザー専用のプロフィールやタイムラインを表示することを見据え、軽量でソーシャルログインを導入できる[LINEログイン](https://developers.line.biz/ja/services/line-login/)を採用しました

> LINEログインの導入は開発終盤に思いついて差し込んだのですが、1時間ほどであっさり導入できました

## ハッカソンの結果は...

残念ながら受賞できませんでした

審査員から

- アナログに紙とペンでメモを残すようにする方法があるなかで、デジタルにしたことで得られる明確なメリットが見えない
- 大量の紙と写真の中から必要な情報を探すのが大変なので、「情報を探す」部分に工夫があると良かった
  - 検索するにしても検索ワードを思い出す「想起」が難しいのでそこのサポートもあると良い

というフィードバックをいただきました
今後のアプリの改善で取り込んで行きたいと思います

## 実際に使ってみてフィードバックをくれる方を募集中！

私自身は若年性アルツハイマーではないし、知り合いにも認知症の方はいません
そのためこのアプリの良いところ・改善点がわかっていません

実際に課題を抱えていて、アプリ使ってみてもいいよ〜という方がいらっしゃったら、ぜひ使って、良し悪しフィードバックを[@mitsuka0423](https://twitter.com/mitsuoka0423)にください！
（DMでもなんでも大丈夫です）

Sovinnonはこちら↓から無料で使えます
https://mitsuoka0423.github.io/dementia-hackathon/


