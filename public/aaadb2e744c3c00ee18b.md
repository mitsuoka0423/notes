---
title: Azure IoT Centralにシミュレートしたデバイスを追加する手順メモ
tags:
  - Azure
  - AzureIoTHub
  - AzureIoTCentral
private: false
updated_at: '2021-02-09T22:08:06+09:00'
id: aaadb2e744c3c00ee18b
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに

公式ドキュメント：[クイック スタート:IoT Central アプリケーションにシミュレートされたデバイスを追加する(https://docs.microsoft.com/ja-jp/azure/iot-central/core/quick-create-simulated-device)](https://docs.microsoft.com/ja-jp/azure/iot-central/core/quick-create-simulated-device)
の手順を実施した際のメモです。

実際のデバイスは用意せず、仮想のデバイスを追加することができるシミュレートという機能があるので、それを利用します。
数秒おきに乱数で生成したデータをテレメトリとして送信してくれます。
デバイスの準備が追いついていないときに便利ですね。

## デバイステンプレートを追加する

<a href="https://gyazo.com/712781983d2a10e3945b1e9781de89f1"><img src="https://i.gyazo.com/712781983d2a10e3945b1e9781de89f1.png" alt="Image from Gyazo" width="1118"/></a>

<a href="https://gyazo.com/7ed4967280984a85f3c1c0d85aa82f89"><img src="https://i.gyazo.com/7ed4967280984a85f3c1c0d85aa82f89.png" alt="Image from Gyazo" width="1118"/></a>
`ESP32-Azure IoT Kit`を選択します。
（`Ctrl + F`で検索すると見つけられます。）

<a href="https://gyazo.com/f179bfdb217ee2383d3358ee0376c746"><img src="https://i.gyazo.com/f179bfdb217ee2383d3358ee0376c746.png" alt="Image from Gyazo" width="1118"/></a>

<a href="https://gyazo.com/91181735524af75e1dcd6cdb2424d57f"><img src="https://i.gyazo.com/91181735524af75e1dcd6cdb2424d57f.png" alt="Image from Gyazo" width="1118"/></a>
こんな画面になればOK。

## クラウドプロパティを追加する

<a href="https://gyazo.com/31e2648a399ec7d384ed2d02c02396f6"><img src="https://i.gyazo.com/31e2648a399ec7d384ed2d02c02396f6.png" alt="Image from Gyazo" width="1118"/></a>

## ビューを作成する

まずは、デバイス管理用のフォームを作成します。

<a href="https://gyazo.com/5e60cf338b1d9a0ce39ebc1e63b8f73a"><img src="https://i.gyazo.com/5e60cf338b1d9a0ce39ebc1e63b8f73a.png" alt="Image from Gyazo" width="1118"/></a>

<a href="https://gyazo.com/e1def6e8a5592efce651fe1ed3b841a2"><img src="https://i.gyazo.com/e1def6e8a5592efce651fe1ed3b841a2.png" alt="Image from Gyazo" width="1118"/></a>
必要なものを選択します。

<a href="https://gyazo.com/78c689019bac5760a499f51202494bf6"><img src="https://i.gyazo.com/78c689019bac5760a499f51202494bf6.png" alt="Image from Gyazo" width="1124"/></a>

<a href="https://gyazo.com/5c80f4a273b4cd234378d1cf4b102ec7"><img src="https://i.gyazo.com/5c80f4a273b4cd234378d1cf4b102ec7.png" alt="Image from Gyazo" width="1124"/></a>

<a href="https://gyazo.com/aaa8df4d026aaa36235eb4b59083678e"><img src="https://i.gyazo.com/aaa8df4d026aaa36235eb4b59083678e.png" alt="Image from Gyazo" width="1124"/></a>
公開すると、デバイスページに表示されます。

デバイスから送信されたデータ（テレメトリクス）を表示するダッシュボードを作成します。

## IoTデバイスを追加する

<a href="https://gyazo.com/3bc7d8f3ec5ef3070d50d665eb322180"><img src="https://i.gyazo.com/3bc7d8f3ec5ef3070d50d665eb322180.png" alt="Image from Gyazo" width="1124"/></a>

<a href="https://gyazo.com/bd3794d09361230b06da7ae8656368ff"><img src="https://i.gyazo.com/bd3794d09361230b06da7ae8656368ff.png" alt="Image from Gyazo" width="1124"/></a>
`このデバイスをシミュレートしますか？`は`はい`を選択します。
シミュレートをONにすると、実際のデバイスを用意しなくても、乱数で生成したデータを数秒おきに送信してくれるため、クラウド側の開発を進めることができます。

<a href="https://gyazo.com/9824b6f4c1cf91c4c0353d1de54de6ec"><img src="https://i.gyazo.com/9824b6f4c1cf91c4c0353d1de54de6ec.png" alt="Image from Gyazo" width="1124"/></a>

<a href="https://gyazo.com/5ec0559b07fe2e25f89cde227c488935"><img src="https://i.gyazo.com/5ec0559b07fe2e25f89cde227c488935.png" alt="Image from Gyazo" width="1124"/></a>
デバイスごとにダッシュボードが作成され、デバイスから送信されたデータを確認することができます。

<a href="https://gyazo.com/3b270b0d1a4e563c19788784e89afda2"><img src="https://i.gyazo.com/3b270b0d1a4e563c19788784e89afda2.png" alt="Image from Gyazo" width="1600"/></a>
生データタブで、送信されたデータのリストを確認することができます。

## ハマったところ

### デバイステンプレートが表示されない：公開しましょう

デバイステンプレートを作成しても、デバイス作成時に一覧に表示されないことがありました。
公開し忘れが原因でしたので、忘れずに公開しましょう。
