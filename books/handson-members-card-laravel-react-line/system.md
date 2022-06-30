---
title: "システム構成"
---

## 利用イメージ

```mermaid
sequenceDiagram
  autonumber
  actor ユーザー
  participant お店
  お店 ->> ユーザー: QRコード表示
  ユーザー ->> お店: 友達登録
  ユーザー ->> お店: 会員カード発行依頼
  お店 ->> ユーザー: 会員カード発行
  ユーザー ->> お店: 会員カード表示
```

## システム構成図

```mermaid
flowchart LR
  スマホ --- LINEアプリ --友達登録--- LINEサーバー --Webhook--- Laravel --- AirTable
  LINEアプリ --LIFF--- React --- Laravel
```
