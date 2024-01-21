---
title: "(補足資料)Face APIのキーとエンドポイントを発行する"
---

Face API は登録した人しか使えないため、キーとなる情報を発行する必要があります。
以下のその手順を記載します。

# Azureポータルにログインする

Azure ポータルを開きます。

https://portal.azure.com/

ログインし、以下のようなページが表示されれば OK です。

![https://i.gyazo.com/9c41bc6e713573818c8a2086a6e204be.png](https://i.gyazo.com/9c41bc6e713573818c8a2086a6e204be.png)

Face API のリソースを作成します。

![https://i.gyazo.com/506f1466fe43354518e43cc5a3bd24fa.png](https://i.gyazo.com/506f1466fe43354518e43cc5a3bd24fa.png)

![https://i.gyazo.com/5201c4d4037788c001d79f1cdfef8725.png](https://i.gyazo.com/5201c4d4037788c001d79f1cdfef8725.png)

# リソースグループを作成する

> 既にリソースグループを作成している方は作成不要です

リソースグループの`新規作成`をクリックし、リソースグループ名`handson-(今日の年月日)`を入力します。

![https://i.gyazo.com/cd8c999ccca54a34eba230c44e926a48.png](https://i.gyazo.com/cd8c999ccca54a34eba230c44e926a48.png)

![https://i.gyazo.com/199465814a832fa28eb3c97955c9eb48.png](https://i.gyazo.com/199465814a832fa28eb3c97955c9eb48.png)

リソースグループが作成されました。

![https://i.gyazo.com/c9cb77de2d96a1974c24eaed0b967e9c.png](https://i.gyazo.com/c9cb77de2d96a1974c24eaed0b967e9c.png)

# Face APIのリソースを作成する

残りの項目を埋めて、`確認および作成`をクリックします。

| 項目 | 値 | 備考 |
| -- | -- | -- |
| リージョン | 東日本 | 一番近い`東日本`を選択します。 |
| 名前 | `handson-{名前}-20210619` | ユニークな名前をつける必要があります。名前と日付を入れましょう。 |
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

これでキーとエンドポイントを発行できました。
