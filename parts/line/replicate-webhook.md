
### 完成イメージ

#### シナリオ

![image](https://i.imgur.com/bOjD9OH.png)

> シナリオが 2 つになります。

### スプレッドシートを作成する

#### 前提

- [スプレッドシート](https://docs.google.com/spreadsheets/u/0/) で操作

#### 操作イメージ

![image](https://i.imgur.com/nUT6yP1.png)

![image](https://i.imgur.com/xdrgYuE.png)

> - A1： `Replicate ID`
> - B1： `LINE ID`
>
> となっていれば OK


### `Google Sheets` モジュールの `Add a Row` を設定する

#### 前提

- [Make](https://www.Make.com/en/login) で操作

#### 操作イメージ

![image](https://i.imgur.com/fe5XFqF.png)

![image](https://i.imgur.com/pxbX3RK.png)

![image](https://i.imgur.com/MbX3lW2.png)

![image](https://i.imgur.com/gUPFTmC.png)

> 先ほど作成したスプレッドシートを選ぶ

![image](https://i.imgur.com/DImLUoP.png)

![image](https://i.imgur.com/wHSqKeF.png)

![image](https://i.imgur.com/HbYBp1u.png)

![image](https://i.imgur.com/aiBX5Qz.png)


### 新しいシナリオを作成する

#### 前提

- [Make](https://www.Make.com/en/login) で操作

#### 操作イメージ

![image](https://i.imgur.com/X81Dttj.png)

![image](https://i.imgur.com/0oMMz8f.png)


### `Webhooks` モジュールの `Custom webhook` を設定する

#### 前提

- 新しく作成したシナリオを変更

#### 操作イメージ

![image](https://i.imgur.com/qdMeMy9.png)

> Webhook URL をコピーできていれば OK


### `HTTP` モジュールの `Make a request` を変更する

#### 前提

- **最初に作ったシナリオを変更**

#### 操作イメージ

![image](https://i.imgur.com/bXd1MoE.png)

![image](https://i.imgur.com/tUYzMrh.png)

> `webhook` と `webhook_events_filter` を追加する

```diff json
{
    "version": "3f0457e4619daac51203dedb472816fd4af51f3149fa7a9e0b5ffcf1b8172438",
    "input": {
      "cond_aug": 0.02,
      "decoding_t": 7,
      "input_image": "{{10.image.url}}",
      "video_length": "14_frames_with_svd",
      "sizing_strategy": "maintain_aspect_ratio",
      "motion_bucket_id": 127,
      "frames_per_second": 6
-    }
+    },
+    "webhook": "{{上の手順でコピーした Webhook URL を貼り付ける}}",
+    "webhook_events_filter": ["completed"]
  }
```

![image](https://i.imgur.com/Loyy3J3.png)

![image](https://i.imgur.com/E41DlcU.png)

![image](https://i.imgur.com/jhMILzf.png)

> `SCHEDULING` が `ON` になっていれば OK


### `Google Sheets` モジュールの `Search Rows` を設定する

#### 前提

- 新しく作成したシナリオを変更

#### 操作イメージ

![image](https://i.imgur.com/RxvUHPM.png)

![image](https://i.imgur.com/N9bM38Z.png)

![image](https://i.imgur.com/MNVNrum.png)

> `id` は `Webhooks` の `id` を選択


### `LINE` モジュールの `Send a Push Message` を設定する

#### 前提

- 新しく作成したシナリオを変更

#### 操作イメージ

![image](https://i.imgur.com/ROleNk8.png)

![image](https://i.imgur.com/KWZq1I1.png)

![image](https://i.imgur.com/HvlHiyX.png)

![image](https://i.imgur.com/v1MMReO.png)

![image](https://i.imgur.com/E41DlcU.png)

![image](https://i.imgur.com/jhMILzf.png)

> `SCHEDULING` が `ON` になっていれば OK
