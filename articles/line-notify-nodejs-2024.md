---
title: "LINE Notifyがサ終になるので、5年前に作ったline-notify-nodejsを振り返る"
emoji: "💬"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["line", "linenotify", "nodejs", "npm", "javascript"]
published: true
---

## はじめに

> この記事は、[LINEDC Advent Calendar 2024](https://qiita.com/advent-calendar/2024/linedc)の10日目の記事です。
>
> 9日目の記事： [ローカルメディアのLINE公式アカウントをGASで開発、頑張ってグロースした話【個人開発】](https://qiita.com/Tyamamoto1007/items/f183a82b02d75ab7af87)
> 11日目の記事： [LINE×生成AI：プロンプト戦闘ゲーム](https://qiita.com/RyuReina_Tech/items/b5c801bc0d17ecc76c48)

こんにちは。LINE開発者コミュニティで[LINE API Expert](https://developers.line.biz/ja/community/api-experts/jp-takahiro-mitsuoka/)として活動している[@mitsuoka0423](https://x.com/mitsuoka0423)です。
普段はDMM.comでDMMブックスのバックエンドを開発しています。

2025年3月末でLINE Notifyがサービス終了することがアナウンスされました。

https://notify-bot.line.me/closing-announce

この記事では、5年前に私が作ったline-notify-nodejsというnpmモジュールについて振り返ろうと思います。

## line-notify-nodejs を作ったきっかけ

- [プロトアウトスタジオ](https://protoout.studio/school)に通っていた時期で、[@n0bisuke](https://x.com/n0bisuke)さんからLINE Notifyの手頃なライブラリがないことを小耳に挟みました。
  - 当時はみんな[axios](https://github.com/axios/axios)でAPIを直接叩いていたようです。
- あとは単純にOSSを作ってみたかったというのもあります。

## 利用状況

### ダウンロード数

npm trendsで見てみます。
リリース直後からじわじわとダウンロード数が伸びていったようです。
ここ1年ほどは、Weeklyで最大600弱DLほどでした。

> ![](https://pbs.twimg.com/media/GeQx7rXaYAA0Amq?format=png&name=large)
> https://npmtrends.com/line-notify-vs-line-notify-nodejs-vs-line-notify-sdk-vs-line-notify-service

### Issue

自分で上げたもの以外では2つもらいました。

- [can update axios to v0.21.1? #6](https://github.com/mitsuoka0423/line-notify-nodejs/issues/6)
  - axiosのアップデート
- [How to put a text to new line. #29](https://github.com/mitsuoka0423/line-notify-nodejs/issues/29)
  - 改行の入れ方について質問

Issueをもらえると、利用者がどの辺に困っているかがわかりますね。
また、普段からIssueを見る習慣がなくて、いただいたIssueに気づくのにほぼ1年くらいかかってしまっています。
GitHub Actions&LINE Notifyで通知するなどしても良かったかなと思います。

## 作ってみて振り返り

初めて作ったnpmモジュールでしたが、こんなに沢山使ってもらえて嬉しいです。
使いやすい&間違った使い方ができないようなインターフェースを考えるのも楽しかったです。

反省点としては、Issueに気づけるようにしておくべきだったなと思います。
せっかくもらったユーザーの声は大事にしないと。

あとはTypeScriptに書き換えたり、ESM対応もできれば良かったかなと思います。
（一回チャレンジしたけど難しくて断念した）

## おわりに

- LINE Notifyは無料で通知を送れる素晴らしいサービスでした。サービス終了になるのが残念。
- LINE Notifyをより使いやすくするのに貢献できたかなと思います。
