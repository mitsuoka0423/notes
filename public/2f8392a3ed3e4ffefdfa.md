---
title: 【5分でデプロイ】Deno Deployを使ってみた
tags:
  - JavaScript
  - TypeScript
  - Deno
  - denodeploy
private: false
updated_at: '2021-03-30T18:17:02+09:00'
id: 2f8392a3ed3e4ffefdfa
organization_url_name: null
slide: false
ignorePublish: false
---
<a href="https://gyazo.com/251c307f125017ac51ad1f371ba50722"><img src="https://i.gyazo.com/251c307f125017ac51ad1f371ba50722.png" alt="Image from Gyazo" width="969"/></a>

## Deno Deployとは

https://deno.com/deploy

## サインインする

<a href="https://gyazo.com/136f12a439a981fe65b92ebd5ece4221"><img src="https://i.gyazo.com/136f12a439a981fe65b92ebd5ece4221.png" alt="Image from Gyazo" width="969"/></a>

GitHub アカウントのログイン＆連携が求められるので許可します。

## プロジェクトを作成する

<a href="https://gyazo.com/c197ba9ded2ac144d31d353f09ba227d"><img src="https://i.gyazo.com/c197ba9ded2ac144d31d353f09ba227d.png" alt="Image from Gyazo" width="969"/></a>

<a href="https://gyazo.com/0671e4589f20570982a04b2b475c1d40"><img src="https://i.gyazo.com/0671e4589f20570982a04b2b475c1d40.png" alt="Image from Gyazo" width="969"/></a>

<a href="https://gyazo.com/e63799cf2381e1a7f85f272a0407f3d3"><img src="https://i.gyazo.com/e63799cf2381e1a7f85f272a0407f3d3.png" alt="Image from Gyazo" width="969"/></a>
プロジェクトが作成されました。

## デプロイするアプリを登録する

<a href="https://gyazo.com/9da07457f5fb0af1d13378940a56949d"><img src="https://i.gyazo.com/9da07457f5fb0af1d13378940a56949d.png" alt="Image from Gyazo" width="969"/></a>

本記事では、公式のサンプルアプリを登録してみます。
公式のリポジトリを Fork します。

<a href="https://gyazo.com/f947cd67818185aacaf2b7774e2505f2"><img src="https://i.gyazo.com/f947cd67818185aacaf2b7774e2505f2.png" alt="Image from Gyazo" width="969"/></a>

<a href="https://gyazo.com/de6fb403a360b9d47450633797d15d80"><img src="https://i.gyazo.com/de6fb403a360b9d47450633797d15d80.png" alt="Image from Gyazo" width="969"/></a>

Fork したリポジトリにある

<a href="https://gyazo.com/debcecc869894fbb7cccae654f76d9c4"><img src="https://i.gyazo.com/debcecc869894fbb7cccae654f76d9c4.png" alt="Image from Gyazo" width="969"/></a>

<a href="https://gyazo.com/674ff65529d7ad3c89650255769ca6fb"><img src="https://i.gyazo.com/674ff65529d7ad3c89650255769ca6fb.png" alt="Image from Gyazo" width="969"/></a>

デプロイ対象のリポジトリのみにアクセス可能にします。
（全リポジトリにアクセス可能にしたい場合は、`All repositories`でも OK）

<a href="https://gyazo.com/ebb705d83635634f0eee4cb9f339b0fc"><img src="https://i.gyazo.com/ebb705d83635634f0eee4cb9f339b0fc.png" alt="Image from Gyazo" width="969"/></a>
Deno Deploy とリポジトリがリンクされました。
<a href="https://gyazo.com/097eb6bdf91b25cb4d4343000af91835"><img src="https://i.gyazo.com/097eb6bdf91b25cb4d4343000af91835.png" alt="Image from Gyazo" width="969"/></a>

## デプロイしたアプリにアクセスしてみる

プロジェクトページに戻り、`Visit`をクリックします。
<a href="https://gyazo.com/8ffebb817efa95eea82a659815e4f762"><img src="https://i.gyazo.com/8ffebb817efa95eea82a659815e4f762.png" alt="Image from Gyazo" width="969"/></a>

<a href="https://gyazo.com/5373f20464ddc8e18eea4d9217dbfc81"><img src="https://i.gyazo.com/5373f20464ddc8e18eea4d9217dbfc81.png" alt="Image from Gyazo" width="969"/></a>
デプロイしたアプリの画面が表示されました！
<a href="https://gyazo.com/89947456cf4f7d4c80d4b285a27f08a5"><img src="https://i.gyazo.com/89947456cf4f7d4c80d4b285a27f08a5.png" alt="Image from Gyazo" width="969"/></a>

## まとめ

Deno Deploy にサンプルアプリをデプロイする手順を説明しました。
アプリを Deno で書いておけば、非常に簡単にデプロイできます。

[POSTも受け取れる](https://github.com/denoland/deploy_examples/blob/main/post_request/mod.js)ので Webhook エンドポイントとして動かすのも良いですね。
