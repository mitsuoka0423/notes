---
title: LINE Botから送った音声メッセージをClovaで再生する(Node.js)
tags:
  - Node.js
  - AWS
  - lambda
  - linebot
  - Clova
private: false
updated_at: '2018-12-09T13:54:00+09:00'
id: a72c94bee0c5acdfe1a3
organization_url_name: null
slide: false
ignorePublish: false
---
この投稿は[LINEBot&Clova Advent Calendar 2018](https://qiita.com/advent-calendar/2018/linebot)の 7 日目の投稿です。

# はじめに
LINE の音声メッセージ入力で録音した音声を Clova で非同期に再生できると、
いろいろとコミュニケーションの幅が広がるのではないかと思い、サンプルアプリを作成しました。
LINE が提供している[Messaging API](https://developers.line.biz/ja/docs/messaging-api/)と[CEK](https://clova-developers.line.biz/guide/)を使用して簡単に実現できたので、その解説を行います。


# 想定環境と作るもののイメージ
学校がおわり、家でお留守番している子供にが Clova に話しかけると、親があらかじめ録音しておいた伝言を再生します。
![グループ化 7.png](https://qiita-image-store.s3.amazonaws.com/0/90087/84f6101d-444a-14b4-2ed7-08b726feae0c.png)

# 構成要素
- AWS
    - API Gateway
    - Lambda
    - DynamoDB
    - S3(音声メッセージ保存用)
- Node.js
- LINE
    - Messaging API SDK
    - CEK(Clova Extensions Kit)

# 仕組み/システム構成図
![グループ化 6.png](https://qiita-image-store.s3.amazonaws.com/0/90087/3c15a56d-4b8a-c4e4-4a30-4c606ff3a9e2.png)

- 音声メッセージ登録処理：
    - Messaging API の[コンテンツを取得する](https://developers.line.biz/ja/reference/messaging-api/#anchor-fcc6cd7ad966ea05230c564c2be362df80c0739d)を参考に、音声メッセージを m4a として取得。
    - 取得した音声メッセージを S3 に格納する。
    - DynamoDB に S3 の URL を LINE ID と紐づけて格納する。
- 音声メッセージ取得処理：
    - LINE ID をもとに、DynamoDB から S3 に格納されている音声メッセージファイルの URL を取得。
    - CEK の[オーディオコンテンツの再生を指示する](https://clova-developers.line.biz/guide/CEK/Guides/Build_Custom_Extension.md#DirectClientToPlayAudio)を参考に、音声を再生する。

 
# サンプルコード

全ソースは[こちら(mono0423/dengonkun)](https://github.com/mono0423/dengonkun)をご参照ください。
ポイントとなる部分のみをピックアップします。

``` JavaScript:LINEBot_音声ファイル取得部分
exports.handler = async (event) {
  const body = JSON.parse(event.body);

  // 音声ファイルを取得
  const audioData = await fetchAudioMessage(body.events[0].message.id);

  // S3にpublic-readで音声ファイルを格納する
  const paramS3 = {
    ACL: 'public-read',
    Body: audioData,
    Bucket: process.env.AUDIO_BUCKET,
    Key: "audio.m4a",
  };
  await s3.putObject(paramS3).promise(); 

  // DynamoDBにS3のURLをインサートする。
  // (省略)
}

// messageIdをもとに、音声ファイルをバイナリで取得する
const fetchAudioMessage = async (messageId) => {
  return new Promise((resolve, reject) => {
    lineClient.getMessageContent(messageId).then((stream) => {
      const content = [];
      stream
          .on('data', (chunk) => {
            content.push(new Buffer(chunk));
          })
          .on('error', (err) => {
            reject(err);
          })
          .on('end', function() {
            resolve(Buffer.concat(content));
          });
    });
  });
}


```


``` JavaScript:ClovaSkill側ソース
exports.handler = async (event) => {
  const context = new clova.Context(JSON.parse(event.body));
  const requestType = context.requestObject.request.type;
  const requestHandler = clovaSkillHandler.config.requestHandlers[requestType];

  await requestHandler.call(context, context);

  return {
    statusCode: 200,
    body: JSON.stringify(context.responseObject),
  };
};

// Clovaスキル実行時の動作を設定
const clovaSkillHandler = clova.Client.configureSkill()
    .onLaunchRequest(async (responseHelper) => {
      // DynamoからS3のURLを取得し、s3urlに格納(省略)
      const s3url = "urlToAudioFileFromDynamoDB";

      responseHelper.setSimpleSpeech(
          // createSpeechUrlメソッドに音声ファイルへのURLを渡して再生する
          clova.SpeechBuilder.createSpeechUrl(s3url),
      ).endSession();
    });
```

# まとめ
Messaging API と CEK を利用し、非同期な音声メッセージの再生を簡単に実現できました。
Clova スキル作成方法の記事もどんどん増えてきているので、みなさんもスキル作成にチャレンジしてみてはいかがでしょうか。  
明日の記事は[@vui_rie](https://qiita.com/vui_rie)さんの「[LINE Clova＆BOTと生きた2018年
](https://note.mu/r9i9e/n/n5adc171a0bac)」です！
