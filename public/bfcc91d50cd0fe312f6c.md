---
title: Azure IoT HubにHTTP POSTでメッセージを送信するメモ
tags:
  - Azure
  - IoT
  - AzureIoTHub
  - AzureIoT
private: false
updated_at: '2022-02-23T21:23:02+09:00'
id: bfcc91d50cd0fe312f6c
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに

HTTP POST で Azure IoT Hub にメッセージを送信するメモです。
<a href="https://gyazo.com/edba20c81dc5255370fb7b5e8f6275b4"><img src="https://i.gyazo.com/edba20c81dc5255370fb7b5e8f6275b4.gif" alt="Image from Gyazo" width="1280"/></a>
Node.js & MQTT のメモは[Node.jsでAzure IoT Hubにテレメトリを送信するサンプルコードを動かすメモ](https://qiita.com/tmitsuoka0423/items/bbc5ea0f462752e31c1f)にまとめています。

公式ドキュメントはこちら：[IoT Hub REST](https://docs.microsoft.com/ja-jp/rest/api/iothub/)。
~~欲しい情報が全然見つからなかった~~ので、下記の記事も参考にさせていただきました。
- [Azure IoT Hubを生MQTTS(mosquitto)やHTTP RESTで使う方法](https://qiita.com/ma2shita/items/032370350cba282d35ff#http-rest) (@ma2shita さん)
- [HTTP REST APIでAzure IoT Hubにメッセージを送信する (C#)](https://www.sukerou.com/2018/11/http-rest-apiazure-iot-hub-c201811.html#toc_headline_4)

(2021/02/11 追記)
リファレンス見つかりました。
→[デバイスからメッセージを送信する(https://docs.microsoft.com/ja-jp/rest/api/iothub/device/senddeviceevent)](https://docs.microsoft.com/ja-jp/rest/api/iothub/device/senddeviceevent)

## 準備

- [手順](https://docs.microsoft.com/ja-jp/azure/iot-hub/quickstart-send-telemetry-node#create-an-iot-hub)を参考に、IoT Hub をデプロイする（スケールは`F1`を選択）。

## HTTP POSTでメッセージを送信する

### URL

`https://{IOT_HUB_NAME}.azure-devices.net/devices/{DEVICE_ID}/messages/events?api-version=2018-06-30`

| 項目 | 内容 | 備考 |
| :-: | :-: | :-: |
| `IOT_HUB_NAME` | Azure IoT Hubの名前 | `サイドバー > プロパティ > 名前`から取得可能 |
| `DEVICE_ID` | デバイスID | `サイドバー > IoTデバイス > デバイス一覧 > デバイスID`から取得可能 |

<a href="https://gyazo.com/334ff9b5af64ac3ad9b7c84b5f7664cc"><img src="https://i.gyazo.com/334ff9b5af64ac3ad9b7c84b5f7664cc.png" alt="Image from Gyazo" width="1282"/></a>

<a href="https://gyazo.com/0cd0a9801680fba8c70d5147ea6d9390"><img src="https://i.gyazo.com/0cd0a9801680fba8c70d5147ea6d9390.png" alt="Image from Gyazo" width="1312"/></a>

私の場合、以下のようになります。
`https://azure-iot-m5atom-lite-sample.azure-devices.net/devices/http-rest-client-1/messages/events?api-version=2018-06-30`

### HTTPヘッダーにSASトークンを設定する

[IoT Hub へのアクセスの制御](https://docs.microsoft.com/ja-jp/azure/iot-hub/iot-hub-devguide-security)に SAS トークンの発行方法が書いてあります。
今回は、Azure CLI を使用してみます。

#### Azure CLIのインストール

[Azure Command-Line Interface (CLI) documentation](https://docs.microsoft.com/ja-jp/cli/azure/?view=azure-cli-latest)から、Azure CLI をインストールします。

インストールできたらターミナルで`az`コマンドを実行してみましょう。
以下のように表示されれば OK です。

```bash
$az

Welcome to Azure CLI!
---------------------
Use `az -h` to see available commands or go to https://aka.ms/cli.

Telemetry
---------
The Azure CLI collects usage data in order to improve your experience.
The data is anonymous and does not include commandline argument values.
The data is collected by Microsoft.

You can change your telemetry settings with `az configure`.


     /\
    /  \    _____   _ _  ___ _
   / /\ \  |_  / | | | \'__/ _\
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|

(略)
```

忘れずに、Azure CLI でもログインしておきます。

```bash
$ az login
```

#### SASトークンを発行する

ターミナルで以下のコマンドを実行します。

```bash
$ az iot hub generate-sas-token --hub-name {IOT_HUB_NAME}
```

私の場合はこうなります。

```bash
$ az iot hub generate-sas-token --hub-name azure-iot-m5atom-lite-sample
```

実行すると、SAS トークンが発行されました。

```bash
{
  "sas": "SharedAccessSignature sr=azure-iot-m5atom-lite-sample.azure-devices.net&sig=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&se=xxxxxxxxxxxxxxx&skn=iothubowner"
}
```

こちらを HTTP ヘッダーの`Authorization`に設定します。

### POSTする

CURL ではこんな感じのリクエストになります。

```
curl -i -X POST \
   -H "Content-Type:application/json" \
   -H "Authorization:SharedAccessSignature sr=azure-iot-m5atom-lite-sample.azure-devices.net&sig=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&se=xxxxxxxxxxxxxxx&skn=iothubowner" \
   -d '"test"' \
 'https://azure-iot-m5atom-lite-sample.azure-devices.net/devices/http-rest-client-1/messages/events?api-version=2018-06-30'
```

動作確認します。
[前記事](https://qiita.com/tmitsuoka0423/items/bbc5ea0f462752e31c1f#%E9%80%81%E4%BF%A1%E3%81%97%E3%81%9F%E3%83%87%E3%83%BC%E3%82%BF%E3%82%92%E5%8F%97%E4%BF%A1%E3%81%99%E3%82%8B)で作成した Node.js の受信プログラムを動かしておき、きちんと受信できるか確認します。

<a href="https://gyazo.com/edba20c81dc5255370fb7b5e8f6275b4"><img src="https://i.gyazo.com/edba20c81dc5255370fb7b5e8f6275b4.gif" alt="Image from Gyazo" width="1280"/></a>
メッセージを送信するごとに受信できてますね。

## まとめ

Azure IoT Hub に HTTP POST でメッセージを送ることができました。
公式ドキュメントが読み解きにくくてちょっと迷いました。

メッセージを送る方法として、MQTT と HTTP POST はメリットデメリットがありそうですが、よく検討して使っていこうと思います。
