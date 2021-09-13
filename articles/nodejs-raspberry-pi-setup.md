---
title: "Raspberry Pi 4 Model BでNode.js LTSを使うための準備メモ"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nodejs", "raspberry pi", "raspberry"]
published: true
---

# 要約

- `sudo apt install nodejs`でインストールできるNode.jsはバージョンが少し古い
- ラズパイで動作するバイナリが公式で提供されているので、そちらを利用する
- ダウンロード～パスの設定までの手順をメモ

# はじめに

ラズパイでNode.jsを最も簡単にインストールする方法は`apt install`を利用することだと思います。

```bash
$ sudo apt install nodejs npm
```

ですが、インストールされるNode.jsのバージョンは`v10.24.0`で少し古いです。(2021/09/13時点)

[公式サイト](https://gyazo.com/c676aae41dc2411e22a7242a9d4e941d)にラズパイで動作するバイナリが公開されているので、そちらを利用する手順をまとめます。

# 環境

- [Raspberry Pi 4 Model B 8GB](https://www.switch-science.com/catalog/6370/)
- Raspberry Pi OS 32bit

# セットアップ手順

## Node.jsをダウンロードする

下記コマンドを実行します。
作業はホームディレクトリで行いました。

```bash
$ cd ~
$ wget https://nodejs.org/dist/v14.17.6/node-v14.17.6-linux-armv7l.tar.xz
```

URLは、その時のLTSのものを公式サイトで確認してください。

https://nodejs.org/ja/download/

[![Image from Gyazo](https://i.gyazo.com/839f1aaf8569306205d5027bc015ca88.png)](https://gyazo.com/839f1aaf8569306205d5027bc015ca88)

## 解凍する

下記コマンドを実行します。

```bash
$ tar Jxvf node-v14.17.6-linux-armv7l.tar.xz
```

成功すると、`node-v14.17.6-linux-armv7l`ディレクトリが生成されます。

## 動作確認する

下記コマンドを実行します。

```bash
$ ~/node-v14.17.6-linux-armv7l/bin/node -v
```

LTSと同じバージョンが表示されればOKです。

```bash
v14.17.6
```

## パスの設定を行う

いわゆる`パスを通す`というやつです。
下記のコマンドを実行します。

```bash
$ echo 'export PATH=$HOME/node-v14.17.6-linux-armv7l/bin:$PATH' > ~/.bashrc
$ source ~/.bashrc
```

パスが通ったか確認します。

```bash
$ node -v
v14.17.6
```

```bash
$ npm -v
6.14.15
```

```bash 
$ which node
/home/pi/node-v14.17.6-linux-armv7l/bin/node
```

```bash
$ which npm
/home/pi/node-v14.17.6-linux-armv7l/bin/npm
```

無事Node.jsを利用する準備ができました。
