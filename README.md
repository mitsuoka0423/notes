# Articles


## 拡張機能

アカウント作成説明など説明記事を再利用するために利用しています。

https://marketplace.visualstudio.com/items?itemName=poyonshot.markdowncat


## 依存関係

### 現状

```mermaid
graph
  articles -->|MarkdownCat| parts
  books -->|MarkdownCat| parts
```

### 理想像

```mermaid
graph
  qiita
  subgraph zenn
    articles
    books
  end
  parts

  qiita -->|MarkdownCat| parts
  articles -->|MarkdownCat| parts
  books -->|MarkdownCat| parts
```