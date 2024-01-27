---
title: Vue.jsで作ったマークダウンエディタアプリをElectronでデスクトップアプリにする
tags:
  - Markdown
  - Vue.js
  - Electron
private: false
updated_at: '2019-12-23T08:40:02+09:00'
id: cbe15eacd6ef871f3fa3
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに

前回、[Auth0を使ってプライベートなマークダウンエディタを作る（クライアントサイド編）](https://qiita.com/tmisuo0423/items/9d55cfdb9bee7eef0b81)で、ブラウザで動作するマークダウンエディタのプロトタイプを作成しました。
本記事では、Electronを使ってデスクトップアプリ化しようと思います。

## システム構成図

本記事では、前回記事で作成したクライアントをElectronを使ってデスクトップアプリにしていきます。
![part.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/9ab04179-939c-4a65-9fd0-3f199e291bbf.png)

## Vue.jsプロジェクトをElectron化する

Vue.jsのプロジェクトは**2コマンド**実行するだけでElectron化できます。

### 前準備：Vue.jsプロジェクトを作成する

[Auth0を使ってプライベートなマークダウンエディタを作る（クライアントサイド編）](https://qiita.com/tmisuo0423/items/9d55cfdb9bee7eef0b81)の手順を一通り行い、[http://localhost:3000/](http://localhost:3000/)にアクセスすると以下のような画面が表示される状態にします。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/4aa377a6-8b61-e227-d0ed-3789d2b61c50.png)

### Electronのコマンドを実行する

`vue-cliプラグイン`の`electron-builder`を使うことでElectron化できます。
詳細はこちら→https://nklayman.github.io/vue-cli-plugin-electron-builder/

以下のコマンドを実行します。

```bash
$ npm i -g @vue/cli
$ vue add electron-builder
```

途中で、使用するElectronのバージョンを聞かれるので、最新の`6.0.0`を選択します。

```
$ npm i -g @vue/cli
$ vue add electron-builder

�📦  Installing vue-cli-plugin-electron-builder...


> electron-chromedriver@5.0.1 install C:\Users\taka\.ghq\github.com\mono0423\p-mark-down-editor\node_modules\electron-chromedriver
> node ./download-chromedriver.js

+ vue-cli-plugin-electron-builder@1.4.3
added 222 packages from 157 contributors and audited 30705 packages in 22.803s
found 7 moderate severity vulnerabilities
  run `npm audit fix` to fix them, or `npm audit` for details
✔  Successfully installed plugin: vue-cli-plugin-electron-builder

? Choose Electron Version ^6.0.0     <-- バージョンはとりあえず最新の6.0.0を選択（他のでも大丈夫です） 

～～～～～～～～～～～～～～～～～～～～～～～～略～～～～～～～～～～～～～～～～～～～～～～～～～～

✔  Successfully invoked generator for plugin: vue-cli-plugin-electron-builder
   The following files have been updated / added:

     src/background.js
     .gitignore
     package-lock.json
     package.json

   You should review these changes with git diff and commit them.
```

少し待つと、コマンドが成功するはずです。
`electron-builder`によって、以下4ファイルが追加・更新されたようなので、バージョン管理している場合は忘れずにコミット・プッシュしておきましょう。

  - src/background.js
  - .gitignore
  - package-lock.json
  - package.json

## いざ動作確認

`package.json`の`scripts`を見るといくつかエイリアスが追加されており、`$ npm run electron:serve`で起動できそうなので、実行してみます。

```bash
$ npm run electron:serve
npm WARN lifecycle The node binary used for scripts is C:\Program Files (x86)\Nodist\bin\node.exe but npm is using C:\Program Files (x86)\Nodist\v-x64\12.11.1\node.exe itself. Use the `--scripts-prepend-node-path` option to include the path for the node binary npm was executed with.
 DONE  Compiled successfully in 7536ms                                                                                                               15:58:18

  App running at:
  - Local:   http://localhost:3000/
  - Network: http://192.168.100.47:3000/

  Note that the development build is not optimized.
  To create a production build, run npm run build.

-  Bundling main process...

 DONE  Compiled successfully in 5384ms                                                                                                               15:58:23
  File                      Size                     Gzipped   

  dist_electron\index.js    651.00 KiB               148.88 KiB

  Images and other types of assets omitted.

 INFO  Launching Electron...
```

するとこんな画面が出てきます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/d24b0475-257e-d0e2-c746-7227268c12cb.png)

開発者ツールが表示されているので、×ボタンで閉じるとWebと同じ見た目になりました。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/e1fbb0db-cd16-943f-7497-756e828fb9c8.png)

Auth0を使ったGoogleログインもでき、マークダウンエディタの機能も壊れることなくそのままデスクトップアプリとして動かすことができました。
![2019-12-14_16h01_31.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/e22e826e-3126-8598-152c-fa338754f3d7.gif)

## まとめ

Vue.jsで作成したWebアプリは、vue-cliプラグインの`electron-builder`の力を借りることで、**2コマンド**だけでデスクトップアプリにすることができました。（うち1つはvue-cliのインストールだったので、vue-cliがインストールされていれば、**1コマンド**のみですね）

デスクトップアプリとして動かした場合でも、Webで動いていた機能が壊れることなく動いたのは驚きました。（Electronの内部ではChromiumが使われているとのことなので、当たり前っちゃ当たり前ですが）

最近はPWAを使ってもデスクトップアプリっぽく見せることができるよう([Windows 10 1803の新機能「PWA」とは？PWAのUWPアプリ化を試してみる](https://codezine.jp/article/detail/10837))ですので、そちらも試していきたいと思います。
