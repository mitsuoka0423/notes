---
title: Replicate にサインインする & API キーを発行する
tags:
  - 'replicate'
  - 'AI'
  - '機械学習'
  - 'API'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

## はじめに

この記事では、 Replicate の利用を開始するための準備手順を説明します。


<!-- ../parts/replicate/about.md -->
## Replicate とは？

[![image](https://i.imgur.com/R4kKcAZ.png)](https://replicate.com/)

https://replicate.com/

- オープンソースなモデルの実行環境で、数千のモデルを利用可能
- APIが提供されており、機械学習やPythonの知識がなくても自身のアプリに組み込むことができる
- カテゴリや人気順などでモデルを探すことができる

![image](https://i.imgur.com/u1c4mhk.png)

### 価格

[Replicate](https://replicate.com/) を利用するのにかかる金額はマシンのスペックと実行時間によって決まります

https://replicate.com/pricing

![image](https://i.imgur.com/Km1qpzA.png)

> 計算例
>
> CPUのマシンで1分間実行した場合
> `$0.000100/sec x 60sec = $0.006 ≒ 0.882円`
>
> ※$1=147円で計算
> ※2024/02/07時点の情報
> 価格が変わっているがあるので詳細は必ず公式ページをご確認ください。



<!-- ../parts/replicate/registration.md -->
## Replicate の利用準備

### ゴール

- [ ] Replicate にサインインできていること
- [ ] クレジットカードの登録が完了していること
- [ ] API キーをコピーできていること

### 事前準備

- GitHub アカウント作成
  - 参考：[Creating an account on GitHub](https://docs.github.com/ja/get-started/start-your-journey/creating-an-account-on-github)

### Replicate にサインインする

#### 操作

- https://replicate.com/signin?next=/ を開く
- `Sign in with GitHub` を選ぶ
- ダッシュボードが表示されれば OK

#### イメージ

![image](https://i.imgur.com/Ibxaw28.png)

![image](https://i.imgur.com/ekFC5yr.png)

### クレジットカードを登録する

#### 操作

- https://replicate.com/account/billing を開く
- `Manage Billing` を選ぶ
- `支払い方法を追加` を選ぶ
- クレジットカード情報を入力して、`追加` を選ぶ
- 支払い方法が表示されれば OK

#### イメージ

![image](https://i.imgur.com/itzCq4s.png)

![image](https://i.imgur.com/Pb0AyqG.png)

![image](https://i.imgur.com/pMtyhDM.png)

![image](https://i.imgur.com/tXfye59.png)

### API キーを発行する

⚠ 悪用される危険があるので API キーは他の人に知られないようにしましょう ⚠

#### 操作

- https://replicate.com/account/api-tokens を開く
- `Token name` を入力し、 `Create token` を選ぶ
  - `Token name` は何でも OK です
- コピーボタンを選ぶ
  - 後で使うのでメモ帳などに貼り付けておきましょう
- `r8_******` のような文字がコピーできていればOK

#### イメージ

![image](https://i.imgur.com/YNhNtHK.png)

![image](https://i.imgur.com/0WW3avP.png)


