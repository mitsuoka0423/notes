---
title: Auth0を使ってプライベートなマークダウンエディタを作る（クライアントサイド編）
tags:
  - Markdown
  - Vue.js
  - Auth0
  - protooutStudio
private: false
updated_at: '2019-12-22T22:56:12+09:00'
id: 9d55cfdb9bee7eef0b81
organization_url_name: null
slide: false
ignorePublish: false
---
## 認証ってめんどくさい

私は仕事で AWS を使用することが多いのですが、AWS の認証認可サービスといえば`Cognito`です。
一度、調査で Google とのフェデレーションを試してみたのですが、なかなか手順が複雑で大変でした。

最近 Auth0 というサービスを知り、Google アカウントでのログインを爆速で実装できることを知りました。
今回はブラウザ上で動作するマークダウンエディタで、アカウントごとに内容を記録できるものを作成していきます。

本記事では、クライアントサイドの実装を行っていきます。
`所要時間30分～40分`ほどでできる内容ですので試してみてください。

## マークダウンエディタの要件

- ブラウザで使用する
- PC、iPhone 両方で使用できる
- ログインしておくとテキストを記録でき、次回ログイン時には記録した内容が表示される

## システム構成

全体像はこんな感じ。
![all.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/e32aac48-e7a5-d2a0-2bb5-b5eae1e54e46.png)

最近は`サーバーレス`ならぬ`Lambdaless`という考え方があるらしいので、Lambda はできるだけ使わない方針とします。
(参考：[https://www.slideshare.net/mooyoul/lambdaless-and-aws-cdk-191793017](https://www.slideshare.net/mooyoul/lambdaless-and-aws-cdk-191793017)）。
認証情報の検証は仕方ないので Lambda を使います。

この記事では、左側のクライアントサイドを作成していきます。
![part.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/4eb3efbc-e53b-9e4e-7fe4-476a3e161d73.png)



## Auth0サインアップ ～ サンプルソースダウンロード ～ サンプルアプリ起動 (所要時間：15分)

[公式サイト(https://auth0.com/jp/)](https://auth0.com/jp/)を開き、右上にある`サインアップ`ボタンからアカウントを作成します。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/38f644bb-f257-c5db-be7b-16abce64f5c1.png)

私は GitHub アカウントを使用したので、`SIGN UP WITH GITHUB`からサインアップしました。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/28bd2010-db18-f715-657d-3f505954758a.png)

ログインするとダッシュボードが開きます。
`CREATE APPLICATION`からアプリを作成していきます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/e6be567d-c3c7-6d16-39e9-5cf045e17849.png)

今回は Vue.js を使用した SPA として作成するので、以下の内容でアプリを作成しました。

| 項目 | 内容 |
| --- | --- |
| Name | My App |
| Choose an application type | Single Page Web Applications |

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/4f4ef90f-1a44-11c2-c7a0-fb4a3dff73a3.png)

アプリを作成すると、以下のような画面になるので、`Vue.js`をクリックします。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/9033971f-b00c-616d-94d8-16bf838b96b1.png)

右下にある`DOWNLOAD SAMPLE`をクリックし、サンプルアプリをダウンロードします。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/922f8653-5748-4045-37a5-95c3fab9dd72.png)

zip を解凍して中身を確認するとこのような感じ。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/38407ead-c03f-0657-b8f9-d277ab7040e5.png)

サンプルアプリを起動してみます。

```bash
$ npm install
$ npm run serve
```

を実行します。


```bash
  ～～～ 略 ～～～
  App running at:
  - Local:   http://localhost:3000/ 
  - Network: http://192.168.10.2:3000/

  Note that the development build is not optimized.
  To create a production build, run npm run build.
```

と表示されたら、[http://localhost:3000/](http://localhost:3000/)にアクセスします。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/70b4071d-31c1-396d-66ca-c1eddac55225.png)

こんな画面が表示されたら OK。
ここまで順調にいけば初見でも 15 分もあればいけちゃいます。

## Googleでログインする (所要時間：1分)

先ほど起動したサンプルアプリで、Google アカウントでログインしてみましょう。
右上の`Login`ボタンをクリック。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/b629f187-c8ca-79b8-1a00-34f44c0e5468.png)

すると、Auth0 のログイン画面が表示されます。
`LOG IN WITH GOOGLE`をクリックします。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/e96ed580-1183-c18a-1eb2-ef2d27fb98c9.png)

お、Google のログイン画面が現れました！
メールアドレスとパスワードを入力し、ログインします。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/53e43367-6fa3-16a0-74d7-1b9b36d18981.png)

ログインできました！
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/ecf80fc1-dbe5-3f50-73d0-7e181d0d1e52.png)

アイコンをクリックすると、Gmail で登録している名前も表示されます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/2f389eb6-335d-0476-0837-446dbe997c9a.png)

## マークダウンエディタの作成 (慣れた人なら15分)

[https://jp.vuejs.org/v2/examples/index.html](https://jp.vuejs.org/v2/examples/index.html)にいい感じのサンプルソースがあるので流用します。

使用する npm モジュールをインストールします。

```bash
$ npm install lodash marked
```

以下の資源を追加します。

```html:src/views/Editor.vue
<template>
  <div id="editor">
    <textarea :value="input" @input="update"></textarea>
    <div v-html="compiledMarkdown"></div>
  </div>
</template>

<script>
import _ from 'lodash';
import marked from 'marked';

export default {
  name: 'editor',
  data() {
    return {
      input: '# hello',
    }
  },
  computed: {
    compiledMarkdown: function () {
      return marked(this.input, { sanitize: true })
    }
  },
  methods: {
    update: _.debounce(function (e) {
      this.input = e.target.value
    }, 300)
  },
}
</script>

<style scoped>
html, body, #editor {
  margin: 0;
  height: 100%;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  color: #333;
}

textarea, #editor div {
  display: inline-block;
  width: 49%;
  height: 100%;
  vertical-align: top;
  box-sizing: border-box;
  padding: 0 20px;
}

textarea {
  border: none;
  border-right: 1px solid #ccc;
  resize: none;
  outline: none;
  background-color: #f6f6f6;
  font-size: 14px;
  font-family: 'Monaco', courier, monospace;
  padding: 20px;
}

code {
  color: #f66;
}
</style>
```

```javascript:src/router.js
import Vue from "vue";
import Router from "vue-router";
import Home from "./views/Home.vue";
import Profile from "./views/Profile.vue";
import Editor from "./views/Editor.vue"; // <-- この行を追加
import { authGuard } from "./auth";

Vue.use(Router);

const router = new Router({
  mode: "history",
  base: process.env.BASE_URL,
  routes: [
    {
      path: "/",
      name: "home",
      component: Home
    },
    {
      path: "/profile",
      name: "profile",
      component: Profile,
      beforeEnter: authGuard
    },
    {                                     // <-- ここから
      path: "/editor",
      name: "editor",
      component: Editor,
      beforeEnter: authGuard
    }                                     // <-- ここまで追加
  ]
});

export default router;
```

```javascript:src/main.js
import Vue from "vue";
import App from "./App.vue";
import router from "./router";
import { Auth0Plugin } from "./auth";
import HighlightJs from "./directives/highlight";

import { library } from "@fortawesome/fontawesome-svg-core";
import { faLink, faUser, faPowerOff } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { domain, clientId } from "../auth_config.json";

Vue.config.productionTip = false;

Vue.use(Auth0Plugin, {
  domain,
  clientId,
  onRedirectCallback: () => {
    // router.push(
    //   appState && appState.targetUrl
    //     ? appState.targetUrl
    //     : window.location.pathname
    // );
    router.push('editor'); // <-- このように修正（ログイン後、/editorに移動する）
  }
});

Vue.directive("highlightjs", HighlightJs);

library.add(faLink, faUser, faPowerOff);
Vue.component("font-awesome-icon", FontAwesomeIcon);

new Vue({
  router,
  render: h => h(App)
}).$mount("#app");
```

資源の追加・修正が終わると、VSCode を使っている方はこんな感じになるはずです。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/2bf647f0-d69d-badb-bcdd-1439fca4f7b9.png)


[http://localhost:3000/](http://localhost:3000/)にアクセスしログインすると以下のような画面になり、右側にプレビューが表示されます。
（Qiita のエディタみたいですね）

![2019-12-12_23h28_25.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/90087/07f37654-6f5c-dc4e-38d3-df6d303db2c9.gif)

## Auth0を使うと、クライアントサイドは30分で実装できる

Auth0 がサンプルソースを提供してくれるおかげで、30 分くらいでクライアントサイドの認証の実装を行うことができました。
慣れてくると、単純な導通であれば 10 分そこそこでできるんじゃないでしょうか。
（Cognito を初見で使ったときは 1 日かかりました）。

次回はサーバーサイドの実装を行っていきます。
