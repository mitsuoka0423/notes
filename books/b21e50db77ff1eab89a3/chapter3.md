---
title: "画像分析AI(顔検出)と組み合わせよう"
---

# 2.1.この章のゴール

- LINE BotとAzure Face APIを組み合わせて、顔検出Botを作成する

## 2.1.1.完成イメージ

[![Image from Gyazo](https://i.gyazo.com/9b7a5bff129e46b7c3da41d050f902a6.gif)](https://gyazo.com/9b7a5bff129e46b7c3da41d050f902a6)

## 2.1.2.システム概要図

[![Image from Gyazo](https://i.gyazo.com/ed3534a28c900c0e87fd4b4593ae90bd.png)](https://gyazo.com/ed3534a28c900c0e87fd4b4593ae90bd)

# 2.2.話を聞くタイム

![https://i.gyazo.com/6529781dd996c64228080c383aa4a325.png](https://i.gyazo.com/6529781dd996c64228080c383aa4a325.png)

## 2.2.1.Microsoft Azure とは？

> 公式サイトはこちら：https://azure.microsoft.com/ja-jp/

ざっくり説明します。

- Microsoftが提供しているクラウドサービスのこと
- 200以上のサービスが提供されている
- AI・機械学習系のサービスも充実している

今日はAzureのサービスの内、AIを提供しているサービスである`Cognitive Service`の`Face API`を利用します。

## 2.2.2.Face APIの紹介

> 公式ドキュメントはこちら：https://azure.microsoft.com/ja-jp/services/cognitive-services/face/

`Face API`には大きく分けて3つの機能が提供されています。

- 顔検出
- 顔認証
- 感情認識

それぞれについて簡単に説明します。

### 2.2.2.1.顔検出

![https://i.gyazo.com/96883a0d6cec3835d1dd226f2787122f.png](https://i.gyazo.com/96883a0d6cec3835d1dd226f2787122f.png)

> 1人以上の人間の顔と各種の属性(年齢、感情、ポーズ、笑顔、顔ひげなど)を検出できます。  
> また、画像内の顔ごとに27個の特徴点が抽出されます。

### 2.2.2.2.顔認証

![https://i.gyazo.com/b95cfdb0061bed0eb926aa9340247015.png](https://i.gyazo.com/b95cfdb0061bed0eb926aa9340247015.png)

> 2つの顔が同一人物のものである可能性を検証し、信頼度スコアを取得します。

### 2.2.2.3.感情認識

![https://i.gyazo.com/94e6036245e1a74871c1720903da2455.png](https://i.gyazo.com/94e6036245e1a74871c1720903da2455.png)

> 怒り、軽蔑、嫌悪感、恐怖、喜び、中立、悲しみ、驚きなど、認識された表情を検出します。

`Face API`は、複雑なプログラミングをせずに利用できる**API**として提供されています。

## 2.2.3.ここまでのまとめ

- Azureとは、Microsoftが提供しているクラウドサービスのこと。
- AzureのAI・機械学習系のサービスの1つとして、**顔検出AI**が提供されている。
- 顔検出AIは**API**として提供されているので、簡単にプロダクトに組み込める。

# 2.3.手を動かすタイム

![https://i.gyazo.com/3600fb35b96dcd212cc0d4b6f3240e74.png](https://i.gyazo.com/3600fb35b96dcd212cc0d4b6f3240e74.png)

## 2.3.1.Face APIを使ってみよう

### 2.3.1.1.Azureポータルにログインする

Azureポータルを開きます。  
https://portal.azure.com/

ログインし、以下のようなページが表示されればOKです。

![https://i.gyazo.com/9c41bc6e713573818c8a2086a6e204be.png](https://i.gyazo.com/9c41bc6e713573818c8a2086a6e204be.png)

Face APIのリソースを作成します。

![https://i.gyazo.com/506f1466fe43354518e43cc5a3bd24fa.png](https://i.gyazo.com/506f1466fe43354518e43cc5a3bd24fa.png)

![https://i.gyazo.com/5201c4d4037788c001d79f1cdfef8725.png](https://i.gyazo.com/5201c4d4037788c001d79f1cdfef8725.png)

### 2.3.1.2.リソースグループを作成する

> 既にリソースグループを作成している方は作成不要です

リソースグループの`新規作成`をクリックし、リソースグループ名`handson-20210320`を入力します。

![https://i.gyazo.com/cd8c999ccca54a34eba230c44e926a48.png](https://i.gyazo.com/cd8c999ccca54a34eba230c44e926a48.png)

![https://i.gyazo.com/199465814a832fa28eb3c97955c9eb48.png](https://i.gyazo.com/199465814a832fa28eb3c97955c9eb48.png)

リソースグループが作成されました。

![https://i.gyazo.com/c9cb77de2d96a1974c24eaed0b967e9c.png](https://i.gyazo.com/c9cb77de2d96a1974c24eaed0b967e9c.png)

### 2.3.1.3.Face APIのリソースを作成する

残りの項目を埋めて、`確認および作成`をクリックします。

| 項目 | 値 | 備考 |
| -- | -- | -- |
| リージョン | 東日本 | 一番近い`東日本`を選択します。 |
| 名前 | `handson-{名前}-20210320` | ユニークな名前をつける必要があります。今日は名字を入れましょう。 |
| 価格レベル | `Free F0` | 無料のものを選択します |

> 同じサブスクリプションでは、無料のFace APIは一つしか作成できません。  
> 既に作成済みの場合はそちらを利用しましょう。  
> サブスクリプションを新しく作成してもOKです。

![https://i.gyazo.com/f574fe4dc34f2111a5993def294400e5.png](https://i.gyazo.com/f574fe4dc34f2111a5993def294400e5.png)

![https://i.gyazo.com/0a4485f141306087f5bc4f6273f04eb8.png](https://i.gyazo.com/0a4485f141306087f5bc4f6273f04eb8.png)

デプロイが完了したら、`リソースに移動`をクリックします。

![https://i.gyazo.com/bd110a382065d4c69ea82a7b41a9dc0c.png](https://i.gyazo.com/bd110a382065d4c69ea82a7b41a9dc0c.png)

`キー1`と`エンドポイント`をコピーします。

![https://i.gyazo.com/199c26da328160a466584e4f420b3f69.png](https://i.gyazo.com/199c26da328160a466584e4f420b3f69.png)

## 2.3.2.Face APIとLINE Botを組み合わせよう

### 2.3.2.1.コードにFace APIのキー・エンドポイントを記入する

Gitpodのタブを開きます。

> Gitpodを一度閉じてしまった人は、[ここ](https://gitpod.io/#https://github.com/tmitsuoka0423/line-bot-azure-face-api-handson)をクリックして再度開きましょう。  
> その際、LINE Developerコンソールから、`チャネルシークレット`と`チャネルアクセストークン`を再びコピーしてくる必要があります。

24行目あたりにある、`faceKey`・`faceEndPoint`にそれぞれ、先程コピーした`キー1`・`エンドポイント`をペーストします。

![https://i.gyazo.com/80f298f713ff96a00375a670ce6e6b5d.png](https://i.gyazo.com/80f298f713ff96a00375a670ce6e6b5d.png)

こんな感じになればOKです。

> `キー1`・`エンドポイント`の内容は人によって変わるので注意。

![https://i.gyazo.com/aa93ee89377d1cd8d9ac5bb68cfa88bb.png](https://i.gyazo.com/aa93ee89377d1cd8d9ac5bb68cfa88bb.png)

### 2.3.2.2.Expressを再起動する

ターミナルをクリックし、`Ctrl + C`を押して、Expressを一度停止させます。

![https://i.gyazo.com/8357e06087a090b6d7eba35bc7b52b09.png](https://i.gyazo.com/8357e06087a090b6d7eba35bc7b52b09.png)

`^C`が出ていれば停止できています。

![https://i.gyazo.com/2b7b172dea88a6999a4e8600ca427ea4.png](https://i.gyazo.com/2b7b172dea88a6999a4e8600ca427ea4.png)

ターミナルに`node index.js`と入力して、`Enter`を押します。

![https://i.gyazo.com/0cd665b2856038452f724002cad15e24.png](https://i.gyazo.com/0cd665b2856038452f724002cad15e24.png)

Expressが起動していることを確認します。

![https://i.gyazo.com/b50a862bf882e03edcf0fe5501d1e676.png](https://i.gyazo.com/b50a862bf882e03edcf0fe5501d1e676.png)

以上で、顔検出Botの設定は終わりです。

## 2.3.3.顔検出Botを動かしてみよう

Botに人の顔が写っている写真を送信してみましょう。  
起きている/寝ているが判定されて返ってきます。（精度は少し低いです。）

[![Image from Gyazo](https://i.gyazo.com/9b7a5bff129e46b7c3da41d050f902a6.gif)](https://gyazo.com/9b7a5bff129e46b7c3da41d050f902a6)

以上で顔検出Botの作成はおしまいです！

## 2.3.4.(オプション)ウインク判定を入れてみよう

起きている/寝ている判定に加え、ウインク（片目を閉じている）判定を加えてみましょう。

# 2.4.まとめ

- Azureが提供しているAI・機械学習系サービスであるFace APIを利用するため、Azure上でリソースを作成しました。
- LINE BotとFace API組み合わせて、送った写真の顔検出を行うBotを作成しました。
