---
title: "GuzzleHttpクライアントではPromise\allを使う際に同じClientを使わないと並列リクエストされない #PHP"
emoji: "👻"
type: "tech"
topics: ["PHP", "Guzzle", "Promise"]
published: false
---

## はじめに

[GuzzleHttp](https://github.com/guzzle/guzzle) の非同期リクエストで `Promise\all` を使用した際、期待通りに並列実行されず、直列実行になってしまう事象に出会いました。

以下の記事で言及されています。

https://blog.takekoshi.net/guzzle-async-client-serial/

この記事では、上記で示されている「リクエストごとに`Client`インスタンスを生成すると直列実行になる」という事象の追試を行います。

具体的には、事象を再現するコードと、記事で示唆されている解決策（`Client`をシングルトンで管理する）を実際に動かして、本当に並列実行されるようになるのかを計測・確認します。

## 事象が再現するコード

最初に、参考記事で問題として指摘されている「リクエストごとに`new Client()`する」コードを見ていきます。
このコードは、複数のリクエストを `Promise` の配列に格納し、`Utils::all($promises)->wait()` で一括して実行しようとしています。

```php
// リクエストごとに新しいClientを生成するパターン
measure_execution_time('New Client Each Request Execution', function () use ($logger) {
    $promises = [];
    for ($i = 1; $i <= NUM_OF_TRIALS; $i++) {
        $stack = HandlerStack::create();
        $stack->push(LogMiddleware::create($logger));
        $client = new Client(['handler' => $stack]);
        $delay = $i * DELAY_COEFFICIENT;
        $promises[] = $client->getAsync(TEST_SERVER_BASE_URL . "?delay={"
    }
    Utils::all($promises)->wait();
});
```

### 実行結果

このコードを実行した際のログは以下の通りです。

```plaintext
--- New Client Each Request Execution ---
[2025-09-13T15:39:53.135P] [LOG] --- New Client Each Request Execution ---
[2025-09-13T15:39:53.143P] [LOG] [before] Request: GET http://localhost:8000/?delay=0.25
[2025-09-13T15:39:53.146P] [LOG] [before] Request: GET http://localhost:8000/?delay=0.5
[2025-09-13T15:39:53.147P] [LOG] [before] Request: GET http://localhost:8000/?delay=0.75
[2025-09-13T15:39:53.147P] [LOG] [before] Request: GET http://localhost:8000/?delay=1
[2025-09-13T15:39:53.148P] [LOG] [before] Request: GET http://localhost:8000/?delay=1.25
[2025-09-13T15:39:53.148P] [LOG] [before] Request: GET http://localhost:8000/?delay=1.5
[2025-09-13T15:39:53.148P] [LOG] [before] Request: GET http://localhost:8000/?delay=1.75
[2025-09-13T15:39:53.148P] [LOG] [before] Request: GET http://localhost:8000/?delay=2
[2025-09-13T15:39:53.148P] [LOG] [before] Request: GET http://localhost:8000/?delay=2.25
[2025-09-13T15:39:53.148P] [LOG] [before] Request: GET http://localhost:8000/?delay=2.5
[2025-09-13T15:39:53.402P] [LOG] [after] Request: GET http://localhost:8000/?delay=0.25
[2025-09-13T15:39:53.402P] [LOG] [after] Response: 200 OK
[2025-09-13T15:39:53.904P] [LOG] [after] Request: GET http://localhost:8000/?delay=0.5
[2025-09-13T15:39:53.904P] [LOG] [after] Response: 200 OK
[2025-09-13T15:39:54.656P] [LOG] [after] Request: GET http://localhost:8000/?delay=0.75
[2025-09-13T15:39:54.656P] [LOG] [after] Response: 200 OK
[2025-09-13T15:39:55.659P] [LOG] [after] Request: GET http://localhost:8000/?delay=1
[2025-09-13T15:39:55.659P] [LOG] [after] Response: 200 OK
[2025-09-13T15:39:56.920P] [LOG] [after] Request: GET http://localhost:8000/?delay=1.25
[2025-09-13T15:39:56.920P] [LOG] [after] Response: 200 OK
[2025-09-13T15:39:58.422P] [LOG] [after] Request: GET http://localhost:8000/?delay=1.5
[2025-09-13T15:39:58.422P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:00.174P] [LOG] [after] Request: GET http://localhost:8000/?delay=1.75
[2025-09-13T15:40:00.174P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:02.178P] [LOG] [after] Request: GET http://localhost:8000/?delay=2
[2025-09-13T15:40:02.178P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:04.434P] [LOG] [after] Request: GET http://localhost:8000/?delay=2.25
[2025-09-13T15:40:04.434P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:06.935P] [LOG] [after] Request: GET http://localhost:8000/?delay=2.5
[2025-09-13T15:40:06.936P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:06.936P] [LOG] Execution Time: 13.801441907883 seconds
[2025-09-13T15:40:06.936P] [LOG] 想定処理時間: 13.75
```

`[before]` のログは一気に出力されますが、`[after]` のログでは `Request` と `Response` が1つずつペアで順番に出力されています。これは、各リクエストの完了を待ってから次の処理に進んでいることを示しており、実質的に「直列実行」であることを示唆します。

この直列実行は実行時間にも反映されています。並列処理であれば、最も遅延の大きいリクエスト（2.5秒）とほぼ同じ時間で完了するはずですが、実際の実行時間は約13.8秒です。
これは各リクエストの遅延時間の合計（0.25 + 0.5 + ... + 2.5 = 13.75秒）とほぼ一致します。

## 提案されている対応策の検証

次に、参考元の記事で示唆されている「`Client`をシングルトンで管理する」という方法を試します。

まず、`Client`をシングルトンで提供する `HttpClient` クラスを用意します。

```php
// src/HttpClient.php
namespace App;

use GuzzleHttp\Client;
use GuzzleHttp\HandlerStack;

class HttpClient
{
    private static $instance;

    private function __construct() {}

    public static function getInstance(): Client
    {
        if (self::$instance === null) {
            $stack = HandlerStack::create();
            $stack->push(LogMiddleware::create(new Logger()));
            self::$instance = new Client(['handler' => $stack]);
        }
        return self::$instance;
    }
}
```

そして、この `HttpClient::getInstance()` を使ってリクエストを送信するコードがこちらです。

```php
// シングルトンなClientで並列実行するパターン
$client = HttpClient::getInstance();
measure_execution_time('Parallel Execution', function () use ($client) {
    $promises = [];
    for ($i = 1; $i <= NUM_OF_TRIALS; $i++) {
        $delay = $i * DELAY_COEFFICIENT;
        $promises[] = $client->getAsync(TEST_SERVER_BASE_URL . "?delay={"
    }
    Utils::all($promises)->wait();
});
```

### 実行結果

シングルトンパターンで実行した結果は、以下の通りです。

```plaintext
[2025-09-13T15:40:20.719P] [LOG] --- Parallel Execution ---
[2025-09-13T15:40:20.720P] [LOG] [before] Request: GET http://localhost:8000/?delay=0.25
[2025-09-13T15:40:20.720P] [LOG] [before] Request: GET http://localhost:8000/?delay=0.5
[2025-09-13T15:40:20.720P] [LOG] [before] Request: GET http://localhost:8000/?delay=0.75
[2025-09-13T15:40:20.720P] [LOG] [before] Request: GET http://localhost:8000/?delay=1
[2025-09-13T15:40:20.720P] [LOG] [before] Request: GET http://localhost:8000/?delay=1.25
[2025-09-13T15:40:20.720P] [LOG] [before] Request: GET http://localhost:8000/?delay=1.5
[2025-09-13T15:40:20.720P] [LOG] [before] Request: GET http://localhost:8000/?delay=1.75
[2025-09-13T15:40:20.720P] [LOG] [before] Request: GET http://localhost:8000/?delay=2
[2025-09-13T15:40:20.720P] [LOG] [before] Request: GET http://localhost:8000/?delay=2.25
[2025-09-13T15:40:20.721P] [LOG] [before] Request: GET http://localhost:8000/?delay=2.5
[2025-09-13T15:40:20.973P] [LOG] [after] Request: GET http://localhost:8000/?delay=0.25
[2025-09-13T15:40:20.973P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:21.224P] [LOG] [after] Request: GET http://localhost:8000/?delay=0.5
[2025-09-13T15:40:21.224P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:21.474P] [LOG] [after] Request: GET http://localhost:8000/?delay=0.75
[2025-09-13T15:40:21.474P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:21.724P] [LOG] [after] Request: GET http://localhost:8000/?delay=1
[2025-09-13T15:40:21.724P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:21.974P] [LOG] [after] Request: GET http://localhost:8000/?delay=1.25
[2025-09-13T15:40:21.974P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:22.224P] [LOG] [after] Request: GET http://localhost:8000/?delay=1.5
[2025-09-13T15:40:22.224P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:22.475P] [LOG] [after] Request: GET http://localhost:8000/?delay=1.75
[2025-09-13T15:40:22.475P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:22.725P] [LOG] [after] Request: GET http://localhost:8000/?delay=2
[2025-09-13T15:40:22.725P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:22.976P] [LOG] [after] Request: GET http://localhost:8000/?delay=2.25
[2025-09-13T15:40:22.976P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:23.229P] [LOG] [after] Request: GET http://localhost:8000/?delay=2.5
[2025-09-13T15:40:23.229P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:23.229P] [LOG] Execution Time: 2.5095121860504 seconds
[2025-09-13T15:40:23.229P] [LOG] 想定処理時間: 2.5
```

`[before]`のログが一気に出力された後、`[after]`のログも各リクエストの完了を待たずに、ほぼ同時に出力されています。

実行時間は約2.5秒であり、これは最も遅延の大きいリクエスト（2.5秒）の完了時間とほぼ一致します。これらの結果から、すべてのリクエストが並列で処理され、全体の実行時間が最も時間のかかる単一のリクエスト時間に収まっていることが確認できます。

## 比較: 純粋な直列実行の場合

参考として、`wait()`をループ内で呼び出す、純粋な直列実行のコードも見てみましょう。

```php
// 比較用の直列実行
$client = HttpClient::getInstance();
measure_execution_time('Sequential Execution', function () use ($client) {
    for ($i = 1; $i <= NUM_OF_TRIALS; $i++) {
        $delay = $i * DELAY_COEFFICIENT;
        $client->getAsync(TEST_SERVER_BASE_URL . "?delay={"
    }
});
```

### 実行結果

このコードは期待通り直列で実行されます。

```plaintext
[2025-09-13T15:40:06.937P] [LOG] --- Sequential Execution ---
[2025-09-13T15:40:06.937P] [LOG] [before] Request: GET http://localhost:8000/?delay=0.25
[2025-09-13T15:40:07.189P] [LOG] [after] Request: GET http://localhost:8000/?delay=0.25
[2025-09-13T15:40:07.189P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:07.190P] [LOG] [before] Request: GET http://localhost:8000/?delay=0.5
[2025-09-13T15:40:07.691P] [LOG] [after] Request: GET http://localhost:8000/?delay=0.5
[2025-09-13T15:40:07.692P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:07.692P] [LOG] [before] Request: GET http://localhost:8000/?delay=0.75
[2025-09-13T15:40:08.443P] [LOG] [after] Request: GET http://localhost:8000/?delay=0.75
[2025-09-13T15:40:08.443P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:08.443P] [LOG] [before] Request: GET http://localhost:8000/?delay=1
[2025-09-13T15:40:09.445P] [LOG] [after] Request: GET http://localhost:8000/?delay=1
[2025-09-13T15:40:09.445P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:09.446P] [LOG] [before] Request: GET http://localhost:8000/?delay=1.25
[2025-09-13T15:40:10.699P] [LOG] [after] Request: GET http://localhost:8000/?delay=1.25
[2025-09-13T15:40:10.699P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:10.699P] [LOG] [before] Request: GET http://localhost:8000/?delay=1.5
[2025-09-13T15:40:12.202P] [LOG] [after] Request: GET http://localhost:8000/?delay=1.5
[2025-09-13T15:40:12.202P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:12.202P] [LOG] [before] Request: GET http://localhost:8000/?delay=1.75
[2025-09-13T15:40:13.955P] [LOG] [after] Request: GET http://localhost:8000/?delay=1.75
[2025-09-13T15:40:13.955P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:13.955P] [LOG] [before] Request: GET http://localhost:8000/?delay=2
[2025-09-13T15:40:15.958P] [LOG] [after] Request: GET http://localhost:8000/?delay=2
[2025-09-13T15:40:15.958P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:15.958P] [LOG] [before] Request: GET http://localhost:8000/?delay=2.25
[2025-09-13T15:40:18.215P] [LOG] [after] Request: GET http://localhost:8000/?delay=2.25
[2025-09-13T15:40:18.215P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:18.215P] [LOG] [before] Request: GET http://localhost:8000/?delay=2.5
[2025-09-13T15:40:20.716P] [LOG] [after] Request: GET http://localhost:8000/?delay=2.5
[2025-09-13T15:40:20.717P] [LOG] [after] Response: 200 OK
[2025-09-13T15:40:20.717P] [LOG] Execution Time: 13.779940128326 seconds
[2025-09-13T15:40:20.718P] [LOG] 想定処理時間: 13.75
```

## 実行結果のまとめ

3つのパターンの実行時間を比較した結果が以下になります。

- **New Client Each Request (問題のコード)**: **13.80秒**
- **Sequential Execution (比較用の直列実行)**: **13.78秒**
- **Parallel Execution (対応策を適用したコード)**: **2.51秒**

「リクエストごとに`new Client()`する」パターンは、「純粋な直列実行」とほぼ同じ実行時間になっています。

## なぜ問題が起きたのか？

では、なぜ `Client` をリクエストごとに生成すると並列実行されなくなってしまうのでしょうか。

その鍵は、GuzzleHttpの内部で非同期処理を支えている **ハンドラ（Handler）** と **コネクションプール** の仕組みにあります。

- **`new Client()` のたびに、新しいハンドラが作られる**: Guzzleの `Client` は、インスタンス化される際に内部でcURLなどを操作するためのハンドラを生成します。このハンドラが、実際のリクエスト実行やイベントループを管理する中心的な役割を担います。
- **ハンドラは独立している**: リクエストごとに `new Client()` すると、その数だけ独立したハンドラが作られます。`Promise\all` は、**単一のハンドラのイベントループ上**で複数のリクエストを効率的にさばくための機能ですが、複数の独立したハンドラにまたがってリクエストを並列化することはできません。
- **シングルトン化でハンドラを共有**: `Client` をシングルトンにすることで、アプリケーション全体でただ一つのハンドラが共有されます。そのため、`Promise\all` に渡されたすべてのPromiseが同じイベントループ上で管理され、効率的な並列実行が可能になります。

端的に言えば、`Promise\all` の並列実行は、単一のハンドラが持つイベントループ上で複数のリクエストを管理することで実現されます。リクエストごとにハンドラを生成することは、この仕組みの恩恵を自ら放棄していることに他なりません。

## まとめ

今回の追試により、GuzzleHttpで `Promise\all` を使って非同期リクエストを行う際には、`Client` インスタンスをシングルトンで管理することが、並列実行を実現する上で非常に重要であることが確認できました。

参考元の記事で言及されていた通り、リクエストごとに `new Client()` すると、意図せず直列実行になってしまうようです。同様の現象に遭遇した際の参考になれば幸いです。

- **原則として、`Client` インスタンスはアプリケーション全体で共有（シングルトン化）する。**
- **リクエストごとに `new Client()` しない。**

Laravelなどのフレームワークを利用している場合は、サービスコンテナに `Client` をシングルトンとして登録するのが一般的なベストプラクティスです。

```php
// AppServiceProvider.phpなど
use GuzzleHttp\Client;

$this->app->singleton(Client::class, function ($app) {
    return new Client(['handler' => ...]);
});
```

## ソースコード

本記事で用いたソースコードの全体は、以下のGitHubリポジトリで確認できます。

https://github.com/mitsuoka0423/php-guzzle-http-client-trial
