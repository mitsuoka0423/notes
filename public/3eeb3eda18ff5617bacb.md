---
title: '[PHPStorm]Shelfで並行開発を楽に管理する'
tags:
  - PHP
  - Git
  - PhpStorm
  - IntelliJ
  - JetBrains
private: false
updated_at: '2024-01-28T07:52:27+09:00'
id: 3eeb3eda18ff5617bacb
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに

PHPStorm のバージョン管理のヘルパー機能である、`Shelf`の使い方メモです。

詳細は公式ドキュメントを参照してください。
  → [Gitを使用して複数の機能を同時に処理する](https://pleiades.io/help/phpstorm/work-on-several-features-simultaneously.html)

## PHPStormの使い方シリーズ

- [Local Changesで開発中の資源をグループで管理する](https://qiita.com/tmisuoka0423/items/281e76ac637c62bedf02)

## 作業用フォルダを用意する

まず、今回作業を行うフォルダを用意します。
この記事では、ローカルリポジトリ上で作業を行います。

```zsh
% cd ~/Desktop
% mkdir shelf
% cd shelf
```

ローカルリポジトリを作成します。

```zsh
% git init
% ls -la

drwxr-xr-x@  3 mitsuoka-takahiro  staff   96  7 29 21:19 ./
drwx------@ 14 mitsuoka-takahiro  staff  448  7 29 21:16 ../
drwxr-xr-x   9 mitsuoka-takahiro  staff  288  7 29 21:19 .git/
```

.Git フォルダがあれば OK。

## PHPStormでGitツールウィンドウを表示する

`View > Tool Windows > Git`から開くことができます。
ショートカットはデフォルトで`⌘9`に設定されています。

<img width="600" src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic1.png">

このような感じのウィンドウが開きます。

<img width="600" src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic2.png">

## Shelfで並行開発を楽に管理する

Shelf は`git stash`と似た機能で、ブランチをクリーンにできます。
変更リストとの親和性が高く、合わせて使うことで開発中のファイルをより便利に管理できます。

### 準備

新しいファイルを 3 つ追加し、ステージングします。
ステージングするのは、バージョン管理対象ファイルでないと Local Changes に表示されなからです。

```zsh
% echo hello > a1.txt
% echo hello > a2.txt
% echo hello > b2.txt
% git add .
```

Git ツールウィンドウはこのようになっていると思います。

<img width="600" src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic3.png">

さらに、変更リスト`add feature A`に`a1.txt`と`a2.txt`を入れておきます。

<img src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic4.png">

### ファイルをShelfにしまう

Git ツールウィンドウで`b2.txt`を右クリックし、`Shelf Changes`を選択します。

<img src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic5.png">

コミットメッセージを求められるので、`add feature B`と入力し、`Shelve Changes`をクリックします。

<img src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic6.png">

これで Shelf に`b2.txt`をしまうことができました。
Git ツールウィンドウに`Shelf`タブが表示され、内容を確認できます。

<img src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic7.png">

同時に、作業ブランチ上から`b2.txt`がなくなっていることが確認できます。

<img src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic8.png">

```zsh
% git status

On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   a1.txt
        new file:   a2.txt
```

### Shelfからファイルを取り出す

`Shelf`タブの`add feature B`を右クリック、`Unshelve...`を選択します。

<img src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic9.png">

ウィンドウが表示されるので、変更せず`Unshelve Changes`をクリックします。

<img src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic10.png">

`Local Changes`タブに変更リスト`add feature B`が追加され、`b2.txt`の変更も適用されます。

<img src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic11.png">

### 変更リストをShelfにしまう

ファイルだけでなく、既に作成している変更リストを Shelf にしまうこともできます。
対象の変更リストを右クリック、`Shelve Changes`を選択することで、Shelf に格納されます。

<img src="https://github.com/tmitsuoka0423/qiita/raw/master/phpstorm-shelf/pic12.png">

## まとめ

Shelf を使うことで、変更リストと親和性の高い`git stash`に似た機能を利用できる。
Shelf にしまったファイルは、ワークツリーから削除されるので、作業ブランチをクリーンに保つことができる。

## PHPStormの使い方シリーズ

- [Local Changesで開発中の資源をグループで管理する](https://qiita.com/tmisuoka0423/items/281e76ac637c62bedf02)
