#### 企業での事例

- JR 東日本 Chat Bot（[https://info.jreast-chat.com/](https://info.jreast-chat.com/)）
- ヤマト運輸（[https://www.kuronekoyamato.co.jp/ytc/campaign/renkei/LINE/](https://www.kuronekoyamato.co.jp/ytc/campaign/renkei/LINE/)）
- ユニクロ・GU（[http://official-blog.line.me/ja/archives/28533966.html](http://official-blog.line.me/ja/archives/28533966.html)）
- 楽天（[http://official-blog.line.me/ja/archives/24736939.html](http://official-blog.line.me/ja/archives/24736939.html)）など

#### 行政での事例

https://linecorp.com/ja/csr/activity/government

### LINE Bot の仕組み

LINE Bot は、ユーザーが LINE 公式アカウントに対して送信したメッセージに対し、
Messaging API を介してメッセージや画像などをリプライすることで実装されます。

![](https://p.ipic.vip/gw6757.png)

https://developers.line.biz/ja/docs/messaging-api/overview/

### Messaging API

大きく分けて 2 種類の機能があります。

- 応答メッセージ
- プッシュメッセージ

#### 応答メッセージ

- ユーザーが LINE 公式アカウントに対して送信したメッセージに返信できる
- 無料で利用できる

#### プッシュメッセージ

- 任意のタイミングでユーザーに送信できるメッセージ
- ユーザーに通知したいとき利用される
- コミュニケーションプランでは、月 200 通まで無料（2024/04 時点）
  - https://www.lycbiz.com/jp/service/line-official-account/plan/
