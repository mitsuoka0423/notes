---
title: "GitpodのVSCode Latestバージョンを使うとシンタックスハイライトが動かないことがあるのでStableに切り替えたメモ"
emoji: "🐈"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["gitpod", "vscode", "editor"]
published: true
---

# はじめに

いつもハンズオンイベントで Gitpod を利用しているのですが、今日起動するとシンタックスハイライトがついておらず白黒でした。

[![Image from Gyazo](https://i.gyazo.com/17377a6003b415332a111597290a0b81.png)](https://gyazo.com/17377a6003b415332a111597290a0b81)

ツイートしたところ中の人から回答をもらえました。

https://twitter.com/akosyakov/status/1441415507748458498?s=20

どうやら Default IDE の Latest バージョンはアーリーリリースなので拡張機能のロードに失敗することがあるとのことで、Stable を使ってくれとのことでした。

Gitpod の Default IDE のバージョンを変更するのが少しわかりにくいところにあったので手順をメモします。

# GitpodのDefault IDEのバージョンを変更する

以下の手順で変更できます。
Gitpod の Web サイトからアクセスするのがミソです。

## GitpodのWebサイトを開く

https://www.gitpod.io/

## Preferences/Default IDEからバージョンを変更する

- `Login` or `Sign up`
- `Settings`
- `Preferences`
- `Default IDE`

から変更できます。

[![Image from Gyazo](https://i.gyazo.com/5c38ec6342261d9eeab9545be13e17e7.gif)](https://gyazo.com/5c38ec6342261d9eeab9545be13e17e7)

# まとめ

Gitpod が不調な場合は、Latest になっていない Stable バージョンを利用しましょう
無事に色がつきました。

[![Image from Gyazo](https://i.gyazo.com/7600c2354f31f073a8618e391ce53171.png)](https://gyazo.com/7600c2354f31f073a8618e391ce53171)